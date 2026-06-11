# Week 4
# PHASE 1 — Analyze the Top-Level Wrapper
# Dependency Tree of the Wrapper

The `user_project_wrapper` module forms the highest level of the user design hierarchy. It acts as the integration point where all functional blocks are connected to the Caravel infrastructure. The wrapper itself does not implement major functionality; instead, it coordinates and instantiates lower-level modules that perform specific tasks.

The dependency relationship is shown below:

```text
user_project_wrapper
│
├── debug_regs
│
├── user_project_gpio_example   (enabled during GPIO testing)
│
└── user_project_la_example     (enabled during Logic Analyzer testing)
```

The wrapper depends on these submodules to provide debugging, GPIO verification, and logic-analyzer-based testing capabilities. Any modification to these lower-level modules directly affects the behavior of the wrapper.

# List of All RTL Files Used in the Design

The design is composed of several RTL source files, each serving a specific purpose within the hierarchy.

| RTL File                      | Purpose                                                                                       |
| ----------------------------- | --------------------------------------------------------------------------------------------- |
| `user_project_wrapper.v`      | Top-level module connecting the user project to the Caravel environment                       |
| `debug_regs.v`                | Implements Wishbone-accessible debug and control registers                                    |
| `defines.v`                   | Contains parameter definitions, macros, and configuration settings used throughout the design |
| `user_project_gpio_example.v` | Demonstrates GPIO functionality and is included when GPIO testing is enabled                  |
| `user_project_la_example.v`   | Demonstrates Logic Analyzer functionality and is included when LA testing is enabled          |

Among these files, `user_project_wrapper.v` serves as the primary integration module, while the remaining files provide supporting functionality.

# Explanation of the Module Hierarchy

The module hierarchy follows a top-down design approach. At the highest level resides the `user_project_wrapper`, which serves as the interface between the user logic and the Caravel SoC framework. This wrapper receives system resources such as clock, reset, Wishbone bus connections, GPIO signals, and interrupt lines.

Below the wrapper are the functional submodules:

* The `debug_regs` module provides register-based communication and debugging support through the Wishbone bus.
* The `user_project_gpio_example` module implements GPIO-related test functionality and is instantiated only when GPIO verification is required.
* The `user_project_la_example` module demonstrates the use of Logic Analyzer signals for debugging and internal signal observation.

The hierarchy is intentionally shallow, with the wrapper directly instantiating the required modules. This structure simplifies integration, verification, and maintenance while allowing optional modules to be enabled or disabled through compile-time configuration settings.

From a compilation perspective, lower-level modules must be compiled first so that the top-level wrapper can successfully instantiate them during elaboration. Consequently, the hierarchy also dictates the compilation order of the RTL files.

# PHASE 2 — Prepare the ORFS Design Environment

<img width="1068" height="672" alt="image" src="https://github.com/user-attachments/assets/121e3243-145f-41a4-8bbc-4b51c036cd98" /> <br/>
The directory structure is organized as follows: <br/>

user_project_wrapper/
│
├── config.mk
├── constraint.sdc
└── rtl/
    ├── user_project_wrapper.v
    ├── debug_regs.v
    └── defines.v
    <br/>
Description:
config.mk → Design configuration and flow parameters
constraint.sdc → Timing constraints
rtl/ → RTL source files
 <br/>
The following RTL files are included:

user_project_wrapper.v → Top-level module
debug_regs.v → Debug register module
defines.v → Global macro definitions (Caravel platform)

# PHASE 3 — Apply 100 MHz Clock Constraint
<img width="1337" height="192" alt="image" src="https://github.com/user-attachments/assets/42b2cfdc-5804-4d91-ae94-fd6d9a23983f" /> <br/>
# The Constraint File Used

To enable timing-driven synthesis and timing verification, a Synopsys Design Constraints (SDC) file named `constraint.sdc` was created and included in the ORFS flow. The file contains the clock definition required for timing analysis:

