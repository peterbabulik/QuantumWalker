## Glossary of Key Terminology

This glossary defines terms relevant to the simulations and theoretical discussions in this project, focusing on Quantum Walks (QW), Cellular Automata (CA), and their connection to computational models of physics.

**Core Concepts:**

*   **Grid / Lattice:**
    A discrete structure of sites (or cells) arranged in a regular pattern, typically in 1D (a line) or 2D (a grid). This forms the "space" on which our simulations take place.
*   **State Vector (`ψ`):**
    In quantum mechanics, a vector in a Hilbert space that completely describes the state of a quantum system. Its components are complex numbers (amplitudes), and the squared magnitude of an amplitude gives the probability of measuring the system in the corresponding basis state.
*   **Qubit (Quantum Bit):**
    The basic unit of quantum information. Like a classical bit (0 or 1), but it can also exist in a superposition of both `|0⟩` and `|1⟩` simultaneously. It can also be entangled with other qubits.
*   **Superposition:**
    A fundamental principle of quantum mechanics where a quantum system can be in multiple states at the same time. For example, a qubit can be in a state that is a combination of `|0⟩` and `|1⟩`. A quantum walker can be in a superposition of being at multiple sites simultaneously.
*   **Entanglement:**
    A quantum mechanical phenomenon in which the quantum states of two or more objects are linked in such a way that they must be described in reference to each other, even though the individual objects may be spatially separated. Measurements performed on one entangled particle instantaneously influence the state of the other(s).
*   **Unitary Operator / Unitary Evolution:**
    In quantum mechanics, the time evolution of a closed quantum system is described by a unitary operator. Unitary operators preserve the norm (total probability) of the state vector and are reversible. All quantum gates are unitary.
*   **Operator (`U`, `C`, `S`, `P`):**
    A mathematical (often matrix) representation of an operation that transforms the state of the system.
    *   `U_step`: The operator for one complete time step of the QW/CA.
    *   `C`: Coin operator.
    *   `S`: Shift operator.
    *   `P`: Potential operator.
*   **Emergence / Emergent Properties:**
    Complex behaviors, patterns, or properties that arise from the interactions of many simpler components, where these global properties are not easily predicted by looking at the individual components or rules in isolation. (e.g., the probability distribution of a QW, the "attraction" of walkers).

**Quantum Walk (QW) Specifics:**

*   **Quantum Walk (QW):**
    The quantum mechanical analogue of a classical random walk. A "walker" (a quantum particle or excitation) moves on a lattice. Its movement is governed by quantum rules, leading to superposition, interference, and entanglement.
*   **Coin (QW):**
    An internal quantum degree offreedom of the walker (typically one or more qubits). The state of the coin is manipulated by a "coin operator."
*   **Coin Operator `C` (QW):**
    A unitary operation applied to the walker's coin qubits at each step. It typically puts the coin into a superposition, which then determines the probabilities of moving in different directions. (e.g., Hadamard coin, Grover coin, DFT coin).
*   **Shift Operator `S` (QW):**
    A unitary operation that moves the walker on the lattice *conditionally* based on the state of its coin. For example, if coin is `|0⟩`, move left; if `|1⟩`, move right. This operation entangles the coin state with the position state.
*   **Ballistic Spreading (QW):**
    A characteristic feature of QWs where the standard deviation of the walker's position distribution grows linearly with time (`σ ∝ t`), much faster than the `σ ∝ √t` of classical random walks. This is due to quantum interference.
*   **Coin-Position Entanglement (QW):**
    The entanglement generated between the walker's internal coin state and its spatial position state. It's a measure of the "quantumness" of the walk.

**Cellular Automaton (CA) Specifics:**

*   **Cellular Automaton (CA):**
    A discrete model consisting of a grid of cells, each of which can be in one of a finite number of states. The state of each cell at each time step is determined by a fixed set of rules based on the states of its neighboring cells.
*   **Classical CA:** Cells have classical states (e.g., 0 or 1). Rules are typically deterministic boolean functions. (e.g., Rule 30, Rule 90, Rule 110, Conway's Game of Life).
*   **Quantum Cellular Automaton (QCA):**
    The quantum mechanical generalization of a CA. Cells are quantum systems (e.g., qubits), and the local update rule is a unitary operator applied simultaneously across neighborhoods. QWs can be considered a type of QCA.
*   **Rule 30, 90, 110 (ECA):**
    Specific Elementary Cellular Automata rules defined by Stephen Wolfram, known for producing complex patterns from simple local rules. Rule 110 is Turing complete.
*   **CA Substrate / Environment:**
    In our coupled QW-CA models, the CA acts as a (potentially dynamic) background or environment whose state influences the QW's local operations (e.g., the coin flip).
*   **Feedback (QW to CA):**
    A mechanism where the state of the quantum system (e.g., probability distribution of the QW) influences the state of the classical CA for its next update. This creates a two-way coupling.

**Theoretical Concepts:**

*   **Quantized Information:**
    The idea that information, like energy or charge, might exist in discrete, indivisible units at a fundamental level. In quantum information theory, the qubit is the basic unit.
*   **Algorithmic Geometry / Shape of Algorithm:**
    The concept that the rules and structure of an algorithm (its connectivity, causal flow, and how it processes information) define an abstract "geometry." The patterns and structures that emerge from running the algorithm (e.g., in the state space or output) are reflections of this algorithmic geometry.
*   **Computational Universe / Digital Physics:**
    A set of hypotheses suggesting that the universe itself is fundamentally discrete and computational at its deepest level, with physical laws being the result of an underlying computation.
*   **Measurement Problem / Quantum-to-Classical Transition:**
    A fundamental issue in quantum mechanics concerning how definite outcomes arise from quantum superpositions during a measurement, and how classical behavior emerges from underlying quantum reality. Our two-phase conceptual equation (`Wave -> Measure -> Particle`) is a toy model for this.
*   **Decoherence:**
    The process by which a quantum system loses its quantum properties (like superposition and entanglement) due to interaction with its environment. This is often considered a key mechanism for the emergence of classical behavior.
