## VSD-Squadron RTL to GDSII SOC Implementation

## WEEK-1 (Digital VLSI SoC Design and Planning – Foundation Phase)

<details>
<summary><b>PHASE 1 — OpenLANE Flow and Logic Synthesis Familiarization</b></summary>

The initial phase of the design flow focuses on establishing a robust automated environment using the **OpenLANE pipeline**. This process begins by the `make mount` command, followed by initiating the interactive Tcl shell with `./flow.tcl -interactive`. 

<img width="1591" height="657" alt="Screenshot 2026-05-17 103216" src="https://github.com/user-attachments/assets/72461f80-821f-47ef-8356-e702acbfc16d" />

To ensure the environment is correctly configured for the **Sky130 PDK**, we load the necessary software versioning using `package require openlane 1.0.2`. The design preparation step, executed through `prep -design picorv32a`, is a critical prerequisite that performs several background tasks: it loads the Verilog RTL, reads the specific `config.tcl` parameters, and links the standard cell libraries while creating the directory structure required to store subsequent logs and reports. The prep -design picorv32a command is used to initialize the OpenLane flow specifically for the picorv32a design
. It acts as a foundational step that prepares the environment for the ASIC design flow by loading Verilog RTL files and reading the essential config.tcl parameters. During this process, the tool links the Sky130 standard cell library, applies clock and design constraints, and establishes a dedicated runs/ directory to store all generated logs, reports, and design results

<img width="393" height="511" alt="Screenshot 2026-05-17 103840" src="https://github.com/user-attachments/assets/ac2a326e-e4d7-4f33-b53f-3b7a8f90b904" />

<img width="429" height="496" alt="Screenshot 2026-05-17 103905" src="https://github.com/user-attachments/assets/f02c46a1-df5c-495a-b7c4-1d99f7f961d4" />

Once the environment is prepared, we perform logic synthesis using the Yosys tool via the run_synthesis command. Throughout this stage, the tool performs Technology Mapping, where every logical operation and behavioral description is translated into a specific physical cell—such as NAND, NOR, or complex AOI gates—selected from the Sky130 Standard Cell Library
.
To ensure the design is efficient, Yosys conducts aggressive logic optimizations, including finite state machine (FSM) re-encoding and the insertion of buffers to maintain signal integrity
. While transforming the logic, the tool simultaneously performs an initial Static Timing Analysis (STA) to evaluate the design’s performance against its target clock period
. This automated analysis generates comprehensive reports regarding total cell count, silicon area, and timing metrics like Worst Negative Slack (WNS)
. By analyzing these reports, the flow confirms that the design satisfies its basic performance constraints and is timing-clean—often achieving a zero TNS (Total Negative Slack)—before it advances to the physical design stages of floorplanning and placement. During this optimization, the tool analyzes area and timing to ensure the design meets basic performance constraints.

<img width="407" height="495" alt="Screenshot 2026-05-17 103930" src="https://github.com/user-attachments/assets/ca968fd8-6331-449f-9ce4-198d488cec41" />

pon the successful completion of the logic synthesis process for the picorv32a core using the OpenLANE flow, the generated gate-level netlist provides detailed quantitative metrics regarding the design's overall physical complexity
. The synthesis report explicitly identifies a total cell count of 15,762 standard cells, within which there are exactly 1,613 sequential flip-flops responsible for state retention throughout the processor's logic
. By evaluating the density of these specific registers relative to the total number of logic gates mapped from the Sky130 library, we establish a precise flop ratio of 10.23%, which serves as a foundational benchmark for the design's sequential logic intensity
.
Simultaneously, the initial static timing analysis (STA) performed on this post-synthesis netlist indicates a high-performance outcome that meets all fundamental timing requirements established in the design constraints
. The implementation demonstrates perfect results with a Total Negative Slack (TNS) of 0.00 and a Worst Negative Slack (WNS) of 0.00, which formally confirms that every logic path in the design is currently free from any accumulated timing violations
. This results in a robust and safe setup slack margin of 0.52ns, effectively ensuring that data signals consistently arrive at their capture registers with a significant positive buffer before the required clock edge, thereby establishing a secure margin for subsequent physical design stages

<img width="603" height="467" alt="Screenshot 2026-05-17 103952" src="https://github.com/user-attachments/assets/d5dbcf2f-ba81-45f1-b1f9-d43a427e3b3f" />


</details>

---

<details>
<summary><b>PHASE 2 — Floorplanning, Macros, and Physical Constraints</b></summary>

Floorplanning is the architectural phase where the physical boundaries of the silicon are defined. This involves determining the dimensions for both the **Core** (the internal area where standard logic cells are placed) and the **Die** (the total silicon area including I/O pads). 
<img width="910" height="397" alt="Screenshot 2026-05-17 104705" src="https://github.com/user-attachments/assets/0580aaff-1df0-4cc6-b842-ef9b5bab9988" />