```tcl
create_clock -name wb_clk_i -period 10 [get_ports wb_clk_i]
```

This constraint defines a clock with a period of 10 ns, corresponding to a target operating frequency of 100 MHz. By providing this information to the synthesis and timing tools, the design can be optimized and analyzed against the intended performance requirements.

# Explanation of How the Clock Port Was Identified

The clock source was identified by examining the top-level module, `user_project_wrapper`, and tracing the signals connected to the synchronous logic within the design. The signal `wb_clk_i` was found to be the primary clock input distributed through the Wishbone interface and used by the internal modules for sequential operation.

Since `wb_clk_i` drives the flip-flops and synchronous elements of the design, it was selected as the reference clock for timing analysis. Based on the required operating frequency of 100 MHz, a clock period of 10 ns was assigned in the constraint file.

# Confirmation That the Constraint Was Recognized During the Flow

The successful application of the clock constraint was verified during the ORFS synthesis and timing-analysis flow. Evidence that the constraint was correctly recognized included:

* Detection of the `wb_clk_i` clock in timing reports.
* Successful execution of Static Timing Analysis (STA).
* Calculation and reporting of setup and hold timing slack values.
* Absence of warnings related to undefined or missing clocks.
* Timing optimization being performed with respect to the specified 10 ns clock period.

These observations confirmed that the synthesis and timing tools successfully read the `constraint.sdc` file and applied the defined clock constraint throughout the implementation flow.

# PHASE 4 — Run the RTL-to-GDS Flow
1. Synthesis <br/>
<img width="1457" height="742" alt="image" src="https://github.com/user-attachments/assets/f5cfe903-87af-4859-9c00-66df9ee7613e" /> <br/>
<img width="623" height="660" alt="image" src="https://github.com/user-attachments/assets/8e4596d2-030f-4964-b453-888478f87fb9" /> <br/>
2. Floorplanning <br/>
<img width="970" height="498" alt="image" src="https://github.com/user-attachments/assets/1db6e22e-f602-4d69-bf51-f7925b6c23cf" /> <br/>
<img width="1317" height="768" alt="image" src="https://github.com/user-attachments/assets/5ec4a55b-4543-4d48-8eb2-9ecc250b0cd8" /> <br/>
3. Placement <br/>
<img width="1327" height="772" alt="image" src="https://github.com/user-attachments/assets/d46c42e8-bac5-463e-8744-443640bbf21f" /> <br/>
<img width="1096" height="737" alt="image" src="https://github.com/user-attachments/assets/214feb65-5e3f-4435-9b92-07cfcc3e76e7" /> <br/>
4. Clock Tree Synthesis <br/>
Min Clock Period = 2.53ns and a maximum frequency of 394.48MHz. <br/>
<img width="1056" height="800" alt="image" src="https://github.com/user-attachments/assets/2fe7c9fe-589b-45ea-b5b2-8981e2d04c9d" /> <br/>
<img width="906" height="807" alt="image" src="https://github.com/user-attachments/assets/054bc77b-8efb-4da7-b281-15b27a3dd1c6" /> <br/>
5. Routing <br/>
<img width="1057" height="743" alt="image" src="https://github.com/user-attachments/assets/c31ce47a-0c9f-4015-8177-2c410dfc03ee" /> <br/>
6. Fill Insertion <br/>
<img width="1321" height="266" alt="image" src="https://github.com/user-attachments/assets/2e982701-8f01-405c-8a0c-135bb9caac29" /> <br/>
Final Database Generation <br/>
In the final database generation stage, 6_final.odb the routed and filled design database is finalized for signoff checks and downstream export. The 6_final.odb file is the creation of the last physical-design database inside the EDA tool after routing, fill insertion, and signoff checks.  <br/>
7.  Final GDS generation <br/>
<img width="447" height="480" alt="image" src="https://github.com/user-attachments/assets/23ca6e20-3ee1-40e1-bbb1-87c641e52aa5" /> <br/>
8. Timing Report <br/>
WNS = 0, TNS = 0 and a Worst slack is 7.52ns <br/>
<img width="977" height="715" alt="image" src="https://github.com/user-attachments/assets/e57f274d-fec0-4dbe-b33c-9ce79e94b7dc" /> <br/>

