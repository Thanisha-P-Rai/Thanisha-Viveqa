# FPGA-Based Simplified Lattice-Based Post-Quantum Cryptography Engine using NTT Accelerator

## Overview

This project implements a **simplified lattice-based Post-Quantum Cryptography (PQC) encryption/decryption engine** on an FPGA using **Verilog HDL**. The design demonstrates the core concepts behind modern lattice-based cryptographic schemes such as **CRYSTALS-Kyber**, while remaining lightweight enough for educational and FPGA implementation.

The system performs key generation, encryption, and decryption using polynomial arithmetic accelerated by the **Number Theoretic Transform (NTT)**. Communication with a host PC is achieved through UART, while encryption status and results are displayed on an LCD.

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

1. The user sends an 8-bit message through the Python UART GUI.
2. The UART receiver captures the incoming byte.
3. The crypto engine generates secret and public keys.
4. Polynomial multiplication is accelerated using the Number Theoretic Transform (NTT).
5. The message is encrypted into ciphertext polynomials.
6. The ciphertext is decrypted using the generated secret key.
7. The recovered message is transmitted back through UART and displayed on the LCD.

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



