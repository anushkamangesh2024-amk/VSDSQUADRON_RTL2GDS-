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


<img width="752" height="628" alt="image" src="https://github.com/user-attachments/assets/af946692-68f4-4ff7-b4ac-04841fcc34d9" />
# Total Run time
Total runtime : 2705 seconds  ~ 45 minutes

<img width="712" height="483" alt="image" src="https://github.com/user-attachments/assets/b7b15f49-1d27-4463-86a2-7ed815918290" />



