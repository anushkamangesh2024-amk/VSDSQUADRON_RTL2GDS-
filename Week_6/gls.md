# Gate-Level Simulation (GLS) Integration and Validation
Netlist Integration


<img width="1050" height="588" alt="image" src="https://github.com/user-attachments/assets/7b6eb780-1b0a-4295-ba53-494aae416c5d" />



The generated gate-level netlist (6_final.v) was integrated into the existing verification flow without creating a new simulation framework. The RTL design reference was replaced with the synthesized gate-level netlist while preserving the original testbench and execution methodology.

Makefile / Simulation Setup Changes

The simulation setup was updated to:

Replace the RTL source with 6_final.v.
Include the SKY130 standard-cell libraries:
sky130_fd_sc_hd.v
primitives.v
Retain the existing simulation flags (FUNCTIONAL, GL, SIM, UNIT_DELAY).
Enable power-pin support for system-level simulation using USE_POWER_PINS.

Modified GLS Compilation Structure

iverilog \
  -DFUNCTIONAL \
  -DGL \
  -DUNIT_DELAY=#1 \
  hkspi_tb_simple.v \
  6_final.v \
  sky130_fd_sc_hd.v \
  primitives.v
GLS Execution

Two independent GLS environments were used:

Validation Level	Testbench	Netlist Used
Block-Level	hkspi_tb_simple.v	6_final.v
System-Level	hkspi_tb.v (Caravel)	6_final.v
Functional Verification Results
Check	Result
Netlist compilation	PASS
Standard-cell resolution	PASS
No missing module errors	PASS
Simulation execution	PASS
VCD waveform generation	PASS
Write transaction verification	PASS
Read transaction verification	PASS
Reset behavior verification	PASS
RTL vs GLS waveform correlation	PASS
Outcome

The gate-level netlist was successfully integrated into the verification flow and simulated using both block-level and full-Caravel environments. All tests executed without compilation or elaboration errors, and the observed behavior matched the expected RTL functionality. The generated GLS waveforms confirmed that the implemented design remained functionally equivalent to the original RTL specification.

