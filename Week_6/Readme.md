# PHASE 1 — Block Selection and Analysis
## RTL Block Analysis – housekeeping_spi

### Overview

The housekeeping_spi block was selected for RTL analysis from the Caravel SoC RTL repository. This module implements the Serial Peripheral Interface (SPI) used for chip configuration, housekeeping register access, and flash pass-through operations.

The block was chosen because it is self-contained, has a clearly defined interface, and represents a practical example of a control-oriented digital design.



### Top Module

| Property    | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| Module Name | `housekeeping_spi`                                                     |
| Source File | `housekeeping_spi.v`                                                   |
| Function    | SPI controller for housekeeping register access and flash pass-through |
| Design Type | Sequential FSM-based controller                                        |

### Module Interface

### Inputs

* `reset`
* `SCK`
* `SDI`
* `CSB`
* `idata[7:0]`

### Outputs

* `SDO`
* `sdoenb`
* `odata[7:0]`
* `oaddr[7:0]`
* `rdstb`
* `wrstb`
* `pass_thru_mgmt`
* `pass_thru_mgmt_delay`
* `pass_thru_user`
* `pass_thru_user_delay`
* `pass_thru_mgmt_reset`
* `pass_thru_user_reset`

## RTL Hierarchy

The design contains a single top-level module with no instantiated submodules. Internally, the functionality is organized into several logical blocks:


housekeeping_spi<br/>
│<br/>
├── SPI Command Decoder<br/>
├── Address Processing Logic<br/>
├── Data Processing Logic<br/>
├── Finite State Machine (FSM)<br/>
├── Pass-Through Control Logic<br/>
└── Shift Registers (TX/RX)<br/>


### Internal Functionality

* Receives SPI commands, addresses, and data.
* Controls read and write transactions.
* Supports streaming and fixed-length transfers.
* Provides pass-through access to management and user flash memories.
* Uses an FSM to manage transaction flow.


## RTL Dependencies

The following RTL files are required for successful synthesis and integration:
''''
| File                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `housekeeping_spi.v` | Top-level SPI controller                   |
| `defines.v`          | Global macros and configuration parameters |
| `debug_regs.v`       | Housekeeping subsystem support module      |
''''
### RTL Directory


~/vsd-scl180-orfs/orfs/flow/designs/sky130hd/housekeeping_spi/rtl/


## Key Observations

* The design is based on a finite state machine controlling SPI transactions.
* Supports both register access and flash pass-through modes.
* Uses separate positive and negative clock-edge logic for SPI timing compliance.
* Includes automatic address incrementing during streaming transfers.
* Implements clean transaction termination through chip-select based reset logic.



## Conclusion

The housekeeping_spi block is a compact SPI controller that manages housekeeping register access and flash pass-through functionality within the Caravel platform. Its well-defined interface, FSM-based architecture, and limited dependencies make it suitable for RTL analysis and subsequent RTL-to-GDSII implementation.

# PHASE 2 — RTL-to-GDS Implementation

Here we set up ORFS flow for the selected block, organize RTL files correctly, apply clock constraint (assume 100 MHz unless block requires otherwise) and run complete RTL-to-GDS flow
~/vsd-scl180-orfs/orfs/flow$ make clean_all
~/vsd-scl180-orfs/orfs/flow$ make
 <br/>Synthesis report : using command cat 1_synth.log <br/>
<img width="1018" height="710" alt="image" src="https://github.com/user-attachments/assets/def7a681-6caa-4f6e-bb39-5588db822979" />

 <br/>Floorplan output <br/>
 <img width="588" height="630" alt="image" src="https://github.com/user-attachments/assets/d7a099da-224b-423a-958f-df1422487ba7" />
<br/>Core area: 4086.419 um^2<br/>

 <br/>Placement result <br/>
 <img width="577" height="630" alt="image" src="https://github.com/user-attachments/assets/8305dd0f-bc47-4236-9ed6-a4f34d301955" />
