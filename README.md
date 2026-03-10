# pulser-neutral-atom-compiler

A full compilation pipeline that takes an abstract digital quantum circuit — written in standard gate notation — and compiles it into a native Pulser Sequence that can run on Pasqal's neutral-atom hardware. The compiler respects the physical constraints unique to neutral-atom devices: Rydberg blockade radius, atom register geometry, native gate sets, and pulse timing constraints.
