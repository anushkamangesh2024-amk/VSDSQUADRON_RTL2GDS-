# PHASE 1 — Block Selection and Analysis
RTL Block Study: housekeeping_spi
Selected Block

The housekeeping_spi module was chosen for analysis. It implements the SPI interface used for chip configuration and housekeeping register access within the Caravel platform.

Top Module
Item	Value
Module Name	housekeeping_spi
Source File	housekeeping_spi.v
Function	SPI controller for register access and flash pass-through operations
Inputs and Outputs

Inputs

reset
SCK
SDI
CSB
idata[7:0]

Outputs

SDO
sdoenb
odata[7:0]
oaddr[7:0]
rdstb
wrstb
pass_thru_mgmt
pass_thru_mgmt_delay
pass_thru_user
pass_thru_user_delay
pass_thru_mgmt_reset
pass_thru_user_reset
RTL Hierarchy Explanation

The design consists of a single top-level module, housekeeping_spi, with no lower-level module instantiations. Internally, it contains:

A finite state machine (FSM) controlling SPI transactions.
Command, address, and data processing logic.
Pass-through modes for management and user SPI flash access.
Internal registers for state tracking, addressing, and data shifting.

Hierarchy:

housekeeping_spi (Top Module)
│
├── SPI Command Decoder
├── Address/Data Processing Logic
├── FSM Controller
├── Pass-Through Control Logic
└── Transmit/Receive Shift Registers
Dependent RTL Files
File	Purpose
housekeeping_spi.v	Main SPI controller implementation
defines.v	Global macros and configuration definitions
debug_regs.v	Housekeeping subsystem support module (contextual dependency)

These files are located in:

~/vsd-scl180-orfs/orfs/flow/designs/sky130hd/housekeeping_spi/rtl/

The dependencies provide required macro definitions and supporting housekeeping functionality needed for successful synthesis and integration.


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
<img width="1232" height="672" alt="image" src="https://github.com/user-attachments/assets/920fd800-3591-465b-aab5-d5ceda6820d5" />

 <br/>Final GDS generated <br/>
<img width="576" height="642" alt="image" src="https://github.com/user-attachments/assets/d245dd99-de69-4db8-bb5a-b12e95c2a4ea" />

# PHASE 3 — Generate Implementation Outputs
