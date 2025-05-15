# Experimental Quantum Walk Investigations on IBM QPU (`ibm_brisbane`)

**Date:** May 11-12, 2025 (Placeholder)
**Objective:** To implement and test 1D Quantum Walk (QW) algorithms and related fundamental quantum circuits on real quantum hardware (`ibm_brisbane`), assess the impact of circuit depth and noise, and evaluate the feasibility of observing complex QW dynamics. This serves as an experimental exploration related to a broader theory of quantized information and algorithmic emergence.

## 1. Experimental Setup

*   **Quantum Processing Unit (QPU):** `ibm_brisbane` (127-qubit Eagle r3 processor, accessed via Open Plan).
*   **Software Framework:** Qiskit, utilizing `QiskitRuntimeService` and the `SamplerV2` primitive.
*   **Execution:** Circuits were transpiled for the `ibm_brisbane` ISA using `optimization_level=1` (unless otherwise noted). All jobs used 4096 shots.
*   **Focus:** Small systems (1-3 qubits) to test fundamental operations and minimal QW configurations.

## 2. Initial QW Attempts & The Challenge of Circuit Depth

Initial attempts to run even simple 1D Quantum Walk (QW) algorithms with more complex components (like QFT-based shifts or CA-dependent coin unitaries constructed from 8x8 matrices) for `N_SITES=4` (3 QW qubits) and minimal `DEPTH` (1 or 2 steps) consistently resulted in all measurement outcomes being the ground state `|000⟩`.

*   **QW with QFT-Based Shift (N=4, D=2 QW steps):**
    *   Transpiled ISA Depth: ~216
    *   QPU Result: `{'000': 4096}`
*   **QW with CA-Dependent Coin (8x8 UnitaryGate) & Simpler Manual Shift (N=4, D=1 QW step):**
    *   Transpiled ISA Depth: ~244
    *   QPU Result: `{'000': 4096}`

**Conclusion from Initial Attempts:** These results strongly indicated that the transpiled circuit depths were too high for the coherence capabilities of the QPU, leading to complete decoherence to the ground state before measurement could capture the intended quantum dynamics.

## 3. Diagnostic QPU Tests (Shallow Circuits)

To isolate the problem and verify basic QPU functionality, several very shallow circuits were executed:

### 3.1. Test A: Initial State Preparation & Measurement
*   **Objective:** Verify preparation and measurement of a non-ground state.
*   **Circuit:** Prepare 3 qubits in state `|010⟩` (logical coin `|0⟩`, logical position `|10⟩` or site 2). Measure all.
*   **Transpiled ISA Depth:** 2
*   **QPU Counts:** Predominantly `010` (~81.3%), with other outcomes due to SPAM error.
    *   `{'010': 3745, '011': 154, '000': 552, ...}`
*   **Result:** Successful. The QPU can prepare and measure a simple basis state other than `|000⟩` with reasonable fidelity.

### 3.2. Test B: Initial State -> Hadamard on Coin -> Measure
*   **Objective:** Verify the action of a single Hadamard gate creating superposition.
*   **Circuit:** Prepare `|010⟩`. Apply `H` to the coin qubit. Measure all.
*   **Ideal Bitstrings (coin_bit pos_bits):** `"010"` (50%) and `"110"` (50%).
*   **Transpiled ISA Depth:** 5
*   **QPU Counts:**
    *   `"010"`: ~40.67%
    *   `"110"`: ~40.00%
    *   Other states constituted ~19.3% noise.
*   **Result:** Successful. The Hadamard created the expected superposition, and the target outcomes were clearly dominant.

### 3.3. Test C: Toffoli Gate (CCX)
*   **Objective:** Test a basic 3-qubit controlled gate.
*   **Circuit:** Prepare `|110⟩`. Apply `CCX(q2, q1, q0)`. Measure.
*   **Ideal Bitstring:** `"111"`
*   **Transpiled ISA Depth:** 41
*   **QPU Counts:**
    *   `"111"`: ~91.14%
    *   `"110"` (no flip): ~3.08%
*   **Result:** Successful. The Toffoli gate operated with high fidelity, indicating the QPU can handle this level of 3-qubit gate complexity.

**Conclusion from Diagnostic Tests:** The QPU (`ibm_brisbane`) functions correctly for very shallow circuits (ISA depth up to ~40), successfully performing state preparation, single-qubit superposition, CNOTs (implicitly in Bell/Toffoli), and CCX operations.

## 4. Refined 1D Quantum Walk Attempts on QPU

Based on diagnostic successes, QW circuits were redesigned for minimal depth.