Two critical parameters managed in the `config.tcl` file are the **Aspect Ratio** and the **Utilization Factor**. The utilization factor represents the ratio between the area occupied by the netlist and the total core area. While 100% utilization is theoretically possible, it is practically avoided (typically capped at 50-60%) to leave sufficient "white space" for routing interconnects and power distribution networks
<img width="1082" height="475" alt="Screenshot 2026-05-17 105402" src="https://github.com/user-attachments/assets/af50766d-7d36-4b37-9bf7-7e1cd8840320" />
<img width="1114" height="554" alt="Screenshot 2026-05-17 120113" src="https://github.com/user-attachments/assets/6feb9612-4d0f-4a85-b3ca-9c57b8660573" />

Corresponding def file:

<img width="772" height="551" alt="Screenshot 2026-05-17 120155" src="https://github.com/user-attachments/assets/9c9ddfc7-9196-47bc-9429-55ac81b9970c" />


In this lab, the core utilization was initially set to 10% and subsequently increased to **30%** by overriding the default settings in the `sky130A_sky130_fd_sc_hd_config.tcl` file. 
<img width="1099" height="371" alt="Screenshot 2026-05-17 120555" src="https://github.com/user-attachments/assets/0a947f6b-109a-4236-86a8-93977e70e406" />

This adjustment pack standard cells more densely, which significantly reduces the total die area as evidenced in the generated `.def` files. 
<img width="1128" height="623" alt="Screenshot 2026-05-17 120702" src="https://github.com/user-attachments/assets/b11f3086-d286-4db6-a5f4-f25655a9e9ff" />
<img width="709" height="742" alt="Screenshot 2026-05-17 120953" src="https://github.com/user-attachments/assets/b87fcc42-dda9-499c-a735-74a62bdcf9ea" />

Furthermore, large functional blocks like RAM are treated as **Macros**. Unlike standard cells, these are pre-placed as "black boxes" before the main floorplanning and placement stages. To ensure supply stability and prevent **IR Drop**, which can push signals into the "undefined region" and cause functional failure, we surround these macros with **decoupling capacitors (DECAP)**. These local charge reservoirs provide the high instantaneous switching current needed to maintain stable VDD and VSS levels. Causing change in die area :

**<img width="741" height="741" alt="Screenshot 2026-05-17 121034" src="https://github.com/user-attachments/assets/65d00305-6ea0-4c4e-a3cb-926df37a4201" />

<img width="982" height="656" alt="Screenshot 2026-05-17 121100" src="https://github.com/user-attachments/assets/4c89dc81-152e-427c-b7a0-94197466e8cd" />

<img width="987" height="597" alt="Screenshot 2026-05-17 123315" src="https://github.com/user-attachments/assets/ae36ba3b-5886-48ff-8fc6-eeb1b5333484" />

**
</details>

---

<details>
<summary><b>PHASE 3 — Static Timing Analysis with Ideal Clocks</b></summary>

Phase 3 introduces **Static Timing Analysis (STA)** using the **OpenSTA** tool to verify that the logic meets the required operating frequency. At this foundation stage, we assume an **ideal clock**, meaning the clock signal arrives at every flip-flop simultaneously with zero delay or skew. This allows us to focus exclusively on the combinational logic delays between the launch and capture flip-flops. We utilize a custom `pre_sta.conf` file to analyze the post-synthesis netlist and identify paths where the combinational delay exceeds the allowed clock period minus the setup time and uncertainty.

<img width="747" height="231" alt="Screenshot 2026-05-17 144315" src="https://github.com/user-attachments/assets/a7e9ce40-8e52-4ff6-b76b-3bdb564b4b20" />

<img width="860" height="577" alt="Screenshot 2026-05-17 144743" src="https://github.com/user-attachments/assets/d02694eb-b1f5-4f84-bd72-3a450f513bc7" />


A major contributor to timing violations is **high fanout**, where a single net drives too many subsequent cells (e.g. here we take a fanout of 10), leading to increased capacitive loading and slower signal transitions. We mitigate these issues by **upsizing buffers** to increase drive strength, which reduces RC delay and improves slack. 

<img width="721" height="574" alt="Screenshot 2026-05-17 144805" src="https://github.com/user-attachments/assets/e7344202-334c-4283-b149-ffe4601cf6cd" />

This modifies the slack:

<img width="808" height="570" alt="Screenshot 2026-05-17 144953" src="https://github.com/user-attachments/assets/da17e246-c431-482d-bad1-111c8ae5f20f" />


Additionally, the synthesis strategy can be pivoted; by switching from **"AREA 0"** (focused on minimizing size) to **"DELAY 3"** (prioritizing speed), the tool performs more aggressive logic restructuring to close timing. 

<img width="925" height="266" alt="Screenshot 2026-05-17 154554" src="https://github.com/user-attachments/assets/854b4515-8682-4c51-80ce-d9a35b40ef3d" />


In our tests, this strategic shift resulted in a modified slack value of **-2.79ns**, demonstrating how tool-driven optimizations can drastically alter the timing profile of the netlist.

