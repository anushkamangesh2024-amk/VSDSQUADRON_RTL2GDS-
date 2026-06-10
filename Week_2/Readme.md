# WEEK 2 — Toolchain Mastery and ORFS Execution [Cloud to Local]
# Phase 1
# Setup
<img width="812" height="490" alt="Screenshot 2026-06-04 234129" src="https://github.com/user-attachments/assets/a0922b3b-07f9-4c3f-a537-3763e828cdd7" />  <br/>
# Synthesis <br/>
Output of make synth  <br/>


<img width="821" height="283" alt="Screenshot 2026-06-09 232512" src="https://github.com/user-attachments/assets/c9b8ebcb-aa44-4b97-9794-132bc7891d5b" /> <br/>
Synthesis report  <br/>
<img width="707" height="282" alt="image" src="https://github.com/user-attachments/assets/2e8df532-22e5-46b0-989a-ef0b367820ac" /> <br/>
The synthesis statistical report detailing the post-synthesis metrics. It shows an initial estimated design area of 61,097 µm² at 100% utilization before physical floorplanning and placement expand the core.

# Floorplanning
The floorplan execution log confirming the core area initialization, standard cell track generation, and an initial design area of 61,496 µm² with 46% utilization.


<img width="898" height="540" alt="image" src="https://github.com/user-attachments/assets/538d4372-12cd-4d1c-bcff-1f2c2d764958" />
Power Delivery Network (PDN)
Terminal output showing the successful insertion of the Power Delivery Network (PDN) grid, which is essential for routing VDD and VSS across the chip without voltage drop.


<img width="1061" height="255" alt="image" src="https://github.com/user-attachments/assets/6c5ac94e-ae33-4850-a968-6c10f2e58fda" />

# Placement
Terminal log for the global and detailed placement stages, showing standard cells legally placed into rows with an updated design area of 69,948 µm² and a core utilization of 52%.


<img width="977" height="411" alt="image" src="https://github.com/user-attachments/assets/be43964f-a4e1-4de5-aceb-eed73f404913" />

# Clock Tree Synthesis (CTS)
The Clock Tree Synthesis log showing buffer insertions and aggressive resizing to repair timing violations, resulting in a design area of 75,857 µm² at 57% utilization.

<img width="1150" height="637" alt="image" src="https://github.com/user-attachments/assets/50ec28e8-7b32-4653-8bf6-ebe025871aad" />

# Routing
Terminal output confirming the successful completion of the global and detailed routing stages, followed by the insertion of 10,342 filler cells.

<img width="1021" height="390" alt="image" src="https://github.com/user-attachments/assets/c4006a61-58ac-4834-bf88-d18398702e13" />

0 violations found in routing

<img width="410" height="128" alt="image" src="https://github.com/user-attachments/assets/89e25829-23eb-4639-822a-e8fa76298405" />


# Final GDS generation

<img width="657" height="577" alt="image" src="https://github.com/user-attachments/assets/39009ea5-8a0f-44d0-8306-809224c549b4" />

# Final Timing report


<img width="752" height="628" alt="image" src="https://github.com/user-attachments/assets/af946692-68f4-4ff7-b4ac-04841fcc34d9" /> <br/>
# Total Run time
Total runtime : 2705 seconds  ~ 45 minutes

<img width="712" height="483" alt="image" src="https://github.com/user-attachments/assets/b7b15f49-1d27-4463-86a2-7ed815918290" />


# Phase 2

Task 2.1<br/>
<img width="1017" height="703" alt="image" src="https://github.com/user-attachments/assets/f7f8282a-ec25-43d7-a888-270ead26bb0e" />
<br/> Task 2.2 <br/>

What ORFS Automates

The OpenROAD Flow Scripts (ORFS) automate the complete digital ASIC design flow, from RTL to GDSII. ORFS automates the synthesis, floorplanning, placement, clock tree synthesis, routing, timing analysis and GDS generation in the correct order, rather than having to run each EDA tool and feed files to it manually. 

How Makefiles Orchestrate the Flow

A Makefile is the flow controller. It describes the dependencies between different design stages and which commands should be executed for a particular stage. The Makefile says that before placement , synthesis must be performed , before clock tree synthesis , placement must be performed , etc . The user just runs a command like make . It also won’t re-run stages that already have up-to-date outputs.

Where Synthesis Ends and Physical Design Begins

The synthesis stage ends after RTL code is converted into a gate-level netlist consisting of standard cells and logic gates. This is typically performed by Yosys. Once the netlist is generated, the flow moves into physical design, where the logical gates are assigned physical locations on the chip. Physical design starts with floorplanning and placement within OpenROAD.

Where Timing Is Checked

Timing is checked throughout the design flow using OpenSTA (Static Timing Analysis). Timing analysis is performed after synthesis to verify that the gate-level netlist meets timing requirements and is repeated after placement, clock tree synthesis, and routing. These checks ensure that setup and hold constraints are satisfied before the design is finalized.

Where GDS Is Produced

The final GDSII file is produced at the end of the physical design flow after routing and design verification are completed. OpenROAD generates the layout data, which is then exported as a GDSII file. This file contains the geometric representation of all layers of the chip and is the format sent to the semiconductor foundry for fabrication. Tools such as KLayout can be used to view and inspect the generated GDS file.

# Phase 3

Install ORFS Locally<br/>
<img width="1146" height="356" alt="image" src="https://github.com/user-attachments/assets/2325ca15-222a-4667-975e-1bbebc569da6" />

<br/>Install Official OpenROAD<br/>
Install OpenROAD from the official OpenROAD repository. <br/>
<img width="1256" height="498" alt="image" src="https://github.com/user-attachments/assets/ee26d921-e3aa-4bfb-b2ec-f78a2e6bc261" /> <br/>
<img width="1347" height="770" alt="image" src="https://github.com/user-attachments/assets/b92a4ff7-8594-48d8-90af-9db7a37b9e18" /> <br/>
Successful compilation of the OpenROAD toolchain from source.
<img width="1232" height="437" alt="image" src="https://github.com/user-attachments/assets/7bcdf06e-7711-407a-8388-94b2b9a5921b" /> <br/>

# Phase 4
Re-Run RTL-to-GDS Locally:
Synthesis <br/>
<img width="450" height="167" alt="image" src="https://github.com/user-attachments/assets/b540bda4-3617-416c-8ec0-6d49bc811a5f" /> <br/>
<img width="856" height="88" alt="image" src="https://github.com/user-attachments/assets/ba020dbf-a208-4ff7-b294-a7af7141f24a" />



