# Technical Interview Preparation: Quantum Information & Qiskit

This guide prepares undergraduate and graduate applicants for technical interviews at organizations like IBM Quantum, covering quantum mechanics, quantum circuits, Qiskit implementation, noise modeling, and experimental design.

---

### Q1: What constitutes the mathematical difference between a classical bit, a probabilistic classical bit, and a qubit?
**Answer:**  
- A **classical bit** is deterministic, occupying a state space $\{0, 1\}$.
- A **probabilistic classical bit** is described by a probability distribution $(p_0, p_1)$ with $p_0, p_1 \ge 0$ and $p_0 + p_1 = 1$, forming a classical $L_1$-simplex.
- A **qubit** is a unit vector $|\psi\rangle = \alpha |0\rangle + \beta |1\rangle$ in a two-dimensional complex Hilbert space $\mathbb{C}^2$, normalized in the $L_2$-norm ($|\alpha|^2 + |\beta|^2 = 1$). Unlike classical probabilities, probability amplitudes $\alpha, \beta$ are complex numbers, allowing quantum interference (constructive and destructive).

---

### Q2: Why are quantum gates represented by unitary matrices?
**Answer:**  
In closed quantum systems, the total probability must always sum to $1$ ($\langle\psi|\psi\rangle = 1$). If state $|\psi\rangle$ transforms to $|\psi'\rangle = U|\psi\rangle$, preservation of the inner product requires:

$$\langle\psi'|\psi'\rangle = \langle U\psi | U\psi \rangle = \langle\psi| U^\dagger U |\psi\rangle = \langle\psi|\psi\rangle = 1$$

This holds for arbitrary states if and only if $U^\dagger U = I$. Thus, all valid quantum operations are unitary, preserving lengths, inner products, and reversibility.

---

### Q3: Why is global phase unobservable, and how does the Bloch sphere parameterization arise?
**Answer:**  
Let $|\psi'\rangle = e^{i\gamma}|\psi\rangle$. For any physical observable represented by Hermitian operator $A$, the expectation value is:

$$\langle\psi'|A|\psi'\rangle = \langle e^{i\gamma}\psi | A | e^{i\gamma}\psi \rangle = e^{-i\gamma}e^{i\gamma}\langle\psi|A|\psi\rangle = \langle\psi|A|\psi\rangle$$

All measurement probabilities are similarly invariant: $|\langle b | e^{i\gamma}\psi\rangle|^2 = |e^{i\gamma}|^2 |\langle b|\psi\rangle|^2 = |\langle b|\psi\rangle|^2$.  
Factoring out global phase leaves two real parameters: polar angle $\theta \in [0, \pi]$ and azimuthal angle $\phi \in [0, 2\pi)$, yielding the Bloch sphere form:

$$|\psi\rangle = \cos(\theta/2)|0\rangle + e^{i\phi}\sin(\theta/2)|1\rangle$$

---

### Q4: Explain the action of the Hadamard gate geometrically and algebraically.
**Answer:**  
- **Algebraically:** $H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$. It maps $H|0\rangle = |+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}$ and $H|1\rangle = |-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$.
- **Geometrically:** It is a $\pi$ rotation on the Bloch sphere about the diagonal axis $\frac{\hat{x} + \hat{z}}{\sqrt{2}}$, mapping the Z-axis to the X-axis and vice versa ($H X H = Z$ and $H Z H = X$).

---

### Q5: How do the $S$ and $T$ gates relate to the Pauli-Z gate?
**Answer:**  
They are fractional powers of the Pauli-$Z$ operator:
- $Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi} \end{pmatrix}$
- $S = \sqrt{Z} = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/2} \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}$ ($\pi/2$ phase rotation)
- $T = \sqrt{S} = Z^{1/4} = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$ ($\pi/4$ phase rotation)

Together with the Clifford group, the $T$ gate provides a universal set of quantum gates for quantum computation.

---

### Q6: What is the formal definition of quantum entanglement in bipartite systems?
**Answer:**  
A bipartite pure state $|\psi_{AB}\rangle \in \mathcal{H}_A \otimes \mathcal{H}_B$ is entangled if and only if it cannot be written as a product of single-qubit states:

$$|\psi_{AB}\rangle \ne |\phi_A\rangle \otimes |\chi_B\rangle$$

For mixed states represented by density matrix $\rho_{AB}$, it is separable if it can be written as a convex combination of product states: $\rho_{AB} = \sum_k p_k \rho_A^k \otimes \rho_B^k$, where $p_k \ge 0, \sum p_k = 1$. Otherwise, $\rho_{AB}$ is entangled.

---

### Q7: Prove that the Bell state $|\Phi^+\rangle = \frac{|00\rangle+|11\rangle}{\sqrt{2}}$ is entangled.
**Answer:**  
Assume $|\Phi^+\rangle$ is separable:

$$|\Phi^+\rangle = (a|0\rangle + b|1\rangle) \otimes (c|0\rangle + d|1\rangle) = ac|00\rangle + ad|01\rangle + bc|10\rangle + bd|11\rangle$$

