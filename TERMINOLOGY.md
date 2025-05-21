# Glossary of Key Terminology

This glossary defines terms relevant to the simulations and theoretical discussions in this project, focusing on Quantum Walks (QW), Cellular Automata (CA), Quantum Kernel Methods, and their connection to computational models of physics.

## Core Quantum & Computational Concepts:

*   **Grid / Lattice:** A discrete structure of sites (or cells) arranged in a regular pattern, typically in 1D (a line) or 2D (a grid). This forms the "space" on which simulations take place.
*   **State Vector (`|ψ⟩`):** In quantum mechanics, a vector in a Hilbert space that completely describes the state of a quantum system. Its components are complex numbers (amplitudes), and the squared magnitude of an amplitude gives the probability of measuring the system in the corresponding basis state.
*   **Qubit (Quantum Bit):** The basic unit of quantum information. Like a classical bit (0 or 1), but it can also exist in a superposition of both `|0⟩` and `|1⟩` simultaneously. It can also be entangled with other qubits.
*   **Superposition:** A fundamental principle of quantum mechanics where a quantum system can be in multiple states at the same time. For example, a qubit can be in a state that is a combination of `|0⟩` and `|1⟩`. A quantum walker can be in a superposition of being at multiple sites simultaneously.
*   **Entanglement:** A quantum mechanical phenomenon in which the quantum states of two or more objects are linked in such a way that they must be described in reference to each other, even though the individual objects may be spatially separated. Measurements performed on one entangled particle instantaneously influence the state of the other(s).
*   **Unitary Operator / Unitary Evolution:** In quantum mechanics, the time evolution of a closed quantum system is described by a unitary operator. Unitary operators preserve the norm (total probability) of the state vector and are reversible. All quantum gates are unitary.
*   **Operator (`U`, `C`, `S`, `P`):** A mathematical (often matrix) representation of an operation that transforms the state of the system.
    *   **`U_step`:** The operator for one complete time step of a QW or QCA.
    *   **`C` (Coin Operator):** Used in Quantum Walks to act on the coin state.
    *   **`S` (Shift Operator):** Used in Quantum Walks to move the walker based on the coin.
    *   **`P` (Potential Operator):** (Not explicitly used in recent experiments but common in QW literature) An operator that applies a phase or modifies amplitude based on position.
*   **Emergence / Emergent Properties:** Complex behaviors, patterns, or properties that arise from the interactions of many simpler components, where these global properties are not easily predicted by looking at the individual components or rules in isolation (e.g., the probability distribution of a QW, glider patterns in a CA).
*   **Hilbert Space:** An abstract vector space where quantum states live. For `N` qubits, the dimension of the Hilbert space is `2^N`.
*   **Tensor Product (`⊗`):** A mathematical operation used to combine the Hilbert spaces of individual quantum systems to form the Hilbert space of the composite system.
*   **Turing Completeness (TC):** A property of a computational system (like a CA or a formal set of rules) indicating that it can simulate any Turing machine, and thus can perform any computation that any other classical computer can.
*   **Computational Irreducibility:** A property of a system whose behavior cannot be predicted by a shortcut; the only way to know its future state is to simulate its evolution step-by-step. Complex CAs often exhibit this.

## Quantum Walk (QW) Specifics:

*   **Quantum Walk (QW):** The quantum mechanical analogue of a classical random walk. A "walker" (a quantum particle or excitation) moves on a lattice. Its movement is governed by quantum rules, leading to superposition, interference, and entanglement.
*   **Coin (QW):** An internal quantum degree of freedom of the walker (typically one or more qubits). The state of the coin is manipulated by a "coin operator."
*   **Coin Operator `C` (QW):** A unitary operation applied to the walker's coin qubits at each step. It typically puts the coin into a superposition, which then determines the probabilities of moving in different directions (e.g., Hadamard coin, Grover coin).
*   **Shift Operator `S` (QW):** A unitary operation that moves the walker on the lattice conditionally based on the state of its coin. This operation entangles the coin state with the position state.
*   **Ballistic Spreading (QW):** A characteristic feature of QWs where the standard deviation of the walker's position distribution grows linearly with time (`σ ∝ t`), much faster than the `σ ∝ √t` of classical random walks.
*   **Coin-Position Entanglement (QW):** The entanglement generated between the walker's internal coin state and its spatial position state. It's a measure of the "quantumness" of the walk.
*   **Aperiodic Substrate (e.g., Fibonacci, Thue-Morse):** A lattice where the local environment or rules (e.g., coin operator) do not repeat periodically but follow a deterministic, non-periodic sequence. QWs on such substrates can exhibit fractal probability distributions and spectra.

## Cellular Automaton (CA) & Quantum-Driven CA (QCA) Specifics:

*   **Cellular Automaton (CA):** A discrete model consisting of a grid of cells, each in one of a finite number of states. The state of each cell at each time step is determined by a fixed set of rules based on the states of its neighboring cells.
    *   **Classical CA:** Cells have classical states (e.g., 0 or 1). Rules are typically deterministic Boolean functions (e.g., Rule 30, Rule 90, Rule 110).
