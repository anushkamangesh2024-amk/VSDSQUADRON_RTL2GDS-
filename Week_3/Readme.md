# Week 3
# PHASE 1 — Standalone Block Verification
Run SPI Master <br/>
<img width="682" height="628" alt="image" src="https://github.com/user-attachments/assets/e5d47e60-d51e-4eff-a14e-4a4c1aabc126" /> <br/>
Result: PASS as the SPI controller successfully transmitted and received the expected values. The testbench confirmed correct functionality of the SPI Master block.

# PHASE 2 — Run All Standalone Tests
Standalone Test Results <br/>

<img width="502" height="485" alt="image" src="https://github.com/user-attachments/assets/3b9e280b-4102-4ca2-8ea6-134721dbc340" /> <br/>
<img width="821" height="483" alt="image" src="https://github.com/user-attachments/assets/39b39085-9aa7-47b8-94a4-887bc6c65f6c" /> <br/>
<img width="822" height="487" alt="image" src="https://github.com/user-attachments/assets/c983275f-e750-407d-a362-211296130914" /> <br/>
<img width="762" height="452" alt="image" src="https://github.com/user-attachments/assets/f4591d99-d9b5-44f3-9b14-ff5ce7ac6051" /> <br/>
<img width="737" height="636" alt="image" src="https://github.com/user-attachments/assets/66527d1c-0664-4c26-a6d5-45f8b3ef0e48" /> <br/>
<img width="653" height="521" alt="image" src="https://github.com/user-attachments/assets/c0b0071d-39fd-4119-9090-2635d51cf8d1" /> <br/>

The following modules successfully passed verification:  
GPIO Management Memory SPI Master  
These blocks executed firmware instructions correctly and produced the expected results.  
The following tests failed due to timeout during simulation:  
Timer IRQ Debug  
In these cases, the expected output condition was not reached before the simulation timeout.  

# PHASE 3 — Caravel Integrated Tests
1. USER_PASS_THR <br/>
<img width="1083" height="580" alt="image" src="https://github.com/user-attachments/assets/547f9cd9-e956-4a22-ae07-74f8f952e4bc" /> <br/>
2. UART <br/>
<img width="935" height="617" alt="image" src="https://github.com/user-attachments/assets/48a92464-a204-4d12-8528-69a761821451" /> <br/>
3. SYSCTRL <br/>
<img width="671" height="617" alt="image" src="https://github.com/user-attachments/assets/ab1416a9-7fa5-49dd-b330-7470d539552d" /> <br/>
4. SRAM_EXEC <br/>
<img width="683" height="611" alt="image" src="https://github.com/user-attachments/assets/0cf85b96-9347-4f08-9f35-b6fb959b8932" /> <br/>
5. SPI_MASTER <br/>
<img width="577" height="652" alt="image" src="https://github.com/user-attachments/assets/61d71d4e-4c57-47ec-95c2-b1e7890c30c1" /> <br/>
6. PULLUPDOWN <br/>
<img width="591" height="632" alt="image" src="https://github.com/user-attachments/assets/2bcae007-51c8-4e48-8c68-c13483c9de46" /> <br/>
7. PLL <br/>
<img width="637" height="622" alt="image" src="https://github.com/user-attachments/assets/72c00b9b-1a6a-45d5-b275-d45cc07f879c" /><br/>
8. PASS_THRU_FIX<br/>
<img width="1092" height="647" alt="image" src="https://github.com/user-attachments/assets/aee792d0-a7b1-4b46-b2bc-572c0da62a73" /><br/>
9. MEM<br/>
<img width="542" height="647" alt="image" src="https://github.com/user-attachments/assets/2cdc3c95-b178-4158-92e4-6842e105776b" /><br/>
10. HKSPI_POWER<br/>
<img width="1062" height="652" alt="image" src="https://github.com/user-attachments/assets/de55682e-3e7e-4b74-a318-68b92ee0057d" /><br/>

11. GPIO_MGMT<br/>
<img width="636" height="648" alt="image" src="https://github.com/user-attachments/assets/87725446-5512-4e0f-bee0-1b6080066c6d" /><br/>

