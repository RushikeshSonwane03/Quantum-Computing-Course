# Grover's Algorithm — 10-Qubit Implementation

A comprehensive quantum computing project implementing **Grover's search algorithm** on a 10-qubit system using Qiskit, demonstrating quantum speedup over classical search with two different oracle implementations.

## Project Overview

This project implements Grover's algorithm on a **10-qubit search space** ($2^{10} = 1024$ states). The implementation explores both **direct oracle construction using MCMTGate** and a **phase kickback technique using ancilla qubits**, comparing their performance and gate efficiency.

### Key Metrics

| Parameter | Value |
|---|---|
| **Search space size** | $N = 2^{10} = 1024$ states |
| **Marked states** | $M = 2$ |
| **Optimal iterations** | $k_{\text{opt}} = 17$ |
| **Theoretical success probability** | $\approx 99.9\%$ |
| **Classical speedup** | $\approx 30\times$ |

## Problem Statement

**Search Task:** Find marked states `"0110011010"` and `"1101010001"` in an unsorted database of 1024 quantum states.

- `"0110011010"` → decimal 410
- `"1101010001"` → decimal 849

**Classical approach:** Check items one by one ($O(N)$ queries ≈ 512 on average)  
**Quantum approach:** Use Grover's algorithm ($O(\sqrt{N})$ queries = 17)

## Algorithm Theory

### Grover's Algorithm at a Glance

Grover's algorithm achieves quadratic quantum speedup by iteratively:

1. **Oracle ($U_f$):** Applies a $-1$ phase to marked states
2. **Diffuser ($U_s$):** Reflects all amplitudes about the average, amplifying marked states
3. **Repeat:** Apply oracle + diffuser combination $k_{\text{opt}}$ times

### Mathematical Framework

Initial superposition of all states:
$$|s\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle$$

After $k$ Grover iterations, the probability of measuring a marked state:
$$P_{\text{marked}}(k) = \sin^2\left((2k+1)\,\theta\right), \quad \theta = \arcsin\sqrt{\frac{M}{N}} \approx 0.0442 \text{ rad}$$

Optimal iterations to maximize success probability:
$$k_{\text{opt}} = \left\lfloor \frac{\pi}{4\theta} \right\rfloor = \left\lfloor \frac{\pi}{4 \times 0.0442} \right\rfloor = 17$$

## Project Structure

### Part A: IBM Documentation Oracle (Direct Port)

Uses the **MCMTGate(ZGate()) multi-controlled-Z gate** with the open-control trick to implement the oracle.

**How it works:**
1. For each marked state, identify positions with bit `'0'`
2. Apply X gates on those positions (flip to `'1'`)
3. Apply MCMTGate(ZGate(), num_controls=9, num_targets=1) — multi-controlled-Z gate (flips phase when all qubits are `|1⟩`)
4. Undo X gates (restore original state)
5. Diffuser: Manual construction using $H^n X^n \text{MCZ} X^n H^n$ (not from library)

**Circuit characteristics:**
- Oracle uses MCMTGate high-level composite gate on data qubits
- Diffuser manually constructed with elementary gates
- Total: 10 qubits (all data, no ancilla)
- Requires transpilation to Aer basis gates before execution

### Part B: Custom Phase Kickback Oracle

Implements the oracle using **phase kickback technique** with an ancilla qubit—built entirely from scratch.

**How it works:**
1. Prepare ancilla in state $|{-}\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$ using $X \to H$
2. For each marked state:
   - Apply X gates on positions with bit `'0'` (open controls)
   - Apply multi-controlled-X (MCX) with all 10 data qubits controlling the ancilla
   - When data == marked state, MCX flips $|{-}\rangle \to -|{-}\rangle$, kicking back $-1$ phase to data
   - Undo X gates and uncompute ancilla back to $|0\rangle$ using $H \to X$
3. Hand-built diffuser: $H^{\otimes n} X^{\otimes n} \text{MCZ} X^{\otimes n} H^{\otimes n}$ (identical to Part A)

**Circuit characteristics:**
- Total: 11 qubits (10 data + 1 ancilla)
- Ancilla serves only as a phase-flip mechanism and is uncomputed (not measured)
- Custom-built oracle and diffuser from first principles
- Both oracle and diffuser use only elementary gates (no library functions)