*   **Quantum Cellular Automaton (QCA - General Definition):** The quantum mechanical generalization of a CA. Cells are quantum systems (e.g., qubits), and the local update rule is a unitary operator applied simultaneously across neighborhoods. QWs can be considered a type of QCA.
*   **Quantum-Driven Cellular Automaton (QCA - Project Specific):** The hybrid model developed in this project where:
    *   **Qunodes:** Sites on a 1D lattice, each possessing a classical "type" (e.g., A, B, C).
    *   **Quantum Interaction Step:** Based on the current classical types of neighboring qunodes, specific 2-qubit (or k-qubit) unitary quantum gates are applied to their underlying quantum degrees of freedom (qubits). This step typically involves initializing the qubits (e.g., to `|+...+>` or based on type) and then applying the interactions.
    *   **Quantum Outcome Extraction:** After quantum interaction, local quantum properties (e.g., marginal probability `P(q_i=1)` for each qunode's qubit) are extracted from the resulting global quantum state (via statevector simulation).
    *   **Classical Type Update Rule:** The classical types of the qunodes for the next time step are updated based on a classical CA-like rule that takes the current types and the extracted quantum outcomes as input.
    *   **Emergent CA:** The evolution of the classical types forms an emergent cellular automaton whose rules are indirectly shaped by the underlying quantum dynamics.
*   **CA Substrate / Environment (for QW on CA):** In earlier QW-CA models, the CA acts as a (potentially dynamic) background whose classical state influences the QW's local operations.
*   **Feedback (QW to CA):** A mechanism where the QW's state influences the classical CA's state for its next update.

## Quantum Machine Learning (QML) & Kernel Specifics:

*   **Quantum Kernel Method:** A QML technique where classical data points `X_i` are mapped to quantum states `|φ(X_i)⟩` in a Hilbert space using a quantum feature map `φ`. The similarity between data points is then computed as a kernel entry, often related to the squared inner product (fidelity) `K_ij = |⟨φ(X_i)|φ(X_j)⟩|^2`.
*   **Quantum Feature Map `φ(x)`:** A parameterized or fixed quantum circuit that encodes classical data `x` into a quantum state. In this project, a simple `Ry(x_k)` encoding was primarily used, and a "neural-inspired" map with classical preprocessing was also explored.
*   **Fidelity Estimation (for Kernel Entry):** The process of estimating `|<ψ|ϕ>|^2`.
    *   **Compute-Uncompute Method (Inverted SWAP Test):** A common technique where the circuit `U_ϕ† U_ψ |0...0⟩` is run, and the probability of measuring the all-`|0...0⟩` state gives the fidelity. This was used for QPU kernel computation.
*   **Kernel Matrix `K`:** An `N x N` matrix where `K_ij` is the kernel value between data points `X_i` and `X_j`. This matrix is then used by classical kernel machines.
*   **Support Vector Machine (SVM):** A classical supervised machine learning algorithm used for classification and regression, which can utilize kernel matrices ("kernel trick") to operate in high-dimensional feature spaces.
*   **`SamplerV2` (Qiskit Runtime):** A Qiskit Runtime primitive used to obtain measurement outcome distributions (counts) from executing quantum circuits on simulators or QPUs. Used for fidelity estimation in the QPU kernel experiments.
*   **Batching (QPU Jobs):** Submitting multiple quantum circuits as a single job to the QPU to improve throughput and manage session resources effectively.

## Theoretical Concepts (Project Specific):

*   **Quantized Information:** The core idea that information is fundamental and discrete, with the qubit as its basic quantum unit.
*   **Algorithmic Geometry / Shape of Algorithm:** The proposition that the rules and structure of an underlying fundamental quantum algorithm define an abstract "geometry," and the observable patterns and structures in the universe ("shape of information") are direct manifestations of this algorithmic geometry.
*   **Algorithmic Folding:** A conceptual model for early universe evolution, where a minimal initial quantum information state iteratively undergoes processing by a fundamental algorithm, leading to an increase in qubit count, entanglement, and overall complexity.
*   **Measurement Problem (Algorithmic Interpretation):** The project explores the idea that the quantum measurement "collapse" is not a separate physical process but an emergent "algorithmic phase change" within a universally quantum system, driven by interactions and decoherence.
*   **Dark Matter/Energy (Algorithmic Interpretation):** Speculative ideas that these phenomena might be manifestations of novel informational patterns or features of the universe's fundamental spacetime-generating algorithm.

## Error Handling & QPU Execution:

*   **NISQ (Noisy Intermediate-Scale Quantum):** Refers to current-generation quantum computers which have a limited number of qubits (50-few hundreds) and are subject to significant noise and errors.
*   **Transpilation:** The process of converting an abstract quantum circuit into a sequence of operations (native gates) that can be run on specific quantum hardware, considering its connectivity and gate set. `optimization_level` in the transpiler controls the effort to reduce circuit depth/gate count.
*   **Error Suppression:** Techniques applied during QPU execution to reduce the impact of noise, without explicit post-processing of results based on error models.
    *   **Dynamical Decoupling (DD):** Applying sequences of pulses to qubits during idle times to refocus them and mitigate decoherence.
    *   **Twirling:** Randomizing gates or measurements to average out certain types of coherent errors into more manageable stochastic noise.
*   **Error Mitigation:** Techniques that involve running additional circuits or classical post-processing based on an error model to estimate an error-free result.
    *   **Measurement Error Mitigation (MREM):** Correcting for errors that occur during the qubit readout process. (A custom MREM was explored).

---
