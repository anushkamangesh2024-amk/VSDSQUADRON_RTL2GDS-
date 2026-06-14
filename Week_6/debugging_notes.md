# Debugging and Insights


Challenges Encountered During GLS Integration

The transition from RTL simulation to gate-level simulation required several modifications to the verification environment. Most of the issues were related to simulation setup rather than design functionality.

<img width="987" height="457" alt="image" src="https://github.com/user-attachments/assets/54f16ad5-f47f-416a-852f-bb1e4e36a196" />

Step 1 — Netlist Validation

Before running GLS, the generated netlist was inspected to confirm:

Correct module name
Presence of all I/O ports
Successful completion of synthesis and routing

This ensured that the verification environment was targeting the correct implementation file.

Step 2 — Dependency Resolution

The netlist contained SKY130 standard-cell instances that are not understood by the simulator by default. Library model files were therefore included so that every instantiated cell could be resolved during compilation.

Step 3 — Simulation Flow Update

Instead of creating a new verification framework, the existing simulation flow was extended. The RTL design reference was replaced with the generated gate-level netlist while preserving the original testbench infrastructure.

Step 4 — Functional Validation

Both standalone and system-level simulations were executed. Waveforms and console outputs were reviewed to verify:

SPI command decoding
Read and write transactions
Address generation
Reset behavior

The observed behavior matched the expected RTL operation.

Important Takeaways
Understanding Dependencies Matters

A synthesized netlist cannot run independently. The simulator must also have access to the standard-cell library models used during synthesis.

GLS Is More Than Replacing RTL

Successful gate-level simulation requires proper integration of libraries, include paths, compilation order, and hierarchy resolution.

Existing Flows Can Be Reused

Rather than building a new verification environment, the original RTL flow can often be extended with minimal modifications, reducing verification effort.

Mixed-Level Simulation Is Effective

Using a gate-level model for the target block while keeping the rest of the SoC at RTL provided a practical and efficient validation strategy.

Functional Validation Remains the Priority

Although gate-level simulation introduces realistic implementation details, the primary objective remains confirming that the implemented design behaves identically to the RTL specification.

Final Reflection

The majority of debugging effort was spent on environment configuration and simulation setup rather than fixing design bugs. Once the correct netlist, libraries, and hierarchy paths were in place, both block-level and SoC-level GLS completed successfully. The exercise provided valuable experience in integrating post-layout netlists into an existing verification flow and highlighted the additional considerations required when moving from RTL verification to implementation-aware simulation.