<br/>Design area 2796 um^2 68% utilization.<br/>
 <br/>CTS log <br/>
 <img width="1025" height="727" alt="image" src="https://github.com/user-attachments/assets/bca5f795-3efb-466e-86c4-0616f72c19fc" />

 <br/>Routing completion <br/>
 <img width="1237" height="672" alt="image" src="https://github.com/user-attachments/assets/0d5356f8-8e2c-4de8-87d6-85293c8e6513" /> <br/>
 [INFO DRT-0198] Complete detail routing.
[INFO ANT-0002] Found 0 net violations.
[INFO ANT-0001] Found 0 pin violations.<br/>
<img width="1232" height="672" alt="image" src="https://github.com/user-attachments/assets/920fd800-3591-465b-aab5-d5ceda6820d5" />

 <br/>Final GDS generated <br/>
<img width="576" height="642" alt="image" src="https://github.com/user-attachments/assets/d245dd99-de69-4db8-bb5a-b12e95c2a4ea" />

# PHASE 3 — Generate Implementation Outputs
<br/>Synthesized netlist<br/>
<img width="630" height="461" alt="image" src="https://github.com/user-attachments/assets/e300855f-313c-4c6a-a24b-bb74b6e52a0e" />

<br/>Final netlist<br/>
<img width="592" height="443" alt="image" src="https://github.com/user-attachments/assets/53ffbb5e-ba62-42ef-8edd-d09e344f379b" />

<br/>DEF / database<br/>
<img width="590" height="443" alt="image" src="https://github.com/user-attachments/assets/2740e388-3d57-46ee-829d-45c98c9d8022" />

<br/>Filled database, GDSII<br/>
<img width="365" height="402" alt="image" src="https://github.com/user-attachments/assets/75f8e10a-c599-49be-8cab-e2c08dcb7d12" />

<br/>Timing report<br/>
TNS (Total Negative Slack)	0.00 ps 
WNS (Worst Negative Slack)	0.00 ps 
Worst slack (max)	+4.70 ns 
Clock period minimum	5.30 ns
f_max achievable	188.80 MHz
Critical path delay	3.30 ns
Slack / critical path ratio	142.68%<br/>
<img width="853" height="443" alt="image" src="https://github.com/user-attachments/assets/41f51eea-fbe7-48f1-930d-1c275ba7ffc6" />


# PHASE 4 — Gate-Level Simulation (GLS)
Gate-Level Simulation (GLS) Integration and Validation
Netlist Integration


<img width="1050" height="588" alt="image" src="https://github.com/user-attachments/assets/8ee3601a-6cb4-484f-817b-ca3eec41e6ce" />



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

# PHASE 5 — Waveform Validation
We generate .vcd during GLS, open waveform in GTKWave and observe gate-level signal activity <br/>


<img width="607" height="286" alt="image" src="https://github.com/user-attachments/assets/959da51e-8868-46a8-9f97-9e6410f2c683" /><br/>
Block-level GLS waveform — hkspi_gls.vcd — 0 to 7055 ns.
Signals: CSB, SCK, SDO, SDI, reset, wrstb, sdoenb, read_data[7:0], rdstb, idata[7:0]<br/>
<img width="596" height="240" alt="image" src="https://github.com/user-attachments/assets/bddf17b4-5221-41a5-8364-94d91545ee8e" /><br/>
<img width="602" height="316" alt="image" src="https://github.com/user-attachments/assets/e9c93a19-834b-4a65-96dd-772f0d8fb79d" /><br/>

System-level GL waveform — GL-hkspi.vcd — 0 to 61410 ns.
Signals: CSB, clock, RSTB, SCK, SDI, SDO, checkbits[15:0], uart_tx, uart_rx

Signal analysis:
CSB (Chip Select Bar)
Purpose: Defines the start and end of SPI transactions and is active LOW.
RTL Observation: Regular CSB pulses are generated as firmware performs register accesses through the SPI interface.
GLS Observation: The CSB pulse pattern matches RTL exactly, confirming correct transaction handling by the gate-level netlist.
SCK (SPI Clock)
Purpose: Provides the clock for SPI communication, with data sampled on the positive edge and driven on the negative edge.
RTL Observation: Each transaction contains the expected clock bursts corresponding to command, address, and data transfers.
GLS Observation: The clock waveform matches RTL, with each transaction showing the same 24-clock-cycle SPI frame structure.
SDI (Serial Data Input)
Purpose: Carries SPI command, address, and data bits into the design.
RTL Observation: Command and data bytes are transmitted serially in the expected MSB-first format.
GLS Observation: The SDI bit stream is identical to RTL, demonstrating correct data capture and protocol implementation in the gate-level netlist.

