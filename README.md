# pulser-neutral-atom-compiler

A full compilation pipeline that takes an abstract digital quantum circuit — written in standard gate notation — and compiles it into a native Pulser Sequence that can run on Pasqal's neutral-atom hardware. The compiler respects the physical constraints unique to neutral-atom devices: Rydberg blockade radius, atom register geometry, native gate sets, and pulse timing constraints.

Core Components

1. Atom Register Layout Optimizer
Takes a logical qubit connectivity graph and solves an assignment problem: place logical qubits at physical atom positions in a 2D register such that frequently-interacting qubit pairs are placed within Rydberg blockade radius of each other, minimizing required SWAP operations.
	•	Input: logical circuit + target Pasqal Device object (hardware constraints)
	•	Uses NetworkX for graph analysis of qubit interaction frequency
	•	Implements a greedy placement algorithm with local search improvement
	•	Output: optimized Register object for the Pulser Sequence

2. Rydberg-Aware Gate Decomposer
Decomposes standard universal gates (CNOT, Toffoli, arbitrary single-qubit rotations) into Pasqal's native gate set, which is based on Rydberg interactions and global/local addressing. This is fundamentally different from superconducting decomposition and requires understanding of the Rydberg blockade mechanism.
	•	Supports decomposition of: CNOT, CZ, SWAP, single-qubit Rx/Ry/Rz, Toffoli
	•	Implements CZ-to-native decomposition using Rydberg blockade
	•	Validates each decomposed gate against hardware fidelity thresholds
	•	Reports estimated fidelity loss per decomposition step

3. Pulse Schedule Builder
Generates the Pulser Sequence object from the decomposed gate list, assigning pulses to channels, respecting timing constraints, and optimizing the schedule for minimal idle time.
	•	Channel assignment: maps gates to local and global addressing channels
	•	Timing optimizer: minimizes idle qubit time to reduce decoherence exposure
	•	Validates against Device constraints: max sequence duration, channel bandwidth
	•	Outputs a valid Pulser Sequence object ready for emulator or hardware execution

4. Benchmarking Harness
Metrics Measured
	•	Circuit depth before and after compilation (should decrease with good register placement)
	•	Number of SWAP gates inserted (fewer = better register layout)
	•	Rydberg blockade violations detected (target: zero after compilation)
	•	Estimated circuit fidelity using published Pasqal gate error rates
	•	Compilation wall-clock time vs circuit size

The Creative Twist
The README includes an interactive visualization: for each benchmark circuit, it shows the atom array as a 2D scatter plot with Rydberg blockade radii drawn as shaded circles around each atom. Before and after register optimization are shown side by side. This visual makes the physical constraint of neutral-atom compilation immediately intuitive — and it's exactly the kind of debugging tool any member of the Pulser team would want to exist but doesn't currently have as a standalone tool.

What Skills This Demonstrates to Pasqal
Direct Match to Their Open Role Requirements
	•	Pulser SDK fluency — the tool is built on top of their own library
	•	Neutral-atom compilation concepts: Rydberg blockade, register geometry, native gate sets
	•	Hardware-aware scheduling and optimization
	•	Production-quality Python with unit tests, CI/CD, and documentation
	•	Open-source community contribution mindset (README explicitly references Pulser community)

Bonus Action
After building this, open a GitHub Issue or Discussion on the official Pulser repository describing a specific limitation you encountered (e.g., a gap in the register optimization API). Frame it as a constructive suggestion, not a complaint. This signals active community engagement — something Pasqal's job description explicitly asks for.


