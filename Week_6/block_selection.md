# Block Selection and Analysis
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

