# FPGA-Based Simplified Lattice-Based Post-Quantum Cryptography Engine using NTT Accelerator

## Overview

Post-Quantum Cryptography (PQC) is a new generation of cryptographic algorithms designed to remain secure against attacks from quantum computers. Among the various PQC approaches, **lattice-based cryptography** has gained significant attention due to its strong security guarantees and efficient implementation in both software and hardware. Modern standards such as **CRYSTALS-Kyber**, selected by NIST for post-quantum key encapsulation, rely heavily on polynomial arithmetic accelerated by the **Number Theoretic Transform (NTT)**.

This project presents an **FPGA-based simplified lattice-based Post-Quantum Cryptography engine** implemented entirely in **Verilog HDL**. The design demonstrates the fundamental operations of lattice-based encryption, including **key generation**, **encryption**, **decryption**, and **NTT-based polynomial multiplication**, while integrating UART communication for data transfer and an LCD interface for displaying the encryption status and results.

The implementation is described as **simplified** because it uses a reduced parameter set compared to production-grade lattice-based cryptographic schemes. Specifically, the design employs:

* **Polynomial degree (N) = 8** instead of the typical **N = 256** used in CRYSTALS-Kyber.
* **Modulus (q) = 17** instead of larger prime moduli such as **3329**.
* Simplified polynomial arithmetic and lightweight parameter sizes suitable for FPGA implementation and educational demonstration.

These reduced parameters significantly lower hardware resource utilization, simplify the NTT architecture, and make the complete encryption/decryption flow easier to understand, verify, and implement on resource-constrained FPGA platforms. Although the system does **not provide the security level of standardized PQC algorithms**, it accurately demonstrates the underlying mathematical principles and hardware architecture used in lattice-based cryptographic accelerators.

Overall, this project serves as a proof-of-concept hardware implementation that illustrates how NTT acceleration can improve polynomial computations in lattice-based cryptography while providing a foundation for future migration toward full-scale implementations such as CRYSTALS-Kyber.


---

# Features

* FPGA implementation of a simplified lattice-based PQC algorithm
* Number Theoretic Transform (NTT) based polynomial multiplication
* Inverse NTT for polynomial reconstruction
* Simplified LWE-based key generation
* UART interface for message transmission
* LCD display for real-time encryption status
* Pseudo-random polynomial generation using LFSR
* Modular arithmetic operations
* Memory-bank architecture for polynomial storage
* Hardware implementation entirely in Verilog HDL

---

# Project Architecture

```
                +--------------------+
                |   PC UART GUI      |
                +---------+----------+
                          |
                      UART RX
                          |
                +---------v----------+
                |  Crypto Engine     |
                |--------------------|
                | Key Generation     |
                | Encryption         |
                | Decryption         |
                | NTT Accelerator    |
                +---------+----------+
                          |
          +---------------+----------------+
          |                                |
     Memory Banks                    LCD Display
          |
      UART TX
          |
       PC Output
```

---

# Directory Structure

```
final2_improved/
│
├── crypto_engine.v          # Main encryption/decryption engine
├── top.v                    # Top-level FPGA module
├── bank_mem.v               # Polynomial memory banks
├── butterfly.v              # NTT butterfly unit
├── inverse_butterfly.v      # Inverse butterfly unit
├── twiddle_rom.v            # Forward twiddle factors
├── inverse_twiddle_rom.v    # Inverse twiddle factors
├── address_generator.v      # Address generation for NTT
├── twiddle_index.v          # Twiddle factor indexing
├── mod_add.v                # Modular addition
├── mod_sub.v                # Modular subtraction
├── mod_mul.v                # Modular multiplication
├── lfsr.v                   # Random polynomial generator
├── uart_rx.v                # UART Receiver
├── uart_tx.v                # UART Transmitter
├── lcd_display.v            # LCD Controller
├── tb_keygen_check.v        # Testbench
├── uart_gui.py              # Python UART Interface
└── top.xdc                  # FPGA Constraints
```

---

# Hardware Requirements

* Artix-7 FPGA Development Board
* 24 MHz system clock
* FTDI
* 16×2 LCD Module
* Push Buttons
* LEDs

---

# Software Requirements

* Xilinx Vivado
* Python 3.x
* PySerial
* FPGA Programmer

---

# Working Principle

