# Assignment 2: Simon's Algorithm

This assignment explores **Simon's Algorithm**, a foundational quantum algorithm that demonstrates exponential quantum speedup over classical computation. Students implement Simon's algorithm using Qiskit to find a hidden binary string encoded in a quantum oracle.

## Overview

This assignment is part of the Quantum Computing course taught by Dr. Samik Mukherjee at Jio Institute. Simon's Algorithm is one of the earliest quantum algorithms to show a separation between quantum and classical complexity: while classical algorithms require exponential queries, Simon's quantum algorithm determines the secret string in polynomial time.

## Problem Statement

Given a function $f : {0,1}^{n} rightarrow {0,1}^{n}$ satisfying the **Simon promise**:

$$f(x) = f(x oplus s)$$

for some secret non-zero binary string $s$, the goal is to determine $s$. The algorithm works by querying a quantum oracle that implements $f$, with each measurement producing a bit string $u$ satisfying:

$$u cdot s equiv 0 pmod{2}$$

These measurement outcomes form a system of linear equations whose solution reveals the secret string.

## Assignment Structure

### Part A: Understanding Simon's Algorithm

**Topics Covered:**

-   Study of the reference implementation (s = 011)
-   Understanding quantum oracle construction
-   Role of Hadamard gates in creating superposition and interference
-   Analysis of measurement outcomes

**Learning Objectives:**

-   Comprehend the three main components of Simon's circuit:
    1.  **First Hadamard Layer:** Creates uniform superposition of all possible inputs
    2.  **Oracle:** Encodes the hidden structure of function f
    3.  **Second Hadamard Layer:** Creates quantum interference, eliminating strings where $u cdot s equiv 1 pmod{2}$

**Key Concepts:**

-   Why measurement outcomes satisfy $u cdot s equiv 0 pmod{2}$
-   Destructive and constructive interference in quantum computing
-   Quantum speedup: $O(n)$ quantum queries vs exponential classical queries

### Part B: Implementation for a New Secret String

**Objective:** Extend Simon's algorithm to a 5-bit secret string s = 10110

**Requirements:**

1.  Implement the Simon oracle for the chosen secret string
2.  Construct the full quantum circuit in Qiskit
3.  Execute with at least 2000 shots on the Aer simulator
4.  Record and analyze measurement outcomes
5.  Exclude the trivial outcome (00...00) from analysis

**Oracle Construction Steps:**

1.  **Copy Step:** CNOT gates from each input qubit to corresponding output qubit
2.  **Encode Step:** Apply CNOT gates from anchor qubit to output qubits where s[i] = 1
    -   Anchor qubit: First position where s[i] = 0
    -   For s = 10110: anchor = qubit 1, targets = output qubits 0, 2, 3

**Verification:**

-   Check that all non-trivial measurement outcomes $u$ satisfy $u cdot s equiv 0 pmod{2}$

### Part C: Classical Post-Processing

**Objective:** Solve the system of linear equations obtained from quantum measurements

**Tasks:**

1.  Extract all distinct measured bit strings $u$ from quantum experiments
2.  Construct the matrix equation: $mathbf{U}s equiv 0 pmod{2}$
3.  Solve the linear system over $text{GF}(2)$ (binary field)
4.  Recover the secret string $s$

**Method:**

-   Use Gaussian elimination over GF(2) to find the solution space
-   The solution yields the hidden string $s$

## Prerequisites

-   Python 3.7+
-   Qiskit (`qiskit-terra` and `qiskit-aer`)
-   NumPy
-   Jupyter Notebook or JupyterLab
-   Matplotlib (for visualizations)

## Installation

Set up the required packages:

```bash
pip install qiskit qiskit-aer numpy matplotlib
```

## Usage

Open and run the Jupyter notebook:

```bash
jupyter notebook Rushikesh-Sonwane-Assignment2.ipynb
```

Execute cells sequentially to:

1.  Understand the reference 3-bit implementation
2.  Implement and test the 5-bit algorithm
3.  Extract measurement outcomes
4.  Solve the linear system and recover the secret string

## Key Quantum Concepts

-   **Superposition:** Quantum states representing multiple possibilities simultaneously
-   **Quantum Interference:** Constructive and destructive amplitudes in quantum mechanics
-   **Quantum Oracle:** Black-box quantum operations implementing classical functions
-   **Hadamard Transform:** Creates uniform superposition and performs basis transformation
-   **Measurement:** Collapses quantum superposition to classical bits
-   **Linear Systems over GF(2):** Boolean algebra using XOR operations (modulo 2 arithmetic)

## Files

```
Assignment-2/├── Rushikesh-Sonwane-Assignment2.ipynb├── Assignment+2.pdf (problem statement)└── README.md (this file)
```

## Circuit Diagram Summary

### Reference Implementation (n=3, s='011')

```
Input Register (q0, q1, q2):    |0⟩ ──H──[Oracle]──H──MOutput Register (q3, q4, q5):   |0⟩ ─────[Oracle]────Classical Bits (c0, c1, c2):              M
```

### Part B Implementation (n=5, s='10110')

```
Input Register (q0..q4):         |0⟩⊗⁵ ──H⊗⁵──[Oracle]──H⊗⁵──M⊗⁵Output Register (q5..q9):        |0⟩⊗⁵ ──────[Oracle]────Classical Bits (c0..c4):                  M⊗⁵
```

## Algorithm Complexity

Aspect

Classical

Quantum

Oracle Queries

$O(2^n)$

$O(n)$

Time Complexity

Exponential

Polynomial

Post-Processing

Gaussian elimination $O(n^3)$

Same

## References

-   [IBM Quantum Learning - Simon's Algorithm](https://quantum.cloud.ibm.com/learning/en/courses/fundamentals-of-quantum-algorithms/quantum-query-algorithms/simon-algorithm)
-   [Qiskit Textbook - Simon's Algorithm](https://github.com/qiskit-community/qiskit-textbook)
-   [Nielsen & Chuang - Quantum Computation and Quantum Information](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information)
-   [Simon, D. (1994) - "On the Power of Quantum Computation over Finite Groups"](https://doi.org/10.1109/SFCS.1994.365701)

## Author

Rushikesh Sonwane

## Course Information

-   **Course:** Quantum Computing
-   **Instructor:** Dr. Samik Mukherjee
-   **Term:** Term 08, Quarter 04
-   **Institution:** Jio Institute

## Notes

-   The reference implementation (Part A) uses s = 011 as provided in the IBM Quantum Learning module
-   The main implementation (Part B) uses s = 10110 as specified in the assignment
-   All circuits are simulated using Qiskit's Aer simulator (no real quantum hardware required)
-   Measurement outcomes are visualized using histograms for easy interpretation
-   Classical post-processing uses Gaussian elimination over the binary field $text{GF}(2)$

## Expected Outcomes

Upon completing this assignment, students will understand:

-   How quantum superposition enables querying all inputs simultaneously
-   How quantum interference amplifies correct answers and cancels incorrect ones
-   The multi-step workflow: quantum circuit → measurement → classical solver
-   Concrete example of quantum advantage in a well-defined problem
-   Implementation details of quantum algorithms in Qiskit

---

*Last Updated: March 2026*