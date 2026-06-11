# Week 5
# PHASE 1 — Prepare Gate-Level Netlist Integration
Gate-Level Netlist Selection and Verification

For Gate-Level Simulation (GLS), the selected netlist is the post-route implementation netlist generated after the complete physical design flow.

Netlist Path:

/home/vsdsquadron/workspace/vsd-scl180-orfs/orfs/flow/results/sky130hd/user_project_wrapper/base/6_final.v

The file 6_final.v was chosen because it represents the final implemented design after placement, Clock Tree Synthesis (CTS), routing, and physical optimizations. Unlike the original RTL description, this netlist contains the actual standard-cell instances and interconnections that will be realized in silicon, making it the most accurate representation of the fabricated hardware for GLS.

To ensure successful simulation, all required standard-cell libraries and primitive models must be included during compilation. The physical netlist references SKY130 standard cells whose functional definitions are provided by the following library files:

/home/vsdsquadron/.volare/sky130A/libs.ref/sky130_fd_sc_hd/verilog/primitives.v
/home/vsdsquadron/.volare/sky130A/libs.ref/sky130_fd_sc_hd/verilog/sky130_fd_sc_hd.v

These files contain the Verilog models for logic gates, buffers, clock cells, tie cells, antenna diodes, and other standard cells inserted during implementation. Without these dependencies, the simulator would report unresolved module references.

The netlist was verified to be simulation-ready by updating the GLS compilation flow to include the required SKY130 library models and replacing the RTL wrapper with the generated 6_final.v netlist. A mixed-mode simulation approach was adopted, where the user_project_wrapper is simulated at gate level while the remaining Caravel infrastructure continues to use RTL models. This approach ensures compatibility, avoids missing-module errors, and provides an accurate verification environment without requiring a full gate-level simulation of the entire SoC.

As a result, the selected netlist, together with the SKY130 standard-cell libraries and the modified GLS Makefile configuration, provides a complete and simulation-ready environment for gate-level verification.