12. HKSPI<br/>
<img width="857" height="647" alt="image" src="https://github.com/user-attachments/assets/0b965e00-c958-4be3-823c-b9ac206b1be4" /><br/>

Results: <br/>
<img width="410" height="557" alt="image" src="https://github.com/user-attachments/assets/3dfd11f3-6dc5-421f-a7c3-deb70a4732f3" /> <br/>
Most tests successfully passed in RTL simulation.
Two tests failed:

PLL
The PLL is an analog/mixed-signal block.
In RTL simulation, the analog behavior of the PLL is not fully modeled, which causes the verification test to fail.

SYSCTRL
The SYSCTRL test depends on clock and timing behavior which may differ in simulation environments.
This can cause timeout conditions during RTL verification.

# Phase 4
### What Happens When `make` Is Executed

The execution of a Makefile is driven by a **dependency graph**, which defines the relationships between targets and their required files. When the `make` command is executed, it begins with the specified target and recursively examines all dependent files and intermediate targets. For each dependency, `make` compares file timestamps to determine whether recompilation is necessary. This incremental build approach ensures that only modified files and their dependents are rebuilt, significantly reducing compilation time and avoiding unnecessary recompilation of the entire project.

### How the Makefile Invokes the Simulator

The Makefile automates the complete simulation flow by invoking the required compilation and simulation tools. First, it calls **Icarus Verilog (`iverilog`)** with the appropriate command-line options to compile the Verilog source files, testbench files, and supporting hardware modules into a simulation executable. Once compilation is successful, the Makefile invokes the **Verilog Virtual Processor (`vvp`)** to execute the generated simulation model. This process allows the hardware design and firmware interactions to be verified automatically.

### Total Compiled Files

The compilation process includes both firmware and hardware components.

* **Firmware Files:** The firmware is written in C and is first compiled into an executable **ELF** file. This ELF file is then converted into a **HEX** file, which is loaded into the simulated processor memory for execution during simulation.
* **Hardware Files:** The hardware portion consists of Verilog source files (`.v`) including the testbench and all modules contained within the `includes.rtl.caravel` directory. These files collectively implement the complete system architecture, including the processor core, system bus, peripheral modules, memory interfaces, and standard-cell-based support logic.

Together, these files form the complete hardware-software co-simulation environment.

### How the Testbench Interacts with the Design

The method of interaction between the testbench and the design depends on the simulation environment being used.

**Standalone Testing:**
In standalone simulations, the testbench wraps only the internal management SoC rather than the complete chip. Since the internal signals of the management core are directly accessible, the testbench can monitor and control internal wires, registers, and interfaces. This provides detailed visibility into the system's internal operation and simplifies functional verification.

**Caravel Testing:**
In Caravel simulations, the testbench encapsulates the entire chip, including the management SoC, user project area, GPIO pads, and surrounding infrastructure. As a result, the testbench can no longer access internal signals directly. Instead, it interacts with the design by supplying power, clock, and reset signals, while observing behavior exclusively through the external GPIO pads. This approach more closely represents real silicon operation and validates the complete chip integration.

### How PASS/FAIL Is Determined

The PASS/FAIL mechanism is implemented through coordination between the firmware running on the processor and the Verilog testbench.

During execution, the firmware writes predefined status or result values that indicate the progress of a test. The Verilog testbench continuously monitors these outputs:

* In **standalone testing**, the testbench observes internal signals and status wires directly.
* In **Caravel testing**, the same information is communicated through external GPIO pins, which are monitored by the testbench.

The testbench uses Verilog `wait` statements to pause execution until the firmware produces a specific synchronization value. Once this value is detected, the testbench proceeds to validate the output generated by the peripheral or subsystem under test. The observed value is compared against the expected result using conditional (`if`) statements.

If the observed value does not match the expected value, the testbench reports a **FAIL** condition and terminates the simulation. If all verification checks pass successfully, the testbench reports **PASS**, indicating that the hardware and firmware have behaved as expected.