# PHASE 5 — Generate Outputs for Gate-Level Verification Preparation
<img width="1717" height="292" alt="image" src="https://github.com/user-attachments/assets/eb181cac-49e4-42f0-9730-0b73be659c77" />
<img width="1390" height="750" alt="image" src="https://github.com/user-attachments/assets/31993eb0-48c3-4c49-93b9-63e133d011e6" /> <br/>
Design Outputs Generated During the RTL-to-GDSII Flow

The RTL-to-GDSII implementation process generates a series of files that capture the design at different stages of development. These outputs are essential for verification, analysis, debugging, and eventual fabrication of the chip.

The synthesized netlist is the first gate-level representation of the design. Generated during the synthesis stage, it converts the RTL description into a network of standard cells and logic gates while preserving the intended functionality. This file serves as the starting point for all subsequent physical design activities.

Location:
/home/vsdsquadron/workspace/vsd-scl180-orfs/orfs/flow/results/sky130hd/user_project_wrapper/base/1_2_yosys.v

After placement, clock tree synthesis, routing, and optimization have been completed, the flow produces the final netlist. This version reflects the implemented design and includes modifications introduced during physical design, such as inserted clock buffers and optimized clock distribution structures.

Location:
/results/sky130hd/user_project_wrapper/base/6_final.v

The routed database captures the complete state of the design after routing. In addition to logical connectivity, it stores physical information such as cell locations, routing paths, power distribution networks, technology references, and timing-related data. This database allows the implementation environment to be restored without rerunning previous stages of the flow.

Location:
/results/sky130hd/user_project_wrapper/base/5_route.odb

To prepare the design for fabrication, the routed database is transformed into a final filled database. This version includes manufacturing-specific additions such as filler cells, dummy metal structures, and redundant vias that help satisfy foundry design and density requirements.

Location:
/results/sky130hd/user_project_wrapper/base/6_1_fill.odb

Once all implementation and manufacturability requirements have been met, the design is exported as a GDSII file. GDSII is the industry-standard format used by semiconductor foundries and contains the geometric layout of the chip, including polygons, routing shapes, and layer information required for mask generation.

Location:
/results/sky130hd/user_project_wrapper/base/6_final.gds

Throughout the implementation process, timing reports are generated using Static Timing Analysis (STA). These reports evaluate setup and hold timing performance, identify timing violations, and verify that the design can operate at the target clock frequency. Timing analysis is performed at multiple stages, including after clock tree synthesis and at the completion of the flow.

Final Timing Report Location:
/home/vsdsquadron/workspace/vsd-scl180-orfs/orfs/flow/reports/sky130hd/user_project_wrapper/base/6_finish.rpt

CTS Timing Report Location:
/home/vsdsquadron/workspace/vsd-scl180-orfs/orfs/flow/reports/sky130hd/user_project_wrapper/base/4_cts_final.rpt

Together, these outputs provide a complete view of the design's progression from RTL code to a fabrication-ready integrated circuit, enabling both logical verification and physical validation before tape-out

# PHASE 6 — Debugging and Issue Resolution
<img width="1320" height="528" alt="image" src="https://github.com/user-attachments/assets/9072cb1f-327d-4d93-9c00-0c9f3a0cb40f" />
<img width="1150" height="681" alt="image" src="https://github.com/user-attachments/assets/54f1b668-b812-4cd3-9bad-ee6b54a32441" /> <br/>
The synthesis run failed due to a missing OpenROAD executable path. Even though synthesis focuses on logic transformation rather than physical implementation, OpenROAD is invoked to generate the .odb database required for downstream stages of the flow. As a workaround, the die area and core area were defined explicitly in config.mk, eliminating the need to rely on automatic area calculation through the CORE_UTILIZATION setting.
