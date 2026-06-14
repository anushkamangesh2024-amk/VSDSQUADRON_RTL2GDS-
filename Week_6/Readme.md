# PHASE 1 — Block Selection and Analysis
# RTL Block Analysis – housekeeping_spi

## Overview

The **housekeeping_spi** block was selected for RTL analysis from the Caravel SoC RTL repository. This module implements the Serial Peripheral Interface (SPI) used for chip configuration, housekeeping register access, and flash pass-through operations.

The block was chosen because it is self-contained, has a clearly defined interface, and represents a practical example of a control-oriented digital design.

---

## Top Module

| Property    | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| Module Name | `housekeeping_spi`                                                     |
| Source File | `housekeeping_spi.v`                                                   |
| Function    | SPI controller for housekeeping register access and flash pass-through |
| Design Type | Sequential FSM-based controller                                        |

---

## Module Interface

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

---

## RTL Hierarchy

The design contains a single top-level module with no instantiated submodules. Internally, the functionality is organized into several logical blocks:

```text
housekeeping_spi
│
├── SPI Command Decoder
├── Address Processing Logic
├── Data Processing Logic
├── Finite State Machine (FSM)
├── Pass-Through Control Logic
└── Shift Registers (TX/RX)
```

### Internal Functionality

* Receives SPI commands, addresses, and data.
* Controls read and write transactions.
* Supports streaming and fixed-length transfers.
* Provides pass-through access to management and user flash memories.
* Uses an FSM to manage transaction flow.

---

## RTL Dependencies

The following RTL files are required for successful synthesis and integration:

| File                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `housekeeping_spi.v` | Top-level SPI controller                   |
| `defines.v`          | Global macros and configuration parameters |
| `debug_regs.v`       | Housekeeping subsystem support module      |

### RTL Directory

```text
~/vsd-scl180-orfs/orfs/flow/designs/sky130hd/housekeeping_spi/rtl/
```

---

## Key Observations

* The design is based on a finite state machine controlling SPI transactions.
* Supports both register access and flash pass-through modes.
* Uses separate positive and negative clock-edge logic for SPI timing compliance.
* Includes automatic address incrementing during streaming transfers.
* Implements clean transaction termination through chip-select based reset logic.

---

## Conclusion

The **housekeeping_spi** block is a compact SPI controller that manages housekeeping register access and flash pass-through functionality within the Caravel platform. Its well-defined interface, FSM-based architecture, and limited dependencies make it suitable for RTL analysis and subsequent RTL-to-GDSII implementation.

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
