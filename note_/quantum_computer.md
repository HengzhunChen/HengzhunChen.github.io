---
layout: splash
author_profile: false
---

<h1> Quantum Computer </h1>

<h2>Content</h2>

- [Quantum Phenomena](#quantum-phenomena)

    - [Quantum Probability and Classical Probability](#quantum-probability-and-classical-probability)

    - [Quantum Coherence and Entanglement](#quantum-coherence-and-entanglement)

- [Quantum Environment](#quantum-environment)

    - [Quantum Noise](#quantum-noise)

    - [Quantum Error Correction](#quantum-error-correction)

- [Quantum Computer](#quantum-computer)

    - [Fundamental Questions](#fundamental-questions)

    - [Frequently Used Preliminaries](#frequently-used-preliminaries)

    - [Quantum Algorithms](#quantum-algorithms)


----------------------------------------------------------------------

# Quantum Phenomena

Below are some interesting and even counter-intuitive phenomena in the quantum world.

## Quantum Probability and Classical Probability

For a classical probability distribution with a mixed state, we have states $\psi_k$, $k=1, \cdots, m$, each with a probability $p_k$ such that 

$$ \sum_{i=1}^m p_i = 1.$$ 

For a quantum probability distribution within a pure state, we have superposition $\vert\psi \rangle =\sum_{i=1}^m p_i \vert\psi_i\rangle$ such that

$$ \sum_{i=1}^m |p_i|^2 = 1. $$

The classical probability distribution of mixed states is not indication of a property inherent to the quantum system itself but rather serves as a reflection of the observer's limitations in accurately describing the system. The "lack of knowledge" associated with a mixed state is fundamentally different from the inherent quantum uncertainty that exists even when we possess complete information, as is the case with pure states.

**Remark**
In a Mathematical view, it is easy to convert the classical probability into quantum probability, which is adopted as the well-known technique, Purification.

**Remark**
The square relation between wavefunction amplitude and probability interpretation also underscores an essential distinction between quantum and classical physics, i.e., the phenomenon of quantum coherence.


**Remark**
The quadratic speedup of the Grover's algorithm for unstructured search problem can be explained as: while classical probabilistic algorithms work with probability densities, quantum algorithms works with wavefunction amplitudes, of which the square gives the probability densities.

---------------------------------------------------------------------------------


## Quantum Coherence and Entanglement

### Quantum Coherence for individual systems

For pure state, given two states $\vert \psi\rangle = \sum_i \psi_i \vert i\rangle$, $\vert \varphi\rangle=\sum_i \varphi_i \vert i\rangle$ with orthonormal basis $\vert i\rangle$, the probability amplitude of finding $\vert \psi\rangle$ and $\vert \varphi\rangle$ or vice versa can be written as 

$$ \langle \varphi | \psi \rangle = \sum_i \varphi_i^* \psi_i, $$

and the associated probability is 

$$ |\langle \varphi | \psi \rangle |^2 = \sum_i |\varphi_i^* \psi_i|^2 + \sum_{i\neq j} \varphi_i^* \varphi_i \psi_j \psi_j^*. $$

The second summation above gives the coherence terms, which introduces interference effects that can significantly influence the system's behavior.

For mixed state, the diagonal matrix element $\rho_{nn}=\text{Tr}(\rho \vert n\rangle\langle n\vert )$ corresponds to the first term in summation of pure state, the off-diagonal elements $\rho_{nm}=\text{Tr}(\rho \vert n\rangle\langle m\vert )$ are the counterparts of the coherence terms.

**Remark**
The influence of an external environment can diminish the coherence terms, giving rise to quantum decoherence, which is partly discussed in the noisy quantum channel topic.

**Remark**
Quantum coherence is basis-dependent, while there always exist a basis that diagonalizes density $\rho$, this may lead to the misconception that quantum coherence is easily removable by changing the basis. However, diagonalizing a density operator is not equivalent to quantum coherence, which invovles an interaction with an external environment. Furthermore, it is generally not possible for a density operator to commute with the Hamiltonian, which leads to a state not evolve in time. 


### Quantum entanglement for composite Systems

The Hilbert space of the composite system is the tensor product of the Hilbert space of subsystems, i.e.,

$$\mathscr{H} = \mathscr{H}_1 \otimes \mathscr{H}_2 \otimes \cdots $$

A state $\vert \psi\rangle \in \mathscr{H}_1 \otimes \mathscr{H}_2$ is termed separable if there exists states $\vert \psi_1\rangle \in \mathscr{H}_1$ and $\vert \psi_2\rangle \in \mathscr{H}_2$ such that 

$$ |\psi\rangle =|\psi_1\rangle \otimes |\psi_2\rangle, $$

while those non-separable states are termed entangled states. 

To further explore the property of entanglement, consider the two subsystems as spin-1/2 systems.
Suppose Charlie prepares a Bell state 

$$ |\psi\rangle = \frac{1}{\sqrt{2}}(|\uparrow\downarrow~\rangle - |\downarrow\uparrow~\rangle) $$

and gives one of them to Alice and Bob respectively, even if Alice and Bob, who are the observers for subsystems 1 and 2 respectively, are separated by any distance, Alice’s and Bob’s measurement outcomes are entangled. Specifically, if Alice measures her spin in the $\vert \uparrow~\rangle$ state, then, by the postulates of quantum mechanics, Bob will find his spin in the $\vert \downarrow~\rangle$ state, and vice versa.

Let's take a look at a classical "analog". Consider a pair of black and white balls placed into two boxes, which are then randomly given to Alice and Bob. No matter their distance apart, if Alice finds a black ball in her box, Bob’s must contain a white one, and vice versa. Nevertheless, this classical scenario fails to capture the true essence of quantum entanglement.

Consider a basis transformation for Bell state,

$$ |\pm\rangle = \frac{1}{\sqrt{2}} (|\uparrow~\rangle \pm |\downarrow~\rangle), $$

which changes the Bell state to

$$ |\psi\rangle = \frac{1}{\sqrt{2}}(|+-\rangle - |-+\rangle) $$

while preserving the entanglement. Alice and Bob are not only free to choose their basis
for measurements, but also find that the entanglement persists regardless of the basis used.

Some might mistake that quantum entanglement allows for instantaneous communication
between distant parties. Although Alice’s and Bob’s measurement outcomes are correlated due to the entangled state, neither can immediately know the result of the other’s measurement without some form of communication. Moreover, to fully decipher the correlation and make use of the entanglement, classical communication is essential for sharing the measurement outcomes and bases used. Thus, regardless of whether classical or quantum communication is used, the speed-of-light constraint remains intact. Special relativity holds in both the classical and quantum realms, as substantiated by extensive experimental evidence to date. Besides, the influence of environment may lead to decoherence and break the entanglement of the two subsystems, just as the noisy quantum channel process.


### EPR Paradox and Bell Inequality

The goal of EPR was to show that quantum mechanics is incomplete, by demonstrating that quantum mechanics lacked some essential 'element of reality', by their criterion. They hoped to force a return to a more classical view of the world, one in which systems could be ascribed properties which existed independently of measurements performed on those systems. Unfortunately for EPR, most physicists did not accept the above reasoning as convincing. The attempt to impose on Nature by fiat properties which she must obey seems a most peculiar way of studying her laws.

Nearly thirty years after the EPR paper was published, an experimental test was proposed that could be used to check whether or not the picture of the world which EPR were hoping to force a return to is valid or not. It turns out that Nature experimentally invalidates that point of view, while agreeing
with quantum mechanics. The key to this experimental invalidation is a result known as Bell’s inequality. Bell’s inequality is not a result about quantum mechanics, and it make two assumptions based on our common sense:

- The physical properties $P_Q$ has definite values $Q$ which exist independent of observation. (Realism)

- Alice performing her measurement does not influence the result of Bob's measurement. (Locality)

However, quantum mechanics, which possess entanglement phenomenon, gives predictions violating with the Bell inequality. And the experiments shows that the Bell inequality is not obeyed by Nature, which implies that at least one of these assumptions is not correct! Our world is not locally realistic!

--------------------------------------------------------


# Quantum Environment

Below we discuss how the environment-system interaction affecting a "quantum machine".

## Quantum Noise

Real systems suffer from unwanted interactions with the outside world. These unwanted interactions show up as *noise* in quantum information processing systems. We should find a model for the environment and for the system-environment interaction.

**Assumption(Markovicity)**: The consecutive noise processes act independently.

The assumption above actually results in a stochastic process $X\rightarrow Y \rightarrow Z$ of a special type known as Markov process.

### Discrete Tool: Kraus Representation and Noisy Quantum Channel

A natural way to describe the dynamics of an open system is to regard it as arising from an interaction between the system of interest, which we shall call the principle system, and an environment, which together form a closed quantum system. 

Assume that the system and its environment are uncorrelated initially and the environment's initial state is purified. The we just perform a partial trace over the environment to obtain the reduced state of the system alone:

$$ \mathcal{E}(\rho) = \text{tr}_{\text{env}} \left[ U(\rho \otimes \rho_{\text{env}}) U^{\dagger} \right]. $$

Note that $\rho_S \otimes \vert \varepsilon_0\rangle \langle \varepsilon_0 \vert  = (1_S \otimes \vert \varepsilon_0\rangle) (\rho_S \otimes 1_E)(1_S \otimes \langle \varepsilon_0 \vert )$, define Kraus operator  $E_a:=\langle \varepsilon_a \vert  U(t) \vert  \varepsilon_a \rangle$, where $\{\vert \varepsilon_a \rangle\}$ is a basis for environment, then we have kraus representation (operator sum representation):

$$ \mathcal{E}(\rho_S) = \sum_a E_a(t) \rho_S E_a^\dagger(t), \quad \sum_a E_a^\dagger(t)E_a(t)=1.$$

Given a noisy quantum channel (NQC), one typical way to describe it is through a probabilistic mixture of unitary transformations, 

$$ \mathcal{E}(\rho) = \sum_a p_a U_a \rho U_a^{\dagger}, \quad \sum_a p_a=1. $$

Although an NQC consists of a set of unitary, its overall effect on a density matrix of the system is non-unitary in general. 

One interesting question is, given a system going through an NQC $\{U_a, p_a\}$ represented by certain Kraus operators, can we augment the system by attaching to it an environment, such that the NQC on the total composite system is represented by a unitary transformation?

This can always be achieved by introducing the environment with $\mathscr{H}=\text{span}\{\vert \varepsilon_a\rangle\}$ and let 

$$ U=\sum_a U_a \otimes |\varepsilon_a\rangle \langle \varepsilon_a |, \quad \text{and} \quad \rho_E = \sum_a p_a |\varepsilon_a \rangle \langle \varepsilon_a |. $$

This representation provides a more intuitive and visualizable depiction of quantum operations,, allowing one to think of the action of a quantum channel as a unitary evolution on a larger system. In essence, it extends the idea of unitary evolution, a core principle of quantum mechanics, to encompass non-unitary processes. This has been instrumental in understanding quantum-to-classical transitions and developing tools and protocols in quantum information theory.


### Continuous Tool: Lindblad (master) Equation

The master equation describes the continuous temporal evolution of open quantum systems. There are several derivations of the equation. Some of them are based on a microscopic model and gives a clear physical picture of the approximations made to arrive at the master equation. One of them just clarifies the link between the master-equation approach and the quantum-operation formalism.

Before going into technical details, let us state the main approximations involved in deriving the master equation:

- Born approximation: The environment is large and practically unaffected by interaction with system.

- Markov approximation: The environment is memoryless, its state at time $t_0$ is essentially unaffected by the history of the system.

Consider the infinitesimal time evolution of the system's state:

$$ \rho_S(t+\mathrm{d}t)= \mathcal{E}(t+\mathrm{d}t, t) \rho_S(t) = \sum_{k=0}^{M-1} E_k \rho_S(t) E_k^\dagger, $$

where 

$$ E_0 = 1_S + \frac{1}{\hbar}(-\mathrm{i}\widetilde{H} + K) \mathrm{d}t, \quad E_k = L_k \sqrt{\mathrm{d}t}. $$

For $\mathrm{d}t=0$, $E_0=1_S$ remains, aligning with $\mathcal{E}(t, t)=1_S$.

$\widetilde{H}$ is an effective Hamiltonian that acknowledge the interaction between the system and environment. 

$K = \frac{\hbar}{2}\sum_{k=1}^{M-1} L_k^{\dagger}L_k$ is defined from the completeness condition $\sum_k E_k^\dagger E_k=1_S$.

By the Born-Marhov approximation, we have

$$ \rho_S(t+\mathrm{d}t) = \rho_S(t) + \dot{\rho}_S(t)\mathrm{d}t + O(\mathrm{d}t^2), $$

compare with the Kraus representation, it leads to the master equation

$$ \frac{\partial \rho(t)}{\partial t} = \frac{\mathrm{i}}{\hbar}[\widetilde{H}, \rho(t)] + \sum_{k=1}^{M-1} \left( L_k\rho_S(t)L_k^\dagger - \frac{1}{2} L_k^\dagger L_k \rho_S(t) - \frac{1}{2} \rho_S(t)L_k^\dagger L_k \right). $$


### Fact

- All density matrices forms a bounded subset in the set of nonnegative symmetric matrices. 

    Given two density matrices, $\rho_1$ and $\rho_2$, we have,

    $$ \|\rho_1 - \rho_2\|_F^2 = \|\rho_1\|_F^2 + \|\rho_2\|_F^2 - 2\mathrm{tr}(\rho_1\rho_2) \leq 1 + 1 - 2\mathrm{tr}(\rho_1\rho_2) \leq 2, $$

    where the inequality is due to the trace property of density matrix and Ruhe's trace inequality.

-------------------------------------------------

## Quantum Error Correction

### Fault-tolerance

Fault-tolerance encapsulates the ability of a quantum error correction code (QECC) to endure errors that might transpire during the error correction phase itself, in addition to the routine operation of quantum computation.

### Error Structure

An error on a qubit is caused by an NQC that couples the qubit with its environment, which is represented by a unitary operator $U$ acting on the composite system of the qubit and the environment. Hence,

$$ U|\psi\rangle |\varepsilon_0\rangle = \sum_{a=1}^4\sum_{n=0}^1 (|n\rangle|\varepsilon_a\rangle\langle n | \langle \varepsilon_a|) U |\psi\rangle |\varepsilon_0 \rangle = ... = \sum_{a=1}^4 E_a|\psi\rangle|\varepsilon_a\rangle, $$

where $\vert \varepsilon_0\rangle$ is any initial state of the environment, $\{\varepsilon_a\}$ is an orthonormal basis of the environment and $E_a:=\langle \varepsilon_a\vert  U \vert \varepsilon_0\rangle$ are Kraus operators that represent the NQC. On a single qubit, any Kraus operator $E_a$ can be expressed in terms of the operators in the set

$$ E_a \in \{ 1_2, \sigma_x, \sigma_y, \sigma_z \}. $$

Note that an $n$-qubit error is also qubit-by-qubit, $n$-qubit Kraus operators can be expressed as tensor products of these Pauli operators. Hence, we have the group of errors

$$ \mathcal{G}_n = \{\pm 1, \pm \mathrm{i} \} \times \{ 1_2, \sigma_x, \sigma_y, \sigma_z\}^{\otimes n}. $$

This Pauli group is $2n$-dimensional in the sense that it can be generated by the products of $2n$ generators:

$$ \{\sigma_z^{(i)}, \sigma_x^{(j)} | i, j = n-1, n-2, \cdots, 1, 0\}. $$

Properties of $n$-qubit Pauli group: Suppose $M, M'\in \mathcal{G}_n$, 

- $M^{-1} = M^{\dagger}$ 

- $M^2 = \pm 1_2^{\otimes n}$

- $MM' = \pm M' M$, i.e., any two elements in Pauli group are either commute or anti-commute

Now we arrive at the two core problems of quantum error correction:

- Q: What is the condition for correctable code?

- Q: How to implement the logical operator?

These problems can be answered entirely using the stabilizer code below.

### Stabilizer Code

Now we have the Hilbert space of the physical qubits and the group of operators (Pauli group only) acting on the physical qubits, and our target is to use the group action to encode the quantum circuit for logical qubit.

For the purpose of quantum error correction, we need to arrange three components of the Pauli group:

- Operators for error syndrome detection measurement $\Rightarrow$ stabilizer

- Operators implement the logical qubit $\Rightarrow$ centralizer

- Operators realize the "real" error, which maybe uncorrectable

#### Error detection

Now given an Abelian subgroup $\mathcal{S} \subseteq \mathcal{G_n}$ as the stabilizer subgroup, all the elements of $\mathcal{S}$ acting on  $\mathscr{H_{2^n}}$ can be simultaneously diagonalized. Its action over the Hilbert space of physical qubit $\mathscr{H_{2^n}}$ gives a subspace defined as the simultaneous eigenspace with eigenvalue of all the elements of $\mathcal{S}$:

$$ \mathscr{H}_{\mathcal{S}} = \{ |\psi\rangle \in \mathscr{H}_{2^n} : M |\psi\rangle = |\psi\rangle, \forall M \in \mathcal{S} \}. $$

This subspace is called the stabilizer code space associated with $\mathcal{S}$ since they are preserved under the action of $\mathcal{S}$. If $\mathcal{S}$ has $n-k$ generators, it can encode $k$ logical qubits.

**NOTE:** Here we assume that only those $\mathcal{G}_n \setminus \mathcal{S}$ as the potential source of errors with respect to $\mathcal{S}$.

**NOTE**: $-I_{2^n} \notin \mathcal{S}$, which shows that $M^2 = I_{2^n}$. This is important for the projection operator later.

The stabilizer formalism provides a simple way to characterize errors that the code can detect and correct. We may think of the $n-k$ generators $\{M_1, \cdots, M_{n-k}\}$ as the check operators of the code, or, in other words, the observables that one needs to measure in order to diagnose the errors. 

Given any $E\in\mathcal{G}_n$, $M\in \mathcal{S}$, then $EM=ME$ or $EM=-EM$,
- if $EM=ME$, for $\vert \psi\rangle \in \mathscr{H}_{\mathcal{S}}$, $ME\vert \psi\rangle = EM \vert \psi\rangle = E\vert \psi\rangle$, error preserve;
- if $EM=-ME$, for $\vert \psi\rangle \in \mathscr{H}_{\mathcal{S}}$, $ME\vert \psi\rangle = -EM \vert \psi\rangle = -E\vert \psi\rangle$, error detect.

In general, if $E_a$ is correctable, we have 

$$ M_i E_a |\psi\rangle = (-1)^{s_{ia}}E_a M_i |\psi\rangle = (-1)^{s_{ia}} E_a |\psi\rangle, \forall |\psi\rangle \in \mathscr{H}_{\mathcal{S}}, $$

the binary string $s_{1a}s_{2a}\cdot s_{(n-k)a}$ forms the error syndrome of $E_a$.

#### Correctable error

We denote $\mathcal{E}_c$ as th subset of correctable errors. Let us discuss what conditions should be satisfied to allow error correction.

- Correctable errors should map two different codewords $\vert i_L\rangle$ and $\vert j_L\rangle$ into orthogonal states:

$$ \langle i_L | E_a^\dagger E_b | j_L\rangle = 0 \quad \text{for } i \neq j, E_a, E_b \in \mathcal{E}_c. $$

If this condition were not satisfied, then the states $E_a\vert i_L\rangle$ and $E_b\vert j_L\rangle$ could not be distinguished with certainty and therefore perfect error correction would be impossible. (Think about the cube of the eight 3-qubit basis states.)

- For any correctable errors $E_a$ and $E_b$, 

$$ \langle i_L | E_a^\dagger E_b | j_L\rangle = C_{ab}, $$

where $C_{ab}$ does not depend on the state $\vert i_L\rangle$. If this were not the case, we would obtain some information on the encoded state from the measurement of the error syndrome. For example, 

$$ (p\langle 000| + q \langle 111|) (1_2 \otimes 1_2 \otimes 1_2)^\dagger (\sigma_x \otimes \sigma_x \otimes \sigma_x) (p|000\rangle + q|111\rangle) = 2pq, $$

with $p^2+q^2=1$ the syndrome measurement process could extract the information, thereby affecting the logical state.

**Remark**: A QEC code is termed non-degenerate if $C_{ab}=\delta_{ab}$; otherwise, it is degenerate. Non-degenerate codes are simple and ease to implement but may require more qubits for equivalent error correction capabilities. Degenerate codes, conversely, provide greater efficiency in qubit usage but at the cost of increased complexity in error diagnosis and correction.


#### Logical operator

A logical operator on a logical qubit is an undetectable and uncorrectable error operator, commuting with the entire stabilizer $\mathcal{S}$ (detectable error will anti-commute with at least one element of $\mathcal{S}$).

The set of operators in $\mathcal{G}_n$ that commute with the entire $\mathcal{S}$ forms a subgroup $\mathcal{Z}(S)\subseteq \mathcal{G}_n$, known as the centralizer of $\mathcal{S}$. Furthermore,

$$ \text{dim} \mathcal{Z}(\mathcal{S}) = 2n - (n-k) = n+k, $$

we can select $n-k$ generators of $\mathcal{Z}(\mathcal{S})$ as the $n-k$ generator of $\mathcal{S}$, leaving exactly $2k$ generators to act as logical operators on the $k$ logical qubits. These $2k$ generators can be denoted as single-logical-qubit operators:

$$ \{\Sigma_l^z, \Sigma_l^x : l= 1, 2, \cdots, k\}, $$

their action on basis states are 

$$ 
\begin{align*}
& \Sigma_l^z | i_{L_{k-1}}i_{L_{k-2}} \cdots i_{L_{l}} \cdots i_{L_{0}} \rangle = (-1)^{i_{L_l}} | i_{L_{k-1}}i_{L_{k-2}} \cdots i_{L_{l}} \cdots i_{L_{0}} \rangle,  \\ 
& \Sigma_l^x | i_{L_{k-1}}i_{L_{k-2}} \cdots i_{L_{l}} \cdots i_{L_{0}} \rangle = | i_{L_{k-1}}i_{L_{k-2}} \cdots \overline{i_{L_{l}}} \cdots i_{L_{0}} \rangle. 
\end{align*}
$$

**Remark** The normalizer $\mathcal{N}(\mathcal{S})$ is defined as all the elements $E\in\mathcal{G}_n$ meeting the condition $EME^\dagger\in \mathcal{S}, \forall M \in \mathcal{S}$. And we have $\mathcal{N}(\mathcal{S})=\mathcal{Z}(\mathcal{S}).$


### A Physical Interpretation

For an $[[n, k, d]]$ stabilizer code $\mathscr{H_{\mathcal{S}}}$, we aim at constructing an $n$-qubit Hamiltonian whose ground state Hilbert space aligns with the code space of the stabilizer code. Note that the code space is the $+1$ eigenspace of the stabilizer $\mathcal{S}$, generated by $n-k$ Pauli operators $M_1$ to $M_{n-k}$, hence

$$ H = -\sum_{i=1}^{n-k} M_i. $$


#### The Symmetry

The code space being ground states of $H$, share identical energy eigenvalue. Energy degeneracy in quantum system typically indicates the presence of a global symmetry within the system and its Hamiltonian. To distinguish these codewords, one need to find a set of symmetry operators and their generators, which commute with the Hamiltonian. In this case, the generators of symmetry operator happen to be the centralizer $\mathcal{Z}(\mathcal{S})$ of the stabilizer $\mathcal{S}$. The non-trivial symmetry generators of quotient space $\mathcal{Z}(\mathcal{S})/\mathcal{S}$ (select a representative operator), $\Sigma_1$ to $\Sigma_k$, enable transitions between different basis of ground states.

Therefore, we can equate the nontrivial symmetry generators with the logical operators of the code.

**Remark**: 

According to quantum mechanics, degenerate energy eigenstates, e.g., ground states, of a system implies that the system has a global symmetry, whose action on the system commutes with the system’s Hamiltonian. The symmetry selects a basis of the degenerate energy eigenstates and endow them with extra good quantum numbers to distinguish the otherwise indistinguishable states by solely their identical energy. Such a global symmetry is generally described by a group, which is produced by a few generators—the symmetry generators—via multiplication.

Note however that although the symmetry generators all commute with the system’s Hamiltonian, they may not all commute among themselves. Only the mutually commuting symmetry generators yield the good quantum numbers and thus are responsible of defining the basis of the degenerate space of states. The symmetry generators that do not commute with the mutually commuting ones can transform the basis states into one another.

In our current situation, given the degenerate, the mutually commuting symmetry generators play precisely the role of logical $Z$ operators, while those not commuting with the mutually commuting ones play the role of logical $X$ operators.

#### Basis of Ground State

Q: Can we explicitly construct the basis ground-state of the Hamiltonian?

A: This can be achievable by constructing projectors from the total Hilbert space to the ground-state subspace.

Since the ground states are simultaneous $+1$ eigenstates of the stabilizer generators $M_1$ through $M_{n-k}$, each $M_i$ have only eigenvalue $\pm 1$ by $M^{-1}=M^{\dagger}$ and $M^2=1_{2^n}$, the projector can be

$$ P_0 = \prod_{i=1}^{n-k} \frac{1_{2^n} + M_i}{2}, $$

it is easy to verify that $P_0^2=P_0$ and it maps all the $-1$ eigenstate component to $0$.

Applying $P_0$ to any basis state in the total Hilbert space $\mathcal{H}_{2^n}$ yields a basis state of $\mathcal{H}_0$, without loss of generality, starting from

$$ \underbrace{|0_L 0_L \cdots 0_L \rangle}_{k} := P_0 \underbrace{|00\cdots 0 \rangle}_{n}, $$

the nontrivial symmetry generators $\Sigma_1^x$ through $\Sigma_k^x$ commute with $P_0$ and can be employed to generate the remaining $2^k-1$ basis ground states.

------------------------------------------------------------------------

# Quantum Computer

## Fundamental Questions

- How a quantum computer is supposed to be used to solve challenging computational problems in scientific and engineering computation?

    - Are we really trying to build a quantum computer, either to perform quantum Fourier transform or to perform a quantum search?

    - How to connect a quantum computer to scientific problems such as solving linear systems, eigenvalue problems, least squares problems, differential equations, numerical optimization etc?

- Is quantum computer at least as powerful as classical computer for most problems?
    
    - How to simulated classical universal gates efficiently with quantum circuits?

    - How to perform classical arithmetic operations with fixed points number representation?

-----------------------------------------------------------------------

## Frequently Used Preliminaries

### Postulates of quantum mechanics

- State space postulate
- Quantum operator postulate
- Quantum measurement postulate
- Tensor product postulate

### Copy operation

Definition: copy operator $U$ gives

$$ U |x\rangle \otimes |s\rangle = |x\rangle \otimes |x\rangle, $$

for any black-box state $x$ and a chosen target state $s$.

**No-cloning theorem**: a cloning operator $U$ can at most copy states which are orthogonal to each other, and a general quantum copy operation is impossible.

Two notable exceptions:

- We know how a quantum state is prepared, then we can copy this specific vector by preparing one

- The copying of classical information is true by CNOT gate, $\text{CNOT} \vert x, 0\rangle = \vert x, x\rangle$, $x \in \{0, 1\}$.


### Universal gate sets

- In the quantum setting, any unitary operators on $n$ qubits can be implemented using $1$- and $2$-qubit gates.

- There are many possible choices of universal gate sets. A set of quantum gates $\mathcal{S}$ is universal if given any unitary operator $U$ and desired precision $\epsilon$, we can find $U_1, \ldots, U_m \in \mathcal{S}$ such that $\| U - U_m U_{m-1} \cdots U_1\| \leq \epsilon$.

---------------------------------------------------------------------


## Quantum Algorithms

Here are some basic quantum algorithms whose ideas and variants are present in numerous quantum algorithms. Hence they are call "quantum primitives".

- Grover's algorithms

- Quantum Fourier transform

- Quantum phase estimation

- Trotter based Hamiltonian simulation

For more quantum algorithms, one can see the [Zoo of quantum algorithms](https://quantumalgorithmzoo.org).

