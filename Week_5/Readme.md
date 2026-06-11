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

# PHASE 2 — Modify Verification Flow for GLS
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

# Phase 3 — Run GLS for Standalone Tests <br/>
Timer Test — FAIL

The timer test passed during RTL simulation but failed during GLS. The most likely cause is the additional clock propagation delay introduced by the synthesized clock tree, which is absent in RTL simulation. As a result, the timer counter may not increment at the expected rate, leading to a timeout during verification. Further timing analysis of the CTS stage and adjustment of simulation timing parameters may be required.

IRQ Test — FAIL

The interrupt test completed successfully at the RTL level but failed during gate-level simulation. This behavior suggests that interrupt-related signals may not be propagating through the implemented logic as expected. The issue could be related to timing effects, synchronization logic, or implementation-specific optimizations introduced during synthesis and place-and-route. Additional netlist and timing inspection is required to identify the source of the failure.

Debug Test — FAIL

The debug interface operated correctly in RTL simulation but failed in GLS. The failure indicates that debug-related signals are not reaching their intended destinations in the implemented design. This may be caused by buffering, logic restructuring, or connectivity issues introduced during backend implementation. A detailed comparison between the RTL and post-route netlist, along with connectivity checks, is recommended to isolate the problem.
<img width="770" height="691" alt="image" src="https://github.com/user-attachments/assets/fc4d397f-ff03-4421-8163-571b687f5d41" /> <br/>
<img width="802" height="682" alt="image" src="https://github.com/user-attachments/assets/9dbb1687-c4ef-49a3-9a32-19e580b41230" /> <br/>
<img width="1033" height="632" alt="image" src="https://github.com/user-attachments/assets/cb4e6559-b592-4eff-a2c2-424dacc2aab4" /> <br/>
PASS: GPIO Mgmt, mem, uart and spi master
<img width="758" height="690" alt="image" src="https://github.com/user-attachments/assets/6e9f63aa-8136-49df-96f0-d2b7c66ad9f7" />
<img width="662" height="677" alt="image" src="https://github.com/user-attachments/assets/87d3dddb-d4a4-42c2-9710-47ce3dbc9823" />
<img width="1152" height="666" alt="image" src="https://github.com/user-attachments/assets/8c29ed0b-b045-4d64-bda5-a90a68a6cb8f" />

<img width="787" height="695" alt="image" src="https://github.com/user-attachments/assets/226c886f-8c30-44b4-9d50-1d3039c49023" /> <br/>
Summary:

<img width="797" height="372" alt="image" src="https://github.com/user-attachments/assets/09c26214-aa79-4e04-a34f-d1792fe2f392" /> <br/>


# PHASE 4 — Run GLS for Caravel Integrated Tests

<br/>user_pass_thru <br/>
<img width="1198" height="670" alt="image" src="https://github.com/user-attachments/assets/06b737b7-1810-4c02-83fc-ac63bc820d6e" />

<br/>uart<br/>
<img width="1156" height="690" alt="image" src="https://github.com/user-attachments/assets/ee64ddd5-da67-4bbb-a376-fb83bcc95b06" />

<br/>sysctrl<br/>
<img width="931" height="676" alt="image" src="https://github.com/user-attachments/assets/4b776e7d-19e4-45b5-a9dc-ce7bf860ba06" />

<br/>sram_exec<br/>
<img width="926" height="690" alt="image" src="https://github.com/user-attachments/assets/0a983ad0-51fd-4788-88b9-e8de2df81c70" />

<br/>spi_master<br/>
<img width="652" height="675" alt="image" src="https://github.com/user-attachments/assets/101e0014-e888-4cb4-802f-823f2891ac92" />

<br/>pullupdown<br/>
<img width="947" height="672" alt="image" src="https://github.com/user-attachments/assets/8e7a05b7-7fde-40c8-aa42-bf5db06ba85f" />

<br/>pll<br/>
<img width="997" height="692" alt="image" src="https://github.com/user-attachments/assets/3a00f383-618e-4a17-a7cf-f2709c7e2bc5" />

<br/>pass_thru_fix<br/>
<img width="1123" height="683" alt="image" src="https://github.com/user-attachments/assets/b06d5c55-56a4-4bf7-b0cf-77920bed66ba" />

<br/>mem<br/>
<img width="701" height="692" alt="image" src="https://github.com/user-attachments/assets/73ee96fd-d6a3-4317-9a99-247fa871f69a" />

<br/>hkspi_power<br/>
<img width="972" height="687" alt="image" src="https://github.com/user-attachments/assets/499c663c-4ed0-4e0b-bf1d-72ecc5f33408" />

<br/>gpio_mgmt<br/>
<img width="891" height="687" alt="image" src="https://github.com/user-attachments/assets/2a5fc3ed-6124-4cf2-a984-258e80dc06bf" />

<br/>hkspi<br/>
<img width="1037" height="690" alt="image" src="https://github.com/user-attachments/assets/28eba649-2f29-4dda-806d-8702b0242f05" />

<br/> Summary <br/>
<img width="750" height="605" alt="image" src="https://github.com/user-attachments/assets/8a1b5c2b-c388-4146-802a-23234fd191d8" />
<br/> Reason: <br/>
PLL Test — FAIL (Observed in Both RTL and GLS)

The PLL test fails consistently in both RTL and Gate-Level Simulation environments. Since the PLL is an analog/mixed-signal component, its behavior cannot be accurately represented using standard digital simulation models. As a result, key functions such as frequency generation and lock detection are not fully modeled, causing the verification test to fail. This behavior was already present during RTL verification and is not related to synthesis, placement, routing, or any gate-level implementation changes. Therefore, the failure is considered expected and does not indicate an issue with the generated netlist.

SYSCTRL Test — FAIL (Observed in Both RTL and GLS)

The SYSCTRL test exhibits the same failure behavior in both RTL and Gate-Level Simulation. The test relies heavily on timing-dependent interactions and specific clock-driven sequences, making it sensitive to simulation conditions and execution environments. Because the test does not complete within the expected simulation window, it results in a timeout. Since the failure is reproducible in both RTL and GLS, it cannot be attributed to gate-level synthesis or physical implementation effects. The issue is considered pre-existing and may require modifications to simulation timing parameters or testbench settings for successful execution.


