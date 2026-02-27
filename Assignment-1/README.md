# Assignment 1: Quantum Teleportation and Superdense Coding

This assignment explores two fundamental quantum communication protocols: **Quantum Teleportation** and **Superdense Coding**, both leveraging the power of Bell states (entangled pairs).

## Overview

This assignment is part of the Quantum Computing course taught by Dr. Samik Mukherjee. Students implement quantum circuits using **Qiskit**, IBM's open-source quantum computing framework, to understand and demonstrate key concepts in quantum information theory.

## Assignment Structure

### Problem 1: Quantum Teleportation (Bell State)
**File:** `Problem1- Rushikesh-Sonwane-Assignment1.ipynb`

**Objective:** Implement quantum teleportation and verify its correctness by undoing the unitary operation used to generate an unknown quantum state |ψ⟩.

**Key Topics:**
- Preparation of arbitrary qubit states using rotation gates ($R_y(\theta)$ and $R_z(\phi)$)
- Creation of Bell states (maximally entangled pairs)
- Classical measurement and quantum correction
- State preparation and verification through Bloch sphere visualization

**Implementation Steps:**
1. Prepare an arbitrary single-qubit state with chosen rotation angles
2. Create a Bell pair using Hadamard and CNOT gates
3. Perform the teleportation circuit (Bell measurement and corrections)
4. Verify the teleported state matches the original state

### Problem 2: Superdense Coding (Bell State)
**File:** `Problem2- Rushikesh-Sonwane-Assignment1.ipynb.ipynb`

**Objective:** Demonstrate how two classical bits can be transmitted using a single qubit and a pre-shared Bell state.

**Key Topics:**
- Bell state creation and manipulation
- Encoding 2-bit classical messages (00, 01, 10, 11) using single-qubit gates
- Bell basis decoding
- Measurement and histogram analysis

**Encoding Rules:**
- `00` → Identity gate (I)
- `01` → Pauli-Z gate (Z)
- `10` → Pauli-X gate (X)
- `11` → Pauli-X followed by Pauli-Z (XZ)

**Implementation Steps:**
1. Create a Bell pair shared between Alice and Bob
2. Encode a chosen 2-bit message using the encoding rule
3. Perform Bell-basis decoding (inverse Bell measurement)
4. Measure both qubits and verify the measurement results

## Prerequisites

- Python 3.7+
- Qiskit (`qiskit-aer`)
- Jupyter Notebook or JupyterLab
- Matplotlib (for visualizations)

## Installation

To set up the required packages:

```bash
pip install qiskit qiskit-aer matplotlib
```

## Usage

Run the Jupyter notebooks in your preferred environment:

```bash
jupyter notebook
```

Then open either notebook file to view the complete implementation, explanations, and visualizations.

## Key Quantum Concepts Covered

- **Bloch Sphere Representation:** Visualization of single-qubit states
- **Bell States:** Entangled two-qubit states and their properties
- **Quantum Measurement:** Projective measurement and state collapse
- **Quantum Gates:** Hadamard (H), CNOT, Pauli gates (X, Y, Z), and rotation gates (Ry, Rz)
- **Quantum Simulation:** Using Qiskit's Aer simulator backend

## Files

```
Assignment-1/
├── Assignment 1_2026 Quantum Computing.pdf
├── Problem1- Rushikesh-Sonwane-Assignment1.ipynb
├── Problem2- Rushikesh-Sonwane-Assignment1.ipynb.ipynb
└── README.md (this file)
```

## Author

Rushikesh Sonwane

## Course Information

- **Course:** Quantum Computing
- **Instructor:** Dr. Samik Mukherjee
- **Term:** Term 08, Quarter 04
- **Institution:** Jio Institute

## References

- [Qiskit Official Documentation](https://qiskit.org/documentation/)
- [Nielsen & Chuang - Quantum Computation and Quantum Information](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information/8D2FBEE5CB181277F5093F1D15897C3C)
- [IBM Quantum Learning Resources](https://learning.quantum.ibm.com/)

## Notes

- Both notebooks include detailed explanations and visualizations
- Circuits are drawn using Qiskit's `draw()` method for clarity
- Simulation results are visualized using histograms for easy interpretation
- Custom rotation angles are chosen outside special values (0, π/2, π, π/4) for general demonstration

---

*Last Updated: 27th February 2026*

