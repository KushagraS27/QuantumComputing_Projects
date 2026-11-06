# Computational Methodology and Simulation Protocol

This document outlines the numerical simulation framework, software architecture, and statistical methodology used in this project.

---

## 1. Computational Framework and Software Architecture

The computational simulations were engineered using the modern **Qiskit 2.x** and **Qiskit Aer** ecosystems:

- **Language:** Python 3.10+ / 3.14
- **Core Library:** Qiskit (`qiskit>=2.0.0`)
- **Simulation Engine:** Qiskit Aer (`qiskit-aer>=0.17.0`)
- **Statevector Algebra & Visualization:** `qiskit.quantum_info`, `qiskit.visualization`, `matplotlib`, `pylatexenc`
- **Data Structuring & Analysis:** `numpy`, `pandas`, `scipy`

### 1.1 Qubit Indexing and Little-Endian Ordering
Qiskit employs the **little-endian convention** for qubit labeling and tensor product representations:

$$|q_n q_{n-1} \dots q_1 q_0\rangle = |q_n\rangle \otimes |q_{n-1}\rangle \otimes \dots \otimes |q_0\rangle$$

In this convention:
- In bitstring `'01'`, the rightmost bit ($q_0 = 1$) is qubit 0, and the leftmost bit ($q_1 = 0$) is qubit 1.
- All matrix operators and measurements in our code adhere strictly to this standard, ensuring that theoretical derivations match numerical results.

---

## 2. Experimental Simulation Protocols

### 2.1 Single-Qubit Gate Benchmarks
- **Initial State:** Ground state $|0\rangle$ and excited state $|1\rangle$.
- **Simulation Method:** Exact algebraic statevector evolution via `Statevector.from_instruction()`.
- **Output:** Unitary matrix extraction, state amplitude tracking, and computational basis projection probabilities.

### 2.2 Quantum Superposition & Central Limit Theorem Verification
- **Circuit:** Hadamard transform applied to qubit 0: $H|0\rangle = |+\rangle$.
- **Measurements:** Projective Z-basis measurements across variable shot counts:
  - Small sample: $N = 1024$ shots
  - Large sample: $N = 10000$ shots
- **Statistical Evaluation:** Standard error $\sigma = \sqrt{\frac{p(1-p)}{N}} = \frac{1}{2\sqrt{N}}$ compared with observed deviations $|f_{obs} - 0.5|$.

### 2.3 Bell State Synthesis Protocol
- **Circuit Construction:**
  - $|\Phi^+\rangle$: $CX_{0,1} \cdot (H_0 \otimes I_1) |00\rangle$
  - $|\Phi^-\rangle$: $Z_0 \cdot CX_{0,1} \cdot (H_0 \otimes I_1) |00\rangle$
  - $|\Psi^+\rangle$: $X_1 \cdot CX_{0,1} \cdot (H_0 \otimes I_1) |00\rangle$
  - $|\Psi^-\rangle$: $Z_0 \cdot X_1 \cdot CX_{0,1} \cdot (H_0 \otimes I_1) |00\rangle$
- **Simulation:** 10,000 shots per Bell state on `AerSimulator` with random seed $42$.

### 2.4 CHSH Inequality Protocol
- **Target Entangled State:** $|\Phi^+\rangle = \frac{|00\rangle+|11\rangle}{\sqrt{2}}$.
- **Measurement Basis Transformation:** Since standard quantum detectors project onto the computational basis $\{|0\rangle, |1\rangle\}$ ($Z$-basis), measuring an observable rotated by angle $\theta$ in the $X$-$Z$ plane is implemented by applying the inverse rotation $R_y(-\theta)$ prior to computational measurement:
  
  $$\langle \psi | (\vec{n}\cdot\vec{\sigma}) | \psi \rangle = \langle \psi | R_y(\theta) Z R_y(-\theta) | \psi \rangle = \langle R_y(-\theta)\psi | Z | R_y(-\theta)\psi \rangle$$

- **Correlator Calculation:**
  $$E(a, b) = \frac{N_{00} + N_{11} - N_{01} - N_{10}}{N_{total}}$$
- **Angles Sampled:**
  - Alice: $a = 0$, $a' = \pi/2$
  - Bob: $b = \pi/4$, $b' = -\pi/4$
- **Continuous Sweep:** Sweep $\theta_B \in [0, 2\pi]$ with $\theta_A = 0$ over 60 discrete angular steps (4,000 shots each).

### 2.5 Quantum Noise Simulation Protocol
- **Noise Engine:** `qiskit_aer.noise.NoiseModel`.
- **Channels Modeled:**
  - Pauli-X (Bit-flip) with probability $p = 0.15$
  - Pauli-Z (Phase-flip) with probability $p = 0.15$
  - Depolarizing error with probability $p = 0.15$
- **Metric:** Quantum state fidelity $\mathcal{F}(\rho, \sigma) = \langle\Phi^+|\sigma|\Phi^+\rangle$ computed directly from noisy density matrix evolution via `save_density_matrix()`.

---

## 3. Reproducibility and Quality Control

- **Deterministic Seeds:** Fixed random seeds (`seed_simulator=42`) were applied to all Aer simulation jobs.
- **Pure Software Paths:** Standardized API methods with zero deprecated calls (e.g. no legacy `qiskit.execute` or `qiskit.Aer.get_backend`).
- **Data Export:** All numerical outputs were exported directly to structured CSV format (`results/simulation_results.csv`) and verified against theoretical predictions.
