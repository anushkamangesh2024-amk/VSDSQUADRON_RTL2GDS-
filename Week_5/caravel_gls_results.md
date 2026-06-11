# Caravel GLS results
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
