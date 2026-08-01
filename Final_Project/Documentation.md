FPGA Implementation of a Simplified Lattice-Based Post-Quantum Cryptography Encryption and Decryption Engine Using a Number Theoretic Transform Accelerator
INTRODUCTION

Public-key cryptography forms the foundation of modern digital communication. Secure Internet browsing, online banking, cloud computing, military communication, healthcare systems, and IoT devices all depend upon cryptographic algorithms such as RSA and Elliptic Curve Cryptography (ECC). These algorithms derive their security from mathematical problems that are computationally infeasible for classical computers.

However, the rapid development of quantum computing has introduced a significant security challenge. Shor's algorithm demonstrates that sufficiently large quantum computers can efficiently solve the integer factorization and discrete logarithm problems upon which RSA and ECC rely. As a result, many widely deployed cryptographic systems will become vulnerable once practical quantum computers become available.

To address this issue, researchers have developed Post-Quantum Cryptography (PQC), a family of cryptographic algorithms designed to remain secure against both classical and quantum attacks. Among these, lattice-based cryptography has emerged as one of the strongest candidates due to its mathematical hardness, high efficiency, and suitability for hardware implementation.

The National Institute of Standards and Technology (NIST) selected CRYSTALS-Kyber as the standard algorithm for public-key encryption and key encapsulation. Kyber relies heavily on polynomial arithmetic performed over finite rings. The computationally intensive operation in Kyber is polynomial multiplication, which is accelerated using the Number Theoretic Transform (NTT).

This project implements a simplified lattice-based encryption and decryption engine inspired by Kyber on an FPGA. Although simplified for educational purposes (N = 8, Q = 17), the architecture preserves the essential components of a practical lattice-based cryptographic processor, including key generation, encryption, decryption, modular arithmetic, noise sampling, and an NTT accelerator.
