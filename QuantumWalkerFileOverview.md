# QuantumWalker Project: File Overview

This document provides a brief description of each file in the QuantumWalker project directory.

---

## Table of Contents

1. [QuantumWalker.ipynb](#quantumwalkeripynb)
2. [QuantumRule110.ipynb](#quantumrule110ipynb)
3. [QuantumRule110exploration.ipynb](#quantumrule110explorationipynb)
4. [QuantumRule30.ipynb](#quantumrule30ipynb)
5. [QantumRule90.ipynb](#qantumrule90ipynb)
6. [QantumWalkers.ipynb](#qantumwalkersipynb)
7. [2QW1DR90.ipynb](#2qw1dr90ipynb)
8. [2QW1DR110.ipynb](#2qw1dr110ipynb)
9. [DistributionTests.ipynb](#distributiontestsipynb)
10. [Info_Theoretic_Analysis.ipynb](#info_theoretic_analysisipynb)
11. [Info_Theoretic_Analysis_part2.ipynb](#info_theoretic_analysis_part2ipynb)
12. [QEC.ipynb](#qecipynb)
13. [QRN.ipynb](#qrnipynb)
14. [HHHtest.ipynb](#hhhtestipynb)
15. [GHZtest.ipynb](#ghztestipynb)
16. [BellState.ipynb](#bellstateipynb)
17. [1dQW_on_ibm_brisbane.ipynb](#1dqw_on_ibm_brisbaneipynb)
18. [IBMHERON.md](#ibmheronmd)
19. [LICENSE](#license)
20. [QW_on_IBM_heron.ipynb](#qw_on_ibm_heronipynb)
21. [README.md](#readmemd)
22. [Info_Theoretic_Analysis_part2.ipynb](#info_theoretic_analysis_part2ipynb-duplicate)
23. [QuantumRule110exploration.ipynb](#quantumrule110explorationipynb-duplicate)
24. [QuantumRule30.ipynb](#quantumrule30ipynb-duplicate)
25. [QuantumWalker.ipynb](#quantumwalkeripynb-duplicate)
26. [QantumRule90.ipynb](#qantumrule90ipynb-duplicate)

---

## File Descriptions

### 1. `QuantumWalker.ipynb`
Main notebook for simulating 2D quantum walks using Cirq. Implements a quantum walker on a 2D grid with Hadamard or Grover coin, calculates probability distributions, entanglement entropy, and supports coarse-graining and plotting. Also includes code for comparing different coin operators.

---

### 2. `QuantumRule110.ipynb`
Simulates a 1D quantum walk with a coin operator determined by the local state of a classical Rule 110 cellular automaton (CA). Explores two-way coupling: the quantum walk influences the CA and vice versa. Includes analysis of emergent computational power and information exchange.

---

### 3. `QuantumRule110exploration.ipynb`
Extends the QuantumRule110 notebook with more detailed experiments, including parameter sweeps (coin operators, feedback types, thresholds) and comparisons with other CA rules (Rule 30, Rule 90). Focuses on emergent behavior and entanglement in the coupled QW-CA system.

---

### 4. `QuantumRule30.ipynb`
Implements a 1D quantum walk where the coin operator is determined by a static or dynamic Rule 30 CA pattern. Analyzes the effect of the CA's complexity on the quantum walk's probability distribution and entanglement.

---

### 5. `QantumRule90.ipynb`
Simulates a 1D quantum walk with a coin operator determined by a dynamic Rule 90 CA. Compares the emergent quantum behavior with other CA rules, focusing on the impact of the CA's fractal structure.

---

### 6. `QantumWalkers.ipynb`
Simulates two independent quantum walkers on a 2D grid (not interacting directly), analyzes their probability distributions, and computes the overlap ("collision") probability. Useful for studying interference and spreading in multi-walker systems.

---

### 7. `2QW1DR90.ipynb`
Simulates two quantum walkers on a 1D grid, both coupled to a shared Rule 90 CA. The CA is dynamically updated based on the walkers' positions. Analyzes emergent interactions, entanglement, and walker "attraction" via the CA medium.

---

### 8. `2QW1DR110.ipynb`
Similar to 2QW1DR90, but uses Rule 110 as the CA. Explores how a computationally universal CA environment affects the interaction and dynamics of two quantum walkers.

---

### 9. `DistributionTests.ipynb`
A collection of small tests and demonstrations for quantum probability distributions, including basic quantum walks, QCA propagation, and noise effects. Includes both 1D and 2D examples, and tests for plotting and text-based summaries.

---

### 10. `Info_Theoretic_Analysis.ipynb`
Performs information-theoretic analysis of the QW-CA system. Calculates not only probability and entanglement, but also CA compressibility (via zlib) as a proxy for algorithmic complexity. Compares different CA rules and feedback parameters.

---

### 11. `Info_Theoretic_Analysis_part2.ipynb`
Extends the information-theoretic analysis by adding spatial mutual information calculations for the CA pattern. Sweeps over CA rules and coin operator pairs, and studies the relationship between quantum and classical information measures.

---

### 12. `QEC.ipynb`
Implements and tests the 3-qubit bit-flip quantum error correction code on IBM QPU and simulators. Measures syndrome extraction fidelity and analyzes QPU performance for shallow circuits.

---

### 13. `QRN.ipynb`
Quantum random number generator experiments using Qiskit. Runs single- and multi-qubit QRNG circuits on IBM QPU and simulators, and extracts random bitstrings for use in further simulations or as entropy sources.

---

### 14. `HHHtest.ipynb`
Tests the creation and measurement of a 3-qubit product state with Hadamard gates (|+++⟩). Used as a diagnostic to verify QPU and simulator functionality for basic quantum operations.

---

### 15. `GHZtest.ipynb`
Tests the creation and measurement of a 3-qubit GHZ (Greenberger–Horne–Zeilinger) state on IBM QPU and simulators. Used to verify entanglement generation and measurement fidelity.

---

### 16. `BellState.ipynb`
Tests the creation of a Bell state and probes it with an additional Hadamard gate. Used to verify entanglement and superposition on IBM QPU and simulators.

---

### 17. `1dQW_on_ibm_brisbane.ipynb`
Implements a minimal 1D quantum walk using Qiskit, targeting the IBM QPU (`ibm_brisbane`). Uses QFT-based shift logic and analyzes the effect of circuit depth and noise on QPU results.

---

### 18. `IBMHERON.md`
Detailed experimental log and analysis of quantum walk experiments on IBM QPU (`ibm_brisbane`). Discusses circuit depth, noise, diagnostic tests, and the feasibility of observing QW dynamics on current hardware.

---

### 19. `LICENSE`
MIT License file for the project.

---

### 20. `QW_on_IBM_heron.ipynb`
Notebook for running quantum walk experiments on IBM's Heron QPU or simulator. Likely similar in structure to 1dQW_on_ibm_brisbane.ipynb, but targeting a different backend.

---

### 21. `README.md`
(If present) Would provide a high-level overview of the project, instructions, and references.

---

### 22. `Info_Theoretic_Analysis_part2.ipynb` (duplicate)
See [#11](#info_theoretic_analysis_part2ipynb).

---

### 23. `QuantumRule110exploration.ipynb` (duplicate)
See [#3](#quantumrule110explorationipynb).

---

### 24. `QuantumRule30.ipynb` (duplicate)
See [#4](#quantumrule30ipynb).

---

### 25. `QuantumWalker.ipynb` (duplicate)
See [#1](#quantumwalkeripynb).

---

### 26. `QantumRule90.ipynb` (duplicate)
See [#5](#qantumrule90ipynb).

---

## Notes

- Some files appear to be duplicates or alternate versions. If you need to distinguish between them, check their modification dates or content.
- If you need more detailed documentation for any specific file, please specify which one.

---