# PHASE 6 — RTL vs GLS Validation
Functional Verification: RTL vs Gate-Level Simulation (GLS)
Objective

The purpose of this verification activity was to ensure that the gate-level netlist generated after synthesis, placement, clock-tree synthesis, and routing remained functionally equivalent to the original RTL design. The final netlist (6_final.v) was integrated into the simulation environment and executed using both block-level and system-level testbenches.

The verification focused on confirming that:

Functional behavior was preserved after physical implementation.
RTL and GLS produced equivalent outputs for the same inputs.
No logic errors were introduced during synthesis or optimization.
The implemented design operated correctly under realistic gate-level conditions.
Verification Method

Two independent GLS approaches were used:

1. Block-Level Verification

A dedicated testbench (hkspi_tb_simple.v) directly instantiated the housekeeping_spi gate-level netlist. This allowed focused verification of SPI transactions without involving the complete Caravel SoC environment.

The following operations were tested:

SPI Write transaction
SPI Read transaction
Reset during an active transaction
2. System-Level Verification

The official Caravel testbench (hkspi_tb.v) was used to verify the gate-level netlist within the complete SoC environment. In this setup, firmware running on the RISC-V processor generated the SPI transactions, providing a realistic end-to-end validation scenario.

Functional Comparison Results
Write Operation Verification

The write transaction sent:

Command: 0x80 (Write)
Address: 0x08
Data: 0x37

Observed GLS behavior:

Address decoded correctly as 0x08
Output data captured correctly as 0x37
wrstb asserted at the end of the transaction

The observed behavior matched the expected RTL functionality exactly.

Read Operation Verification

The read transaction sent:

Command: 0x40 (Read)
Address: 0x08

A fixed value of 0xA5 was provided through the idata input.

Observed GLS behavior:

Address decoded correctly
Read strobe (rdstb) asserted correctly
Returned value was 0xA5

The returned data matched the expected value, confirming correct operation of the read path.

Reset Verification

A reset was asserted in the middle of an active SPI transaction.

Observed GLS behavior:

State machine immediately returned to its idle state
Address output returned to 0x00
Internal transaction state was cleared

This behavior matched the intended RTL reset functionality.

RTL vs GLS Waveform Comparison

Several key signals were examined in both RTL and GLS waveforms.

CSB (Chip Select)

The active-low transaction boundaries appeared at identical locations in both simulations. Every firmware-generated SPI transaction observed in RTL was also present in GLS.

SCK (SPI Clock)

The SPI clock burst structure remained unchanged. Each transaction contained the expected sequence of clock pulses corresponding to command, address, and data transfers.

SDI (Serial Data Input)

The serial command and data stream observed in GLS matched the RTL waveform bit-for-bit, confirming correct data capture by the synthesized logic.

SDO (Serial Data Output)

Readback data was transmitted correctly and matched RTL behavior throughout the simulation.

Mismatch Investigation

During verification, the RTL and GLS results were carefully compared to identify any functional discrepancies.

Functional Mismatches

No functional mismatches were observed.

The following checks all passed successfully:

Check	Result
Write data correctness	Pass
Read data correctness	Pass
Address decoding	Pass
FSM state transitions	Pass
Reset behavior	Pass
SPI protocol compliance	Pass
System-level execution	Pass
Root Cause Analysis

Since no mismatches were detected:

No root-cause investigation was required.
No corrective actions were necessary.
No design modifications were needed.

Any minor timing differences observed between RTL and GLS were expected because GLS includes gate propagation delays and physical implementation effects, whereas RTL assumes ideal timing. These timing differences did not affect functionality.

Verification Outcome

