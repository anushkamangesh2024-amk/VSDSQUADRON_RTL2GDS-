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
<img width="1457" height="742" alt="image" src="https://github.com/user-attachments/assets/f5cfe903-87af-4859-9c00-66df9ee7613e" />
<img width="623" height="660" alt="image" src="https://github.com/user-attachments/assets/8e4596d2-030f-4964-b453-888478f87fb9" />
2. Floorplanning <br/>
<img width="970" height="498" alt="image" src="https://github.com/user-attachments/assets/1db6e22e-f602-4d69-bf51-f7925b6c23cf" />
<img width="1317" height="768" alt="image" src="https://github.com/user-attachments/assets/5ec4a55b-4543-4d48-8eb2-9ecc250b0cd8" />
3. Placement
<img width="1327" height="772" alt="image" src="https://github.com/user-attachments/assets/d46c42e8-bac5-463e-8744-443640bbf21f" />
<img width="1096" height="737" alt="image" src="https://github.com/user-attachments/assets/214feb65-5e3f-4435-9b92-07cfcc3e76e7" />
4. Clock Tree Synthesis
Min Clock Period = 2.53ns and a maximum frequency of 394.48MHz.
<img width="1056" height="800" alt="image" src="https://github.com/user-attachments/assets/2fe7c9fe-589b-45ea-b5b2-8981e2d04c9d" />
<img width="906" height="807" alt="image" src="https://github.com/user-attachments/assets/054bc77b-8efb-4da7-b281-15b27a3dd1c6" />
5. Routing
<img width="1057" height="743" alt="image" src="https://github.com/user-attachments/assets/c31ce47a-0c9f-4015-8177-2c410dfc03ee" />
6. Fill Insertion
<img width="1321" height="266" alt="image" src="https://github.com/user-attachments/assets/2e982701-8f01-405c-8a0c-135bb9caac29" />
Final Database Generation
In the final database generation stage, 6_final.odb the routed and filled design database is finalized for signoff checks and downstream export. The 6_final.odb file is the creation of the last physical-design database inside the EDA tool after routing, fill insertion, and signoff checks. 
7.  Final GDS generation
<img width="447" height="480" alt="image" src="https://github.com/user-attachments/assets/23ca6e20-3ee1-40e1-bbb1-87c641e52aa5" />
8. Timing Report
<img width="977" height="715" alt="image" src="https://github.com/user-attachments/assets/e57f274d-fec0-4dbe-b33c-9ce79e94b7dc" />