Equating coefficients with $\frac{1}{\sqrt{2}}|00\rangle + 0|01\rangle + 0|10\rangle + \frac{1}{\sqrt{2}}|11\rangle$:
1. $ac = 1/\sqrt{2} \implies a \ne 0 \text{ and } c \ne 0$
2. $ad = 0 \implies d = 0$ (since $a \ne 0$)
3. $bc = 0 \implies b = 0$ (since $c \ne 0$)
4. $bd = 0 \cdot 0 = 0$

However, the coefficient of $|11\rangle$ is $bd = 1/\sqrt{2} \ne 0$, yielding a contradiction. Thus, $|\Phi^+\rangle$ cannot be factored into product states.

---

### Q8: What is the difference between Qiskit's qubit ordering and traditional textbook ordering?
**Answer:**  
Qiskit uses **little-endian** indexing, whereas many physics textbooks use **big-endian** indexing.
- In Qiskit, qubit index 0 corresponds to the least significant bit (rightmost bit in a bitstring):
  
  $$|q_1 q_0\rangle = |q_1\rangle \otimes |q_0\rangle$$

- For example, if qubit 0 is $|1\rangle$ and qubit 1 is $|0\rangle$, Qiskit labels the state as $|01\rangle$.
- In the matrix representation, the tensor product order in Qiskit is $q_1 \otimes q_0$.

---

### Q9: How does the CNOT gate create entanglement from separable states?
**Answer:**  
A CNOT gate entangles states when its control qubit is in a superposition state. For example:
1. Initialize $|00\rangle$.
2. Apply Hadamard to qubit 0: $(H \otimes I)|00\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}} \otimes |0\rangle = \frac{|00\rangle + |10\rangle}{\sqrt{2}}$ (still a separable product state).
3. Apply $CX$ (qubit 0 as control, qubit 1 as target):
   $$CX \left( \frac{|00\rangle + |10\rangle}{\sqrt{2}} \right) = \frac{CX|00\rangle + CX|10\rangle}{\sqrt{2}} = \frac{|00\rangle + |11\rangle}{\sqrt{2}} = |\Phi^+\rangle$$
Because the target qubit's state is conditioned on the superposition of the control, the composite system becomes non-separable.

---

### Q10: How can all four Bell states be constructed using only $H$, $CX$, and Pauli gates?
**Answer:**  
Starting from the standard entangled state $|\Phi^+\rangle = CX(H \otimes I)|00\rangle$:
1. $|\Phi^+\rangle = \frac{|00\rangle+|11\rangle}{\sqrt{2}}$
2. $|\Phi^-\rangle = (Z \otimes I)|\Phi^+\rangle = \frac{|00\rangle-|11\rangle}{\sqrt{2}}$
3. $|\Psi^+\rangle = (I \otimes X)|\Phi^+\rangle = \frac{|01\rangle+|10\rangle}{\sqrt{2}}$
4. $|\Psi^-\rangle = (Z \otimes X)|\Phi^+\rangle = \frac{|01\rangle-|10\rangle}{\sqrt{2}}$

This illustrates that any Bell state can be mapped to any other via local single-qubit Pauli transformations.

---

### Q11: Why cannot computational Z-basis measurements alone distinguish $|\Phi^+\rangle$ from $|\Phi^-\rangle$?
**Answer:**  
In the computational basis $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$, both states have identical outcome probabilities:

$$P(00) = \left|\pm\frac{1}{\sqrt{2}}\right|^2 = 0.5, \quad P(11) = \left|\pm\frac{1}{\sqrt{2}}\right|^2 = 0.5, \quad P(01) = P(10) = 0$$

The difference is a relative phase factor ($+1$ vs. $-1$). To distinguish them, one must measure in a basis sensitive to phase, such as the $X \otimes X$ basis, or perform full quantum state tomography.

---

### Q12: What is the physical meaning of the CHSH inequality, and what does its violation prove?
**Answer:**  
The CHSH inequality states that for any local hidden variable (LHV) theory satisfying local realism:

$$S = E(a, b) + E(a, b') + E(a', b) - E(a', b') \le 2$$

A measured value of $S > 2$ mathematically rules out all local realist theories (the EPR postulate). It proves that physical systems do not possess predetermined values independent of measurement, and that quantum correlations are fundamentally non-local.

---

### Q13: What is the Cirel'son bound?
**Answer:**  
The Cirel'son bound is the maximum possible value of the CHSH parameter $S$ allowed by quantum mechanics:

$$S_{max} = 2\sqrt{2} \approx 2.8284$$

Even though an arbitrary algebraic combination of correlations bounded by $\pm 1$ could theoretically reach $S = 4$ (e.g. in PR-boxes), quantum mechanics cannot exceed $2\sqrt{2}$.

---

