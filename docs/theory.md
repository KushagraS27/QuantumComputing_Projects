# Theoretical Background & Mathematical Foundations

This document provides a comprehensive, rigorous undergraduate Engineering Physics treatment of the quantum mechanical principles underpinning the **Quantum Circuit Simulation and Entanglement Analysis** project.

---

## 1. State Vectors, Hilbert Space, and Dirac Notation

In quantum mechanics, the state of an isolated physical system is represented by a unit vector $|\psi\rangle$ inhabiting a complex Hilbert space $\mathcal{H}$. 

For a two-level quantum system (a **qubit**), the Hilbert space is two-dimensional: $\mathcal{H} = \mathbb{C}^2$. The canonical computational basis comprises the orthonormal vectors $\{|0\rangle, |1\rangle\}$, defined in column vector representation as:

$$|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \quad |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

By the superposition principle, any arbitrary pure state $|\psi\rangle$ is a linear combination of these basis states:

$$|\psi\rangle = \alpha |0\rangle + \beta |1\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}, \quad \alpha, \beta \in \mathbb{C}$$

The dual vector $\langle\psi|$ (bra) corresponds to the conjugate transpose of $|\psi\rangle$ (ket):

$$\langle\psi| = (|\psi\rangle)^\dagger = \begin{pmatrix} \alpha^* & \beta^* \end{pmatrix}$$

The inner product induces a natural norm on the state:

$$\langle\psi|\psi\rangle = |\alpha|^2 + |\beta|^2 = 1$$

---

## 2. Bloch Sphere Geometric Representation

A general state in $\mathbb{C}^2$ requires four real parameters (two complex numbers $\alpha$ and $\beta$). However, normalization removes one degree of freedom:

$$|\alpha|^2 + |\beta|^2 = 1$$

Furthermore, an overall global phase factor $e^{i\gamma}$ is physically unobservable, as all expectation values $\langle\psi|A|\psi\rangle$ remain invariant under $|\psi\rangle \to e^{i\gamma}|\psi\rangle$:

$$\langle e^{i\gamma}\psi | A | e^{i\gamma}\psi \rangle = e^{-i\gamma}e^{i\gamma}\langle\psi|A|\psi\rangle = \langle\psi|A|\psi\rangle$$

Factoring out the global phase yields the canonical Bloch sphere parameterization with two independent real parameters, the polar angle $\theta \in [0, \pi]$ and the azimuthal angle $\phi \in [0, 2\pi)$:

$$|\psi\rangle = \cos\left(\frac{\theta}{2}\right)|0\rangle + e^{i\phi}\sin\left(\frac{\theta}{2}\right)|1\rangle$$

This maps the state uniquely to a point on the surface of the unit sphere $\mathcal{S}^2 \subset \mathbb{R}^3$, with Cartesian coordinates corresponding to the expectation values of the Pauli spin observables:

$$\vec{r} = (r_x, r_y, r_z) = (\langle X \rangle, \langle Y \rangle, \langle Z \rangle) = (\sin\theta\cos\phi, \; \sin\theta\sin\phi, \; \cos\theta)$$

- North pole ($\theta = 0$): $|0\rangle$
- South pole ($\theta = \pi$): $|1\rangle$
- Equator ($\theta = \pi/2$): Superposition states such as $|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}$ ($\phi = 0$) and $|R\rangle = \frac{|0\rangle+i|1\rangle}{\sqrt{2}}$ ($\phi = \pi/2$).

---

## 3. Unitary Operators and Quantum Logic Gates

Time evolution in closed quantum systems is governed by unitary operators $U$:

$$U^\dagger U = U U^\dagger = I$$

This guarantees that inner products and state normalizations are conserved:

$$\langle U\psi | U\psi \rangle = \langle\psi | U^\dagger U | \psi\rangle = \langle\psi | I | \psi\rangle = 1$$

### 3.1 Single-Qubit Elementary Gates

1. **Identity Gate ($I$):** Leaves the state completely unchanged.
   $$I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}$$

2. **Pauli-X Gate ($X$):** Quantum bit-flip operator (rotation by $\pi$ around the X-axis).
   $$X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad X|0\rangle = |1\rangle, \quad X|1\rangle = |0\rangle$$

3. **Pauli-Y Gate ($Y$):** Simultaneous bit-flip and phase-flip (rotation by $\pi$ around the Y-axis).
   $$Y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad Y|0\rangle = i|1\rangle, \quad Y|1\rangle = -i|0\rangle$$

4. **Pauli-Z Gate ($Z$):** Quantum phase-flip operator (rotation by $\pi$ around the Z-axis).
   $$Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}, \quad Z|0\rangle = |0\rangle, \quad Z|1\rangle = -|1\rangle$$

5. **Hadamard Gate ($H$):** Transforms between computational and diagonal bases (rotation by $\pi$ around the axis $(X+Z)/\sqrt{2}$).
   $$H = \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}, \quad H|0\rangle = |+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}, \quad H|1\rangle = |-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$$

6. **Phase Gate ($S = \sqrt{Z}$):** Quarter-turn around the Z-axis ($\pi/2$ phase shift).
   $$S = \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/2} \end{pmatrix}$$

7. **T Gate ($T = \sqrt{S} = Z^{1/4}$):** Eighth-turn around the Z-axis ($\pi/4$ phase shift).
   $$T = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$$