The proposed system implements a simplified lattice-based Post-Quantum Cryptography (PQC) engine on an FPGA. The complete encryption and decryption process is executed in hardware using Verilog HDL, with the Number Theoretic Transform (NTT) accelerating polynomial multiplication. The operational flow is as follows:

### Step 1: Message Input

The user enters an 8-bit plaintext message through the Python-based UART GUI. The message is transmitted serially to the FPGA using the UART communication interface.

### Step 2: UART Reception

The UART Receiver module converts the incoming serial data into parallel 8-bit data and forwards it to the Crypto Engine. Once a valid byte is received, the controller initiates the cryptographic operations.

### Step 3: Key Generation

The Crypto Engine generates the required secret and public keys. An LFSR (Linear Feedback Shift Register) is used to produce pseudo-random polynomial coefficients representing the secret key and error polynomials. These coefficients are stored in dedicated memory banks for subsequent computations.

### Step 4: Polynomial Preparation

The plaintext message is encoded into polynomial form, while the generated secret, public, and error polynomials are arranged according to the selected lattice parameters (**N = 8**, **q = 17**). These reduced parameters simplify hardware implementation while preserving the essential computational flow of lattice-based cryptography.

### Step 5: Forward Number Theoretic Transform (NTT)

The input polynomials are transformed from the coefficient domain into the NTT domain using the Forward NTT module. The butterfly processing units perform modular additions, subtractions, and multiplications with precomputed twiddle factors stored in ROM. Transforming the polynomials into the NTT domain enables polynomial multiplication to be performed efficiently as element-wise multiplication.

### Step 6: Encryption

Using the generated keys, the plaintext polynomial is encrypted according to the simplified lattice-based encryption algorithm. Polynomial multiplications required during encryption are accelerated by the NTT engine, while modular arithmetic units ensure all computations remain within the finite field defined by modulus **q = 17**.

### Step 7: Inverse NTT

After point-wise multiplication in the transform domain, the Inverse NTT converts the resulting polynomial back into the coefficient domain. This reconstructed polynomial forms the encrypted ciphertext.

### Step 8: Decryption

The ciphertext is processed using the generated secret key. Similar polynomial arithmetic operations are performed to recover the original plaintext polynomial. The recovered coefficients are then converted back into the corresponding message bits.

### Step 9: Output Display

The decrypted message is transmitted back to the host computer through the UART Transmitter. Simultaneously, the LCD controller displays the current encryption/decryption status and the recovered message, providing real-time verification of successful operation.

### Step 10: System Verification

The complete hardware design is verified using Verilog simulation testbenches and implemented on the FPGA. Successful recovery of the transmitted message confirms the correct functionality of the simplified lattice-based PQC engine and the NTT accelerator.

---


# Main Modules

### Crypto Engine

Implements key generation, encryption, decryption, and overall control using a finite state machine.

### NTT Accelerator

Performs fast polynomial multiplication using:

* Forward NTT
* Point-wise multiplication
* Inverse NTT

### Modular Arithmetic Units

Implements efficient modular:

* Addition
* Subtraction
* Multiplication

### LFSR

Generates pseudo-random coefficients for secret keys and noise polynomials.

### UART Interface

Handles communication between the FPGA and the host computer.

### LCD Controller

Displays encryption status, ciphertext, and recovered message.

---

# Applications

* Post-Quantum Cryptography Research
* FPGA-based Cryptographic Accelerators
* Secure Embedded Systems
* Hardware Security
* Educational Demonstration of Lattice-Based Cryptography
* Real-Time Encryption Systems

---

# Future Improvements

* Full CRYSTALS-Kyber parameter implementation
* Support for larger polynomial sizes (N = 256)
* Higher security parameter sets
* Optimized pipelined NTT architecture
* DMA-based communication
* AXI interface for SoC integration
* Performance benchmarking and power optimization

---

# Results

* Successfully implemented a simplified lattice-based PQC engine on FPGA.
* Demonstrated hardware-accelerated polynomial multiplication using NTT.
* Achieved successful encryption and decryption of UART-transmitted messages.
* Displayed encryption status and recovered messages on an LCD.
* Validated the complete design using simulation and FPGA testing.

---

# Technologies Used

* Verilog HDL
* Xilinx Vivado
* FPGA (Artix-7)
* Python
* UART Communication
* Number Theoretic Transform (NTT)
* Lattice-Based Cryptography
* Finite State Machines (FSM)

---