### Q14: How is measurement along an arbitrary axis $\vec{n} = (\sin\theta, 0, \cos\theta)$ implemented in Qiskit?
**Answer:**  
Standard quantum hardware only performs projective measurements in the computational $Z$-basis ($\Pi_0 = |0\rangle\langle 0|, \Pi_1 = |1\rangle\langle 1|$).  
To measure an observable $O = \vec{n}\cdot\vec{\sigma} = R_y(\theta) Z R_y(-\theta)$, we apply the inverse unitary rotation $R_y(-\theta)$ prior to computational measurement:

$$\langle\psi| O |\psi\rangle = \langle\psi| R_y(\theta) Z R_y(-\theta) |\psi\rangle = \langle R_y(-\theta)\psi | Z | R_y(-\theta)\psi \rangle$$

In Qiskit, this is written as:
```python
qc.ry(-theta, qubit)
qc.measure(qubit, cbit)
```

---

### Q15: What is the difference between a pure state and a mixed state?
**Answer:**  
- A **pure state** is an isolated quantum state represented by a state vector $|\psi\rangle$ or rank-1 projection density matrix $\rho = |\psi\rangle\langle\psi|$, with $\text{Tr}(\rho^2) = 1$.
- A **mixed state** represents a statistical ensemble of pure states $\{\left(p_i, |\psi_i\rangle\right)\}$ with $p_i > 0, \sum p_i = 1$. Its density matrix is $\rho = \sum p_i |\psi_i\rangle\langle\psi_i|$, with $\text{Tr}(\rho^2) < 1$. Mixed states describe open quantum systems entangled with the environment.

---

### Q16: How do bit-flip, phase-flip, and depolarizing noise channels differ?
**Answer:**  
- **Bit-Flip Channel:** Applies $X$ with probability $p$. Inverts $|0\rangle \leftrightarrow |1\rangle$, affecting diagonal populations.
- **Phase-Flip Channel:** Applies $Z$ with probability $p$. Leaves $|0\rangle$ and $|1\rangle$ populations unchanged, but dampens off-diagonal coherences by $(1-2p)$.
- **Depolarizing Channel:** With probability $p$, replaces the state with the maximally mixed state $I/2$ (applying $X, Y, Z$ each with probability $p/3$). Isotropically shrinks the Bloch sphere vector $\vec{r} \to (1 - \frac{4}{3}p)\vec{r}$.

---

### Q17: What is quantum state fidelity, and how is it calculated for pure vs. mixed states?
**Answer:**  
Fidelity $\mathcal{F}(\rho, \sigma)$ measures the geometric closeness of two quantum states:

$$\mathcal{F}(\rho, \sigma) = \left( \text{Tr}\sqrt{\sqrt{\rho}\sigma\sqrt{\rho}} \right)^2 \in [0, 1]$$

When $\rho = |\psi\rangle\langle\psi|$ is a pure state, the expression simplifies to the overlap:

$$\mathcal{F}(|\psi\rangle, \sigma) = \langle\psi|\sigma|\psi\rangle$$

$\mathcal{F} = 1$ if and only if $\rho = \sigma$.

---

### Q18: Why does simulating an $n$-qubit quantum system on a classical computer scale exponentially?
**Answer:**  
An $n$-qubit statevector requires $2^n$ complex amplitudes. Storing these amplitudes with double precision (16 bytes per complex number) requires:
- 30 qubits: $2^{30} \times 16 \text{ B} \approx 17.18 \text{ GB}$ RAM
- 40 qubits: $2^{40} \times 16 \text{ B} \approx 17.59 \text{ TB}$ RAM
- 50 qubits: $2^{50} \times 16 \text{ B} \approx 18 \text{ PB}$ RAM

Furthermore, applying a two-qubit gate involves multiplying a $2^n \times 2^n$ matrix by a $2^n$ vector, yielding an $\mathcal{O}(2^n)$ runtime per gate.

---

### Q19: What is the Law of Large Numbers scaling for quantum shot noise?
**Answer:**  
Projective measurement of a qubit in state $|\psi\rangle = \sqrt{p}|0\rangle + \sqrt{1-p}|1\rangle$ with $N$ independent shots follows a Binomial distribution $B(N, p)$.  
The sample frequency $\hat{p} = k/N$ has variance:

$$\text{Var}(\hat{p}) = \frac{p(1-p)}{N} \implies \sigma = \sqrt{\frac{p(1-p)}{N}} \propto \frac{1}{\sqrt{N}}$$

To reduce statistical uncertainty by a factor of 10, the number of measurement shots must be increased by a factor of 100.

---

### Q20: What are the primary modern features of Qiskit 2.x compared to legacy versions?
**Answer:**  
1. **Core Architecture:** Legacy packages (`qiskit-terra`, `qiskit-aer`, etc.) have been consolidated; Qiskit is now a unified top-level library with high-performance Rust internals (`qiskit._accelerate`).
2. **Execution Model:** Deprecation of monolithic helpers like `qiskit.execute()`. Execution now cleanly separates circuit construction from primitives (`SamplerV2`, `EstimatorV2`) and backends (`AerSimulator.run()`).
3. **OpenQASM 3.0 Support:** Full native support for classical control flow (dynamic circuits, real-time feedforward).
