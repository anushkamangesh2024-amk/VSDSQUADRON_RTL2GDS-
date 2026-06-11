Extension of Existing Verification Flow for Gate-Level Simulation

The existing RTL verification flow was extended to support Gate-Level Simulation (GLS) without creating a separate verification environment. The same testbench, compilation flow, and simulation infrastructure were retained, while only the design-under-test source was replaced with the generated gate-level netlist and the required standard-cell libraries were added.

Modifications on Makefile 
iverilog -Ttyp -DFUNCTIONAL -DSIM -DUSE_POWER_PINS -DUNIT_DELAY=#1 \
  -y $(CARAVEL_PATH)/rtl \
  -I $(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/verilog \
  $(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/verilog/primitives.v \
  $(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/verilog/sky130_fd_sc_hd.v \
  ~/vsd-scl180-orfs/orfs/flow/results/sky130hd/user_project_wrapper/base/6_final.v \
  testbench.v -o sim.vvp


Explanation of Changes

The primary modification was the replacement of the RTL design file with the synthesized and physically implemented gate-level netlist (6_final.v). This allows the simulator to verify the actual implementation produced by the RTL-to-GDSII flow rather than the original behavioral RTL description.

Since the gate-level netlist contains SKY130 standard-cell instances instead of behavioral logic, the required standard-cell model files (primitives.v and sky130_fd_sc_hd.v) were added to the compilation command. These files provide functional definitions for gates, buffers, clock cells, tie cells, and other standard cells present in the netlist.

An additional include path (-I) was introduced to ensure that the simulator can locate all required library files during compilation. This prevents unresolved references and missing-module errors.

No modifications were made to the existing testbench or simulation settings. The same verification environment used for RTL simulation was reused for GLS, ensuring consistency between RTL and gate-level verification.

Verification Checks
Requirement	Status
RTL replaced with gate-level netlist	 Completed
Standard-cell dependencies included	 Completed
Netlist compiles successfully	 PASS
No missing module/library errors	 PASS
Existing testbench reused	 PASS
Existing verification flow preserved	 PASS

The resulting flow successfully extends the original RTL verification environment to support gate-level simulation while maintaining the same testbench and execution methodology. This ensures that the implemented design can be verified without creating a separate verification flow.