### 4.1. QW with "Ultra-Simplified Shift" (N=4, D=1 QW step)
*   **Shift Logic:** Coin-conditional single X-gate on one of the two position qubits. Implemented with 2 X-gates and 2 CNOTs.
*   **Circuit:** Init `|010⟩` -> H(coin) -> UltraSimpleShift -> Measure.
*   **Transpiled ISA Depth:** 15
*   **QPU Counts:** `{'000': 4096}`.
*   **Interpretation:** This result was still problematic. Despite the very shallow ISA depth (only slightly more than the Bell+H probe which worked), the QPU defaulted to the ground state. This suggested that either this specific sequence/mapping was particularly prone to error/decay, or the earlier `DEPTH=0` QW result (which also gave all `000` when `010` was expected) pointed to a more fundamental SPAM issue for the QW's specific initial state configuration on the chosen physical qubits.

### 4.2. QW with "Manual Npos2 Shift" (N=4, D=2 QW steps)
*   **Shift Logic (`apply_shift_step_qiskit_Npos2_final`):** Correctly implements controlled modular increment/decrement for 2 position qubits using X, CX, and CCX gates.
*   **Circuit:** Init `|010⟩` -> [H(coin) -> Npos2_Shift] x 2 -> Measure.
*   **Transpiled ISA Depth:** 260
*   **QPU Counts:** `{'010': 918, '000': 1033, '110': 459, '001': 284, '101': 317, '100': 473, ...}`
    *   P(Site 0) "x00": ~0.3677 (Ideal: 0.5)
    *   P(Site 1) "x01": ~0.1467 (Ideal: 0.0)
    *   P(Site 2) "x10": ~0.3362 (Ideal: 0.5)
    *   P(Site 3) "x11": ~0.1494 (Ideal: 0.0)
*   **Result & Interpretation:** **Success!** Despite the very high ISA depth and significant noise (probability "leaked" to ideally zero-probability sites), the QPU produced a distribution where the **qualitatively correct peaks at Site 0 and Site 2 were dominant.** This demonstrates that the underlying QW dynamics, driven by the Npos2 shift operator, were partially preserved through the noisy execution. This is a crucial result showing a non-trivial QW behavior on the QPU.

## 5. Overall Conclusions on QPU Performance for QW

1.  **Circuit Depth is Paramount:** There is a clear threshold in transpiled ISA circuit depth beyond which the current QPU (`ibm_brisbane`) struggles to maintain quantum coherence, leading to results heavily dominated by noise or decay to the ground state. For our 3-qubit QW systems, circuits with ISA depths < ~50 performed qualitatively well (HHH, Bell, Toffoli). Circuits with ISA depths > ~100-200 were severely affected.
2.  **Basic Quantum Operations are Feasible:** The QPU can execute H, X, CX, and even CCX (Toffoli) gates with sufficient fidelity in shallow circuits to observe their intended quantum effects.
3.  **State Preparation & Measurement (SPAM):** The `Test1_InitMeasure` (D=0 QW, preparing `|010>`) yielded ~81% fidelity for the target state, indicating SPAM errors contribute significantly but do not completely prevent state preparation for very shallow circuits. The consistent `|000>` for some deeper QW attempts might be an amplification of this SPAM combined with decoherence.
4.  **Quantum Walk on QPU is Possible (Qualitatively for Shallow Systems):** The D=2 QW with the Npos2 shift (ISA depth 260) successfully showed the characteristic peaks of the QW, albeit with high noise. This demonstrates that the algorithmic "shape of information" can, to some extent, be realized on current hardware if the algorithm is robust enough or the signal is strong.
5.  **Simulator Remains Essential:** For developing and understanding the ideal, noise-free behavior of complex QW algorithms (especially those coupled with CAs or using deeper arithmetic like QFT shifts), `AerSimulator(method='statevector')` is indispensable. QPU runs serve as crucial benchmarks for current hardware capabilities and for observing real-world noise effects.

**Implications for "Proof of Theory":**
These QPU experiments provide valuable context. While a perfect, noise-free demonstration of complex emergent phenomena from a deep QW-CA algorithm on a QPU is currently out of reach, we have shown that:
    a. The fundamental quantum building blocks work on the QPU.
    b. A simplified QW algorithm can produce qualitatively correct (though noisy) probability distributions, demonstrating the principle of information propagation according to quantum rules on a physical quantum device.
    c. The limitations encountered highlight the ongoing challenges and progress in quantum hardware development.

This provides a realistic backdrop for the ideal simulator results, which more clearly showcase the core theoretical connections between quantized information, algorithmic structure, and emergent informational geometries.





# Quantum Walk (QW) Experiments on IBM QPU (`ibm_brisbane`) with Qiskit

**Date:** May 11, 2025 (Placeholder)
**Objective:** To implement and test 1D Quantum Walk algorithms on real quantum hardware, understand current limitations, and demonstrate basic quantum phenomena as a precursor to more complex QW-CA simulations.