<img width="824" height="616" alt="Screenshot 2026-05-17 154656" src="https://github.com/user-attachments/assets/cc0754ca-f3b5-4435-99bb-dd283f25e1e3" />

</details>

---

<details>
<summary><b>PHASE 4 — Clock Tree Synthesis and Real Clock Timing</b></summary>

After the logic timing is optimized, we must physically distribute the clock signal using **Clock Tree Synthesis (CTS)**. In real silicon, wires have resistance and capacitance (RC) that distort the clock waveform and cause arrival time variations known as **clock skew**. If the skew is too large, the design becomes unreliable. To solve this, we use the **TritonCTS tool** to construct a balanced distribution network, typically following an **H-Tree topology**. This symmetrical structure ensures that the clock signal travels an equal distance from the source to every flip-flop, theoretically reducing skew to near zero.

<img width="749" height="464" alt="Screenshot 2026-05-17 155421" src="https://github.com/user-attachments/assets/936dffa3-f12b-447c-a254-72c1d884ced0" />
<img width="914" height="636" alt="Screenshot 2026-05-17 155821" src="https://github.com/user-attachments/assets/117996f6-8f62-499c-9efb-343207f7ae43" />
<img width="892" height="618" alt="Screenshot 2026-05-17 155842" src="https://github.com/user-attachments/assets/937402c3-0a76-4319-ad81-16c4c7e68636" />
<img width="903" height="598" alt="Screenshot 2026-05-17 155917" src="https://github.com/user-attachments/assets/998f62f1-1430-4a11-8f08-27b6c9b12f5b" />
<img width="850" height="614" alt="Screenshot 2026-05-17 155945" src="https://github.com/user-attachments/assets/cce6e19d-cd52-46da-80d5-1e9c6632e8ee" />
<img width="862" height="615" alt="Screenshot 2026-05-17 160012" src="https://github.com/user-attachments/assets/267d0a2e-2cf7-4c98-b972-0c6b5004d390" />


<img width="908" height="554" alt="Screenshot 2026-05-17 160044" src="https://github.com/user-attachments/assets/d70ab57a-ae17-4ada-a8b6-ffa595e20e39" />



During this process, the tool inserts a series of **clock buffers** along the branches of the tree. These buffers serve a dual purpose: they restore the signal's drive strength to maintain sharp rise/fall times and protect the signal from RC-induced distortion. Once the physical clock tree is built, we transition from ideal clock analysis to **propagated clock analysis**. This provides a realistic view of the design's performance, accounting for the actual delays introduced by the clock buffers and the physical wires. While setup timing is usually the primary focus, hold timing becomes equally critical at this stage to ensure data remains stable long enough to be captured correctly

<img width="684" height="343" alt="Screenshot 2026-05-17 160102" src="https://github.com/user-attachments/assets/c59adf15-e567-4871-8aa8-29f4582e19f9" />



</details>

---

<details>
<summary><b>PHASE 5 — Power Distribution Network and Routing</b></summary>

The final foundation phase involves establishing the **Power Distribution Network (PDN)** and performing signal routing. The PDN is constructed immediately following floorplanning and before any signal routing occurs. This grid, consisting of VDD and VSS rails and stripes, is designed to carry power across the entire chip with minimal resistance. To achieve this, the power network occupies the **top, thickest metal layers**, which have the lowest resistance and are best suited for high-current delivery. This robust grid prevents **ground bounce** and voltage drops that occur when many logic gates switch simultaneously 
<img width="984" height="515" alt="image" src="https://github.com/user-attachments/assets/937096d9-d3a0-4053-b402-4a05df2773fd" />

<img width="749" height="532" alt="Screenshot 2026-05-17 162807" src="https://github.com/user-attachments/assets/40a6ffa9-7255-48f8-8238-049057cb02a6" />

<img width="935" height="455" alt="image" src="https://github.com/user-attachments/assets/04e2770d-a487-483e-9290-4eaa112ccc92" />

<img width="613" height="642" alt="image" src="https://github.com/user-attachments/assets/ef77336b-94e5-489a-a591-41fff2934c93" />



Once the power grid is established, the flow proceeds to **Routing**, which is divided into two distinct stages. First, **Global Routing (FastRoute)** creates a coarse 3D grid and generates "routing guides" to plan the approximate paths for all nets while managing congestion. Second, **Detailed Routing (TritonRoute)** uses these guides to draw the exact metal geometries and insert vias between layers. This stage ensures that all pins are connected according to the netlist while strictly adhering to **Design Rule Checks (DRC)**. The final output is a clean, routed layout that satisfies all manufacturing requirements and is ready for GDSII generation 

<img width="1012" height="651" alt="image" src="https://github.com/user-attachments/assets/e950d0e4-cdf2-4410-803c-6132a64d4a31" />

<img width="819" height="410" alt="image" src="https://github.com/user-attachments/assets/182825aa-79e9-43dc-80ff-f5901047b3dd" />


<img width="1285" height="794" alt="image" src="https://github.com/user-attachments/assets/3e292fc0-8d09-4fd2-9546-62e086b997d2" />



</details>
