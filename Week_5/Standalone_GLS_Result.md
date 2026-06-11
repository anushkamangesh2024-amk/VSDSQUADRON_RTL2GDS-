# Standalone GLS Result 
FAIL: Timer,IRQ and Debug tests <br/>
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

Reason:


Timer Test — FAIL

The timer test passed during RTL simulation but failed during GLS. The most likely cause is the additional clock propagation delay introduced by the synthesized clock tree, which is absent in RTL simulation. As a result, the timer counter may not increment at the expected rate, leading to a timeout during verification. Further timing analysis of the CTS stage and adjustment of simulation timing parameters may be required.

IRQ Test — FAIL

The interrupt test completed successfully at the RTL level but failed during gate-level simulation. This behavior suggests that interrupt-related signals may not be propagating through the implemented logic as expected. The issue could be related to timing effects, synchronization logic, or implementation-specific optimizations introduced during synthesis and place-and-route. Additional netlist and timing inspection is required to identify the source of the failure.

Debug Test — FAIL

The debug interface operated correctly in RTL simulation but failed in GLS. The failure indicates that debug-related signals are not reaching their intended destinations in the implemented design. This may be caused by buffering, logic restructuring, or connectivity issues introduced during backend implementation. A detailed comparison between the RTL and post-route netlist, along with connectivity checks, is recommended to isolate the problem.