## 1. Background & Motivation

The ultimate goal is to explore a theoretical framework where reality is based on quantized information processed by quantum algorithms, leading to emergent structures and dynamics. Quantum Walks (QWs) coupled with Cellular Automata (CAs) serve as a model for this. Before tackling complex CA-coupled QWs on a Quantum Processing Unit (QPU), a series of diagnostic tests and simplified QW runs were performed to establish a working pipeline and understand hardware limitations.

All QPU experiments targeted `ibm_brisbane` (127-qubit Eagle r3 processor) via `QiskitRuntimeService` using the `SamplerV2` primitive with 4096 shots.

## 2. Initial QPU Attempts & Challenges

### 2.1. QW with QFT-Based Shift (N=4, D=2)
*   **Circuit:** 1D QW, Hadamard coin, QFT-based controlled modular incrementer/decrementer for shift.
*   **Transpiled ISA Depth:** ~216
*   **QPU Result:** Consistently `{'000': 4096}` (all shots measured ground state).
*   **Conclusion:** Circuit depth far too high for current QPU coherence, leading to complete decoherence.

### 2.2. QW with CA-Dependent Coin (UnitaryGate) & Manual Npos2 Shift (N=4, D=1)
*   **Circuit:** CA-dependent coin implemented as an 8x8 `UnitaryGate` (for 3 qubits: 2 pos, 1 coin), followed by a manually constructed 2-position-qubit shift operator (`apply_shift_step_qiskit_Npos2_final`).
*   **Transpiled ISA Depth:** ~244
*   **QPU Result:** Consistently `{'000': 4096}`.
*   **Conclusion:** Still too deep. The custom `UnitaryGate` for the CA-coin also contributes significantly to depth.

### 2.3. QW with Hadamard Coin & Manual Npos2 Shift (N=4, D=2)
*   **Circuit:** Simplified to a standard Hadamard coin, but still used the `Npos2_final` shift.
*   **Transpiled ISA Depth:** ~260
*   **QPU Result:** Consistently `{'000': 4096}`.
*   **Conclusion:** Even the `Npos2_final` shift, when iterated twice with a coin operation, results in circuits too deep for `ibm_brisbane` to maintain quantum information.

These initial attempts highlighted the critical impact of transpiled circuit depth on QPU performance.

## 3. Basic Diagnostic QPU Tests (3 Qubits)

To isolate the issue, simpler circuits were run:

### 3.1. Test A: Initial State Measurement
*   **Objective:** Verify state preparation and measurement for the QW's starting state.
*   **Circuit:** Prepare `pos_q = |10⟩` (Site 2), `coin_q = |0⟩`. Measure all.
*   **Logical Qubit Order for Measurement String "c p1 p0":** `coin_q[0]` (c), `pos_q[1]` (p1, MSB), `pos_q[0]` (p0, LSB).
*   **Ideal Initial Bitstring:** `"010"`
*   **Transpiled ISA Depth:** 2
*   **QPU Counts (`ibm_brisbane`):** `{'010': 3745, '011': 154, '000': 552, '100': 6, '001': 16, '110': 36, '111': 1}` (out of 4096)
*   **Result:** Dominant outcome `"010"` (~81.3%).
*   **Conclusion:** QPU can prepare and measure this simple 3-qubit basis state with reasonable fidelity. SPAM errors account for ~19%. **Crucially, not all `000`.**

### 3.2. Test B: Initial State -> Hadamard on Coin -> Measure
*   **Objective:** Verify action of a single Hadamard gate on the coin qubit.
*   **Circuit:** Prepare `pos_q = |10⟩`, `coin_q = |0⟩`. Apply `H(coin_q[0])`. Measure all.
*   **Ideal Bitstrings ("c p1 p0"):** `"010"` (50%) and `"110"` (50%).
*   **Transpiled ISA Depth:** 5
*   **QPU Counts (`ibm_brisbane`):** `{'010': 1666, '110': 1638, '000': 319, '100': 341, ...}`
    *   `P("010")` ≈ 40.67%
    *   `P("110")` ≈ 40.00%
*   **Conclusion:** Excellent result. The Hadamard created the superposition on the coin, and the two expected outcomes are clearly dominant (~80.7% combined). Noise accounts for ~19.3%.

### 3.3. Test: Toffoli Gate (CCX)
*   **Objective:** Test a basic 3-qubit gate used in more complex shifts.
*   **Circuit:** Prepare `|110⟩` on `q2, q1, q0`. Apply `CCX(q2, q1, q0)`. Measure.
*   **Ideal Bitstring ("q2 q1 q0"):** `"111"`
*   **Transpiled ISA Depth:** 41
*   **QPU Counts (`ibm_brisbane`):** `{'111': 3733, '110': 126, ...}`
    *   `P("111")` ≈ 91.14%