### Comparison

| Aspect | Part A (IBM-style) | Part B (Phase Kickback) |
|--------|-------------------|----------------------|
| **Oracle implementation** | MCMTGate(ZGate()) + open controls | MCX into ancilla $|{-}\rangle$ + open controls |
| **Ancilla qubits** | 0 | 1 (uncomputed) |
| **Diffuser source** | Manual: $H^n X^n \text{MCZ} X^n H^n$ | Manual: $H^n X^n \text{MCZ} X^n H^n$ (identical) |
| **Total qubits** | 10 | 11 |
| **Measured qubits** | 10 | 10 (ancilla excluded) |
| **Success probability at k=17** | $\approx 99.9\%$ | $\approx 99.9\%$ |

## Implementation Details

### Circuit Components

**1. Initialization**
```
|0⟩^n ──H^⊗n── |s⟩ (uniform superposition)
```

**2. Grover Operator** (Applied 6 times)
```
|s⟩ ──[U_f]──[U_s]── Amplified state
        ↑      ↑
      Oracle Diffuser
```

**3. Measurement**
```
Final state ──M── Classical bitstring
```

### Code Structure in Notebook

1. **Imports & Setup** - Load Qiskit libraries, set parameters
2. **Problem Definition** - Define marked states, compute optimal iterations
3. **Part A: IBM Subroutine**
   - Build oracle using `MCMTGate`
   - Construct full circuit
   - Simulate and visualize results
4. **Part B: Custom Implementation**
   - Define `custom_grover_oracle()` function
   - Define `custom_diffuser()` function
   - Build full circuit with custom components
   - Simulate and compare results
5. **Comparison & Verification** - Benchmark both approaches

## Results

### Success Probability

Both implementations achieve approximately 99.9% success probability (measuring a marked state), confirming the theoretical prediction:

$$P_{\text{theory}}(\text{marked}) = \sin^2(35\theta) \approx 0.999$$

### Measured Distribution

Out of 8192 shots at optimal iterations ($k = 17$):
- **Marked state 1** (`"0110011010"`): $\approx 4000-4100$ hits
- **Marked state 2** (`"1101010001"`): $\approx 3900-4000$ hits
- **Other 1022 states**: $\approx 50$ total hits combined

### Key Observations

1. **Both oracles work equally well** despite using different gate sets
2. **The diffuser is the key component** — it converts phase differences to amplitude amplification
3. **Fewer iterations than expected** — Only 6 queries instead of classical ~64
4. **Phase patterns matter** — The oracle's internal gate structure doesn't affect final probability

## Prerequisites

- Python 3.8+
- Qiskit (`qiskit-terra` and `qiskit-aer`)
- NumPy
- Matplotlib
- Jupyter Notebook or JupyterLab
- 8192+ shots capability (standard on Aer simulator)

## Installation

Set up the required packages:

```bash
pip install qiskit qiskit-aer numpy matplotlib jupyter
```

Activate your Python environment and install within the virtual environment for this course:

```bash
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install qiskit qiskit-aer numpy matplotlib
```

## Usage

Open and run the Jupyter notebook:

```bash
jupyter notebook Rushikesh-Sonwane-Grovers-Algorithm-Project.ipynb
```

Execute cells sequentially to:
1. Understand Grover's algorithm theory
2. Run Part A (IBM subroutine) simulation
3. Run Part B (custom phase kickback) simulation
4. Compare results and analyze findings

## Key Concepts Covered

- **Quantum Superposition** - Creating uniform superposition with Hadamard gates
- **Quantum Interference** - Phase amplification and amplitude cancellation
- **Quantum Oracle** - Marking target states via phase flip
- **Amplitude Amplification** - Grover diffuser mechanism
- **Phase Kickback** - Using ancilla qubits to achieve phase gates
- **Multi-Controlled Gates** - MCX and MCZ generalizations
- **Quantum Circuit Simulation** - Transpilation and execution on Aer backend
- **Computational Complexity** - Comparing $O(N)$ vs $O(\sqrt{N})$ speedup

## Performance Metrics

### Quantum Advantage

