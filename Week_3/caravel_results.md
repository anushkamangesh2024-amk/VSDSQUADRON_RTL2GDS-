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