---

## 4. Multi-Qubit Systems and Quantum Entanglement

For an $n$-qubit system, the composite Hilbert space is formed by the Kronecker (tensor) product:

$$\mathcal{H}_{total} = \mathcal{H}_1 \otimes \mathcal{H}_2 \otimes \dots \otimes \mathcal{H}_n = (\mathbb{C}^2)^{\otimes n} \cong \mathbb{C}^{2^n}$$

### 4.1 Separability vs. Entanglement

A bipartite state $|\psi_{AB}\rangle \in \mathcal{H}_A \otimes \mathcal{H}_B$ is **separable** (a product state) if there exist subsystem states $|\phi_A\rangle \in \mathcal{H}_A$ and $|\chi_B\rangle \in \mathcal{H}_B$ such that:

$$|\psi_{AB}\rangle = |\phi_A\rangle \otimes |\chi_B\rangle$$

A state is **entangled** if it cannot be decomposed as a product of subsystem states.

### 4.2 Controlled-NOT (CNOT / $CX$) Gate
The two-qubit Controlled-NOT gate applies a Pauli-$X$ to the target qubit conditioned on the control qubit residing in $|1\rangle$:

$$CX = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes X = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}$$

### 4.3 The Canonical Bell Basis

Applying a Hadamard gate followed by a CNOT gate produces maximal entanglement:

$$(CX)(H \otimes I)|00\rangle = CX \left( \frac{|00\rangle + |10\rangle}{\sqrt{2}} \right) = \frac{|00\rangle + |11\rangle}{\sqrt{2}} = |\Phi^+\rangle$$

The complete orthonormal Bell basis is:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$$
$$|\Phi^-\rangle = \frac{|00\rangle - |11\rangle}{\sqrt{2}} = (Z \otimes I)|\Phi^+\rangle$$
$$|\Psi^+\rangle = \frac{|01\rangle + |10\rangle}{\sqrt{2}} = (I \otimes X)|\Phi^+\rangle$$
$$|\Psi^-\rangle = \frac{|01\rangle - |10\rangle}{\sqrt{2}} = (Z \otimes X)|\Phi^+\rangle$$

---

## 5. Bell's Theorem and the CHSH Inequality

### 5.1 Local Realism
Local Realism assumes:
1. **Realism:** Physical properties have definite values prior to observation.
2. **Locality:** Physical influences cannot propagate faster than the speed of light in vacuum ($c$).

Under any Local Hidden Variable (LHV) theory parameterized by hidden variables $\lambda$ with distribution $\rho(\lambda)$, the correlation $E(a, b)$ between measurements along directions $a$ and $b$ satisfies the **Clauser-Horne-Shimony-Holt (CHSH) inequality**:

$$S = E(a, b) + E(a, b') + E(a', b) - E(a', b') \le 2$$

### 5.2 Quantum Mechanical Violation
In quantum mechanics, when measuring spin observables along rotated axes $\theta_A$ and $\theta_B$ in the $X$-$Z$ plane on $|\Phi^+\rangle$, the correlation is:

$$E(\theta_A, \theta_B) = \cos(\theta_A - \theta_B)$$

Choosing the optimal angles:
- $a = 0$
- $a' = \pi/2$
- $b = \pi/4$
- $b' = -\pi/4$

Gives:
$$E(a, b) = \cos(-\pi/4) = \frac{1}{\sqrt{2}}$$
$$E(a, b') = \cos(\pi/4) = \frac{1}{\sqrt{2}}$$
$$E(a', b) = \cos(\pi/4) = \frac{1}{\sqrt{2}}$$
$$E(a', b') = \cos(3\pi/4) = -\frac{1}{\sqrt{2}}$$

Thus:
$$S_{QM} = \frac{1}{\sqrt{2}} + \frac{1}{\sqrt{2}} + \frac{1}{\sqrt{2}} - \left(-\frac{1}{\sqrt{2}}\right) = 2\sqrt{2} \approx 2.8284 > 2$$

This rigorously rules out any local hidden variable interpretation of quantum mechanics.

---

## 6. Quantum Noise and Open Quantum Systems

A quantum system interacting with its environment undergoes non-unitary evolution described by completely positive trace-preserving (CPTP) maps via **Kraus operators** $\{E_k\}$:

$$\rho \to \mathcal{E}(\rho) = \sum_k E_k \rho E_k^\dagger, \quad \sum_k E_k^\dagger E_k = I$$

### 6.1 Noise Channels
1. **Bit-Flip Channel:** $E_0 = \sqrt{1-p}I$, $E_1 = \sqrt{p}X$
2. **Phase-Flip Channel:** $E_0 = \sqrt{1-p}I$, $E_1 = \sqrt{p}Z$
3. **Depolarizing Channel:** $E_0 = \sqrt{1-p}I$, $E_1 = \sqrt{\frac{p}{3}}X$, $E_2 = \sqrt{\frac{p}{3}}Y$, $E_3 = \sqrt{\frac{p}{3}}Z$

### 6.2 Quantum State Fidelity
To compare an ideal pure state $|\psi\rangle$ with a noisy density matrix $\rho$, we evaluate:

$$\mathcal{F}(|\psi\rangle, \rho) = \langle \psi | \rho | \psi \rangle \in [0, 1]$$

Where $\mathcal{F} = 1$ denotes identical quantum states.
