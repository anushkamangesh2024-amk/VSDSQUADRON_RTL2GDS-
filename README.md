### VSD-Squadron RTL to GDSII SOC Implementation
#### WEEK-1 (Digital VLSI SoC Design and Planning – Foundation Phase)

The process begins with invoking the OpenLANE flow, which automates the conversion of a hardware description (RTL) into a finalized silicon layout (GDSII) [1, 2]. Preparing the design involves initializing environment variables such as the Process Design Kit (PDK), standard cell libraries, and Synopsys Design Constraints (SDC) to match manufacturing rules [1, 3, 4]. 

During the logic synthesis stage using Yosys, the RTL code is transformed into a gate-level netlist mapped to the Sky130 standard cell library [5-7]. The metrics from this implementation are detailed below:

*   **Flop Ratio:** (Number of Flip-Flops / Total Number of Cells) = 1613 / 15762 = **10.23%** [1, 7].
*   **Synthesis Timing:** The report indicates a Worst Setup Slack of **0.52 ns**, confirming that the design meets initial timing requirements with a positive margin [8].

**(Insert Synthesis Statistics and Timing Report Image Here)**

---

Floorplanning involves defining the physical boundaries of the die and core area based on the dimensions of the logic gates in the netlist [9]. Key parameters like the **Aspect Ratio** (Height/Width) and **Utilization Factor** (Netlist Area / Total Core Area) are defined within the `config.tcl` file [10, 11]. 

In this implementation, the core utilization was initially set at 10% and later increased to **30%** by overriding the configuration in the `sky130A_sky130_fd_sc_hd_config.tcl` file [12, 13]. This adjustment packs standard cells more densely, reducing the overall die area as observed in the resulting DEF files [10, 13]. 

Additionally, large IP blocks such as RAM are treated as **Macros**. Unlike standard cells, these are pre-placed as "black boxes" before the floorplanning and placement of standard logic gates, meaning only their input/output pins are relevant for the current flow [10, 14, 15].

**(Insert Floorplan View and Die Area DEF File Details Image Here)**

---

Static Timing Analysis (STA) is performed using **OpenSTA** to verify setup and hold constraints, initially assuming an ideal clock distribution [16, 17]. A custom `pre_sta.conf` file is utilized to analyze the post-synthesis netlist [18, 19].

To optimize timing, we identify high-fanout nets (e.g., those driving more than 10 cells) and upsize buffers to reduce capacitive loading and RC delay [18, 20, 21]. Furthermore, modifying the synthesis strategy from **"AREA 0"** (size-focused) to **"DELAY 3"** (speed-focused) significantly improves performance, shifting the slack to values such as **-2.79 ns** in optimized scenarios [18, 20].

**(Insert OpenSTA Timing Report and Slack Optimization Image Here)**

---

Clock Tree Synthesis (CTS) is then executed to build a balanced distribution network, often using an **H-Tree** structure to ensure the clock signal reaches all flip-flops with minimal skew [22, 23]. After constructing the tree with buffers to restore signal integrity, timing is re-analyzed using **real, propagated clocks** rather than ideal ones [24-26].

**(Insert CTS Run Results and Clock Tree Screenshot Here)**

---

The **Power Distribution Network (PDN)** is generated immediately after floorplanning but before final signal routing [18, 27]. To minimize IR drop and supply instability, the network utilizes the **top, thickest metal layers** for VDD and VSS, ensuring low-resistance power travel across the entire chip logic [18, 28, 29]. The flow concludes with global and detailed routing to finalize the physical interconnects [30, 31].

**(Insert PDN Generation Log and Final Routed Layout Image Here)**
