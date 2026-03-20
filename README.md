# Quantum Computing Course 
A comprehensive repository containing assignments and projects from the Quantum Computing course at Jio Institute, exploring fundamental quantum algorithms and their implementations using Qiskit.

## About This Course

This course covers core concepts in quantum computing, including quantum gates, circuits, entanglement, quantum algorithms, and practical implementations on quantum simulators. All code is written in Python using **Qiskit**, IBM's open-source quantum computing framework.

**Author:** Rushikesh Sonwane \
**Instructor:** Dr. Samik Mukherjee \
**Term:** Term 08, Quarter 04 \
**Institution:** Jio Institute

## Assignments & Projects

### 1. 📖 **[Assignment 1: Quantum Teleportation and Superdense Coding](Assignment-1/README.md)**
Implementation of two fundamental quantum communication protocols using Bell states.

- Quantum Teleportation - Transfer an unknown quantum state using entanglement
- Superdense Coding - Send 2 classical bits using 1 qubit and a Bell state

### 2. 📖 **[Assignment 2: Simon's Algorithm](Assignment-2/README.md)**
A quantum search algorithm demonstrating exponential quantum speedup over classical approaches.

- Find a hidden binary string from a quantum oracle
- $O(2^n)$ classical queries vs $O(n)$ quantum queries

### 3. 📖 **[Course Project: Grover's Algorithm (10-Qubit Search)](Project/README.md)**
A comprehensive implementation of Grover's search algorithm with two different oracle approaches.

- Search an unsorted database of 1024 quantum states
- Two implementations: IBM MCMTGate oracle and custom phase kickback oracle
- Multi-iteration study: 1, 3, 5, 10, and optimal (k=17) iterations
- $O(N)$ classical search vs $O(\sqrt{N})$ quantum search (~30× speedup)

---

## Quick Start

### Prerequisites

- Python 3.7+
- Qiskit (`qiskit-terra` and `qiskit-aer`)
- Jupyter Notebook

### Installation

```bash
# Create virtual environment
python -m venv .venv

# Activate (Windows)
.venv\Scripts\activate

# Install dependencies
pip install qiskit qiskit-aer numpy matplotlib jupyter
   ```

### Running the Notebooks

1. Navigate to any assignment or project folder:
   ```bash
   cd Assignment-1
   ```

2. Start Jupyter:
   ```bash
   jupyter notebook
   ```

3. Open and run the `.ipynb` files

## Key Quantum Concepts

| Concept | Where Covered |
|---------|---------------|
| **Quantum Gates** (H, X, Z, CNOT) | All assignments |
| **Superposition & Measurement** | Assignment 1 |
| **Bell States & Entanglement** | Assignment 1, Project |
| **Quantum Interference** | Assignment 2, Project |
| **Quantum Oracles** | Assignment 2, Project |
| **Amplitude Amplification** | Project |
| **Phase Kickback** | Project |

## References

- [IBM Quantum Learning](https://learning.quantum.ibm.com/)
- [Qiskit Documentation](https://qiskit.org/documentation/)
- [Qiskit Textbook](https://github.com/qiskit-community/qiskit-textbook)
- [Nielsen & Chuang - Quantum Computation and Quantum Information](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information)

## Author

**Rushikesh Sonwane**

---

*For detailed information about each assignment and project, please refer to the README files in their respective directories.*

*Last Updated: March 2026*