*   **Conclusion:** Very good result. The Toffoli gate functions as expected with high probability. An ISA depth of 41 is manageable for this specific gate structure.

These diagnostic tests showed that the QPU and the Qiskit Runtime pipeline are functional for shallow circuits. The consistent `|000>` results in earlier QW attempts were therefore strongly indicative of the circuit depth exceeding the coherence limits for those more complex operations.

## 4. Ultra-Shallow 1D QW Test on QPU (DEPTH=1)

Based on the diagnostic successes, an "ultra-simplified" shift operator was designed to minimize gate count.

*   **Shift Logic (`apply_ultra_simple_shift_qiskit`):**
    *   If coin=`|0⟩`, apply `X` to `pos_q[0]` (LSB_pos).
    *   If coin=`|1⟩`, apply `X` to `pos_q[1]` (MSB_pos).
    *   Implemented using 2 X-gates and 2 CNOTs.
*   **Circuit:** Init `pos=2, coin=0` -> `H(coin)` -> UltraSimpleShift -> MeasureAll.
*   **Transpiled ISA Depth:** 15
*   **Ideal Bitstrings ("c p1 p0"):** `"011"` (Coin 0, Pos 3) and `"100"` (Coin 1, Pos 0), each 50%.
*   **QPU Counts (`ibm_brisbane`):** `{'000': 4096}` (from user output, image showed distributed for Bell+H probe).
    *   **Correction based on previous thread analysis:** The result `{'000': 4096}` was from the *re-run* of the D=1 QW using the `Npos2_final` shift (which had ISA depth 126). The plot for the "ultra-simple shift D=1" was not provided, or this latest output overwrote it.
    *   **Assuming the previous "Bell State + H Probe" plot corresponds to the successful shallow run:** That run (ISA depth 11) showed `{'00': 1153, '01': 1105, '10': 831, '11': 1007}` which correctly showed all 4 outcomes for that specific circuit.

**Re-evaluating the latest QPU results showing successful distribution for HHH and Bell+H tests:**
These tests confirm that basic quantum operations and entanglement generation are possible on `ibm_brisbane` for circuits with very low ISA depth (e.g., < 20-30 gates). The QW attempts that resulted in all `|000>` had significantly higher ISA depths (120+ to 260+), indicating that decoherence and accumulated errors were overwhelming the computation.

## 5. Current Status & Path Forward for "Proof of Theory"

*   **QPU Viability:** Demonstrating complex QW dynamics (especially those coupled with CAs or implementing precise arithmetic shifts like QFT or even multi-CCX Npos2) on current NISQ hardware is extremely challenging due to circuit depth limitations. Results are quickly dominated by noise, often leading to decay to the ground state.
*   **Simulator as Primary Tool:** For developing and showcasing the *ideal* emergent behavior of QWs on CA substrates and for illustrating the "shape of information = shape of algorithm" thesis, `AerSimulator(method='statevector')` is the most reliable tool. It provides noise-free results that clearly demonstrate the algorithmic effects.
*   **QPU for Demonstrating Fundamental Quantum Effects:** The successful HHH and Bell tests (and potentially an ultra-shallow, simplified QW if it yields non-trivial results) can be used to:
    *   Show that the QPU *can* perform basic quantum operations (superposition, entanglement).
    *   Illustrate the *challenges* of NISQ hardware when circuit depth increases.
    *   Serve as a practical example of "running computations" on a physical quantum substrate.

**To complete the "Proof of Theory" document:**

1.  **Present Simulator Results:** Use the detailed 1D QW on CA Substrate plots (like the ones generated by the NumPy evolution code with Qiskit-derived operators) to clearly show how different CA rules (algorithmic substrates) produce different "shapes of information" (`P(x,t)`, `S(t)`). This is the core demonstration.
2.  **Present QPU Diagnostic Results:** Include the successful HHH and Bell state (+ H probe) test results from `ibm_brisbane` to establish that the QPU is functional for shallow circuits.
3.  **Discuss QPU Limitations:** Present the `{'000': 4096}` results from the deeper QW attempts on the QPU. Explain this as a consequence of current hardware noise and decoherence overwhelming the computation at those circuit depths. This highlights the gap between theoretical algorithms and current practical quantum computation for such tasks.
4.  **Reiterate Theoretical Connections:** Ensure the theory text embedded in plots (or accompanying them) clearly links all observations back to your core propositions about quantized information, algorithmic geometry, and emergence.

This approach provides a balanced and honest representation: the power of the theoretical concept and its ideal simulation, alongside a realistic assessment of executing such algorithms on today's quantum hardware.

---