The gate-level netlist successfully completed both block-level and full-system simulations without compilation, elaboration, or runtime errors. All functional tests passed, and the outputs produced by GLS matched the expected RTL behavior.

Final Assessment
Item	Status
GLS execution successful	PASS
Functional correctness preserved	PASS
RTL and GLS outputs match	PASS
Logic behavior preserved after implementation	PASS
Functional mismatches found	NONE
Root-cause analysis required	NO
Resolution required	NO
Conclusion

The verification confirms that the housekeeping_spi gate-level netlist (6_final.v) is functionally equivalent to the original RTL design. Both isolated block-level testing and full Caravel SoC testing produced the expected results, demonstrating that synthesis and physical implementation preserved the intended functionality of the design without introducing any logic errors.

# PHASE 7 — Debugging and Insights


Challenges Encountered During GLS Integration

The transition from RTL simulation to gate-level simulation required several modifications to the verification environment. Most of the issues were related to simulation setup rather than design functionality.

<img width="987" height="457" alt="image" src="https://github.com/user-attachments/assets/54f16ad5-f47f-416a-852f-bb1e4e36a196" />

Verification Preparation

Before starting Gate-Level Simulation (GLS), the generated netlist was carefully reviewed to ensure it was suitable for simulation. This involved checking that the correct top-level module had been synthesized, verifying that all expected input and output ports were present, and confirming that the RTL-to-GDS flow had completed successfully through synthesis, placement, and routing. Performing this review early helped avoid debugging issues caused by using an incorrect or incomplete netlist.

Resolving Simulation Dependencies

Unlike RTL code, a gate-level netlist consists of standard-cell instances from the SKY130 library. As a result, the simulator cannot interpret the netlist unless the corresponding library models are provided during compilation. To address this, the required SKY130 files (primitives.v and sky130_fd_sc_hd.v) were added to the simulation environment. Once these dependencies were included, the simulator was able to resolve every cell instance and compile the design successfully.

Extending the Existing Verification Flow

Rather than creating a completely new verification framework for GLS, the existing RTL verification flow was reused and extended. The primary modification involved replacing the RTL design file with the generated gate-level netlist while keeping the original testbench structure intact. This approach minimized changes to the verification environment and allowed a direct comparison between RTL and gate-level behavior under the same test conditions.

Functional Verification

After the simulation environment was configured, both standalone block-level simulations and full system-level simulations were executed. The resulting waveforms and simulation logs were examined to verify key functionality, including SPI command processing, address decoding, data transfers, and reset operation. The gate-level implementation produced the same functional results as the RTL design, demonstrating that the synthesis and physical implementation stages had preserved the intended behavior of the module.

Lessons Learned

One of the most important observations from this exercise was that a gate-level netlist cannot be simulated in isolation. The simulator must have access to the same standard-cell libraries used during synthesis; otherwise, compilation will fail due to unresolved cell references.

Another key takeaway is that GLS involves more than simply replacing an RTL file with a netlist. Successful execution depends on correctly configuring library paths, include directories, compilation order, and hierarchy resolution. Many simulation failures originate from environment configuration issues rather than actual design problems.

The project also demonstrated the value of reusing existing verification infrastructure. By extending the original RTL flow instead of developing a separate GLS framework, verification effort was reduced while maintaining consistency between RTL and gate-level testing.

A particularly effective strategy was the use of mixed-level simulation, where the target block was simulated using its gate-level netlist while the remainder of the SoC continued to use RTL models. This provided realistic implementation validation without requiring a complete gate-level version of the entire system.

Finally, although GLS introduces physical effects such as gate delays and implementation-specific details, the ultimate goal remains unchanged: confirming that the implemented hardware behaves exactly as intended by the RTL specification.

Conclusion

Most of the effort during GLS integration was spent configuring the simulation environment rather than correcting functional design issues. Challenges such as library inclusion, hierarchy resolution, and netlist integration were systematically resolved through debugging and verification. Once the environment was properly configured, both block-level and system-level simulations completed successfully, and the observed behavior matched RTL expectations. This exercise provided practical experience in post-implementation verification and highlighted the additional considerations required when transitioning from RTL simulation to gate-level validation.
