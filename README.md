# 2D Quantum Walk Simulator

This project simulates a discrete-time 2D Quantum Walk (QW) on a square lattice. It allows for exploration of various coin operators, boundary conditions, and the introduction of spatial potential fields. The primary observables calculated are the walker's probability distribution across the grid and the entanglement entropy between the walker's internal coin state and its spatial position.

## Features

*   **2D Lattice:** Simulates a quantum walker on a `ROWS` x `COLS` grid.
*   **Coin System:** Uses a 2-qubit coin, allowing for 4 distinct internal states that determine the direction of movement (North, South, East, West).
*   **Coin Operators:** Implements several common coin operators:
    *   Hadamard Coin (Tensor product of two Hadamard gates)
    *   Grover Coin (Diffusion operator)
    *   Discrete Fourier Transform (DFT) Coin
*   **Boundary Conditions:**
    *   Periodic (walker wraps around the edges)
    *   Reflecting (walker reflects off the edges, coin state is flipped)
*   **Spatial Potentials:**
    *   Ability to introduce site-dependent potential fields that apply a phase to the walker.
    *   Example potentials implemented:
        *   `potential_none`: No potential.
        *   `potential_barrier`: A linear barrier at a specified column.
        *   `potential_harmonic`: A harmonic potential centered on the grid.
*   **Observables:**
    *   **Probability Distribution:** Calculates `P(r,c)`, the probability of finding the walker at site `(r,c)` by summing over all coin states.
    *   **Coin-Position Entanglement:** Calculates the von Neumann entropy of the reduced density matrix of the coin system (by tracing out the position degree of freedom). This quantifies the entanglement between the walker's internal state and its spatial path.
    *   **Coarse-Grained Probability:** Averages probabilities over larger blocks for a macroscopic view.
*   **Visualization:**
    *   Plots the final probability distribution as a heatmap (log scale).
    *   Plots the evolution of coin-position entanglement entropy over time.
    *   Plots the final coarse-grained probability distribution.
*   **Text Summary:** Generates a detailed text summary of simulation parameters and key results, including top probability locations and entanglement values at selected time steps.

## Simulation Model

The state vector of the system has dimension `4 * ROWS * COLS`, representing the amplitude for the walker to be at each of the `ROWS * COLS` sites in one of 4 coin states. The evolution in each discrete time step `U_step` is given by:

`U_step = S @ C @ P`

Where:
*   `P`: Potential operator (diagonal matrix applying a site-dependent phase).
*   `C`: Coin operator (applies the chosen 4x4 coin matrix to the coin qubits at each site).
*   `S`: Shift operator (moves the walker to an adjacent site based on its coin state, implementing boundary conditions).

The simulation iteratively applies `U_step` to the initial state vector.

## Code Structure

*   **Parameters:** Defined at the top of the script (grid size, depth, etc.).
*   **Enums/Constants:** For boundary conditions and coin matrices.
*   **Helper Functions:**
    *   `get_index`, `get_coords_from_index`: Map between 1D state vector index and (coin, row, col) representation.
*   **`prepare_initial_state`:** Sets up the walker at a specific starting site and coin state.
*   **`build_qw_operators`:** Constructs the full `C`, `S`, and `P` operators and the combined `U_step`.
*   **`calculate_coin_position_entanglement`:** Calculates the entanglement entropy between the coin and position.
*   **`calculate_p1_position_2d`:** Calculates the probability distribution over spatial sites.
*   **`coarse_grain_probs`:** Calculates coarse-grained probabilities.
*   **`run_2d_qw_simulation`:** Main function to execute a single simulation instance.
*   **`plot_2d_qw_results`:** Generates plots for visualization.
*   **`generate_2d_qw_text_output`:** Creates the text summary.
*   **Main Execution Block:** Sets up and runs simulations with different configurations.

## How to Run

1.  Ensure you have Python 3, NumPy, Matplotlib, SciPy, and Cirq installed.
    ```bash
    pip install numpy matplotlib scipy cirq
    ```
2.  Modify the parameters in the "Parameters" section or the `settings_to_run` list in the `if __name__ == "__main__":` block to configure your desired simulation (grid size, depth, coin, boundary, potential).
3.  Run the script:
    ```bash
    python your_script_name.py
    ```
    This will generate plots and print text summaries to the console.

## Future Directions & Exploration

*   Implement other coin operators or custom potentials.
*   Explore different initial coin states (e.g., superpositions).
*   Calculate and analyze other observables (e.g., variance of position for spreading rate, return probability).
*   Optimize operator construction or evolution for larger grids (e.g., using sparse matrices if applicable, or by avoiding explicit matrix construction for `U_step` if memory becomes an issue).
*   Implement more sophisticated spatial entanglement measures (e.g., different bipartitions, mutual information).
*   Extend to different lattice geometries.

## Connection to Quantum Information & Computation

This simulation serves as a model for:
*   **Quantum Information Propagation:** How quantum states evolve and spread on a discrete lattice.
*   **Emergent Behavior:** Complex global patterns (like probability distributions) emerging from simple, local quantum rules.
*   **Quantum Computation:** The QW itself is a universal model of quantum computation.
*   **Toy Models of Physical Systems:** QWs can approximate the behavior of relativistic particles (like the Dirac equation) in certain limits.
*   **Discrete Spacetime Models:** Provides a framework for thinking about how physics might operate on a fundamental, discrete "information grid."

---