- **Classical search space**: 1024 items
- **Classical oracle queries** (average): ~512
- **Quantum oracle queries**: 17
- **Speedup**: $512 / 17 \approx 30\times$
- **Asymptotic speedup**: $O(\sqrt{N})$ for large $N$
- **Iteration counts studied**: 1, 3, 5, 10, and 17 (optimal)

### Circuit Complexity

| Metric | Part A (10 qubits) | Part B (11 qubits) |
|--------|---------|----------|
| Oracle depth | $\approx 15-20$ | $\approx 20-30$ |
| Diffuser depth | $\approx 25-30$ | $\approx 25-30$ |
| Per-iteration depth | $\approx 40-50$ | $\approx 45-60$ |
| Total circuit depth (k=17) | $\approx 700-800$ | $\approx 800-1000$ |
| Single-qubit gates | $\approx 3000+$ | $\approx 3500+$ |
| Two-qubit gates (MCX depth) | $\approx 150-200$ | $\approx 200-250$ |

## References

- [IBM Quantum Learning - Grover's Algorithm Tutorial](https://quantum.cloud.ibm.com/docs/en/tutorials/grovers-algorithm)
- [Qiskit Documentation - Grover's Algorithm](https://qiskit.org/documentation/stubs/qiskit.algorithms.AmplificationProblem.html)
- [Qiskit Textbook - Grover's Algorithm](https://github.com/qiskit-community/qiskit-textbook)
- [Grover, L. K. (1996) - "A Fast Quantum Mechanical Algorithm for Database Search"](https://arxiv.org/abs/quant-ph/9605043)
- [Nielsen & Chuang - Quantum Computation and Quantum Information](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information)

## Author

**Rushikesh Sonwane**

## Course Information

- **Course:** Quantum Computing
- **Instructor:** Dr. Samik Mukherjee
- **Term:** Term 08, Quarter 04
- **Institution:** Jio Institute
- **Project Type:** Final Course Project

## Important Notes

### Transpilation Requirement

Custom gates like `MCMTGate(ZGate())` create high-level abstract composite gates that aren't directly executable by Qiskit Aer's simulator. Before running:

```python
qc_transpiled = transpile(qc, backend, optimization_level=1)
```

This decomposes custom gates into Aer's supported **basis gates** (`cx`, `u`, `x`, `h`, `measure`, etc.), producing an equivalent circuit that the simulator can execute. This means the effective circuit depth is significantly larger than the abstract representation.

### Ancilla Uncomputation (Phase Kickback)

In Part B, the ancilla qubit is **not** measured — it's returned to $|0\rangle$ at the end of each oracle application. This is critical:
- The ancilla is only used to create a phase-flip mechanism via the $|{-}\rangle$ kickback
- After each marked state encoding, the ancilla is uncomputed: $|{-}\rangle \xrightarrow{H} |1\rangle \xrightarrow{X} |0\rangle$
- At measurement time, only the 10 data qubits are measured (classical register has 10 bits)
- The phase information from the ancilla interaction "leaks" into the data qubit amplitudes

## Future Extensions

Possible enhancements to this project:

1. **Larger search spaces** - Extend to 15+ qubits ($2^{15} = 32768$ states) to see asymptotic behavior
2. **Variable marked state count** - Generalize to $M > 2$ marked states and verify $k_{\text{opt}} \propto \sqrt{N/M}$
3. **Iterative deepening** - Implement variable-depth search without prior knowledge of $k_{\text{opt}}$
4. **Hardware execution** - Run on IBM Quantum real devices and measure decoherence effects
5. **Amplitude amplification beyond search** - Generalize to other amplitude amplification problems
6. **More oracle variants** - Compare with Z-basis, QAOA-style, and database-specific oracles
7. **Error mitigation** - Add noise models and test error mitigation strategies

## Troubleshooting

**Q: "Unknown gate" error during simulation?**  
A: You likely forgot to transpile. Use `transpile(qc, backend)` before `backend.run()`.

**Q: Marked states don't appear with high probability?**  
A: Check that:
- Marked states are defined correctly (7-bit strings)
- Optimal iteration count is correct
- Oracle correctly identifies marked state positions

**Q: Why use 8192 shots?**  
A: Provides $\approx 1\%$ statistical error margin. Use more shots for higher precision.

---

*Last Updated: March 2026*
