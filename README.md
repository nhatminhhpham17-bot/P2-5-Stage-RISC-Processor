# P2 - 8-Bit 5-Stage Pipelined RISC Processor
Project P2 - 5-Stage-RISC-Processor -Full Vivado files, ppt and pdf files

A fully functional, cycle-accurate 8-bit RISC processor implemented in Verilog. This core features a classic 5-stage pipeline (Instruction Fetch, Instruction Decode, Execute, Memory, Write-Back) and is designed with robust hazard management and hardware optimizations suitable for FPGA deployment.

##  Repository Contents

This repository contains the complete Vivado project archive alongside comprehensive technical documentation:

*   **`P2_8bit_RISC_5Stage.zip`**: The complete Xilinx Vivado project archive. It includes all Verilog RTL sources, the automated testbench, the embedded ROM, and the custom Python software toolchain. 
    *   *Note:* The root project directory inside this zip was inadvertently named `P3_8bit_RISC_5Stage`. However, all architectural structures and source codes belong entirely to this Project 2.
*   **`P2_Project.pptx`**: Project defense presentation slides. Covers the hardware architecture, hazard resolution strategies, and the Early JMP architectural upgrade.
*   **`P2_8bitcpu.pdf`**: Detailed technical documentation featuring RTL block diagrams, Instruction Set Architecture (ISA) specifications, and waveform timing analyses.

##  Key Architectural Features

### 1. Robust Hazard Resolution
The processor integrates a dedicated `hazard_unit` to ensure data integrity without flushing the pipeline unnecessarily:
*   **Dynamic Data Forwarding:** Implements multi-path forwarding multiplexers to route data directly from the `EX/MEM` and `MEM/WB` pipeline registers back to the ALU inputs, resolving Read-After-Write (RAW) data hazards seamlessly.
*   **Load-Use Stalls:** Automatically detects memory-read dependencies and inserts exactly 1 stall cycle (bubble) to preserve logic correctness.

### 2. Early Jump Resolution (ID Stage)
To optimize control flow, unconditional jumps (`JMP`) are resolved exceptionally early in the **Instruction Decode (ID)** stage rather than waiting for the Execute stage. This architectural enhancement reduces the standard branch penalty from 2 cycles down to just 1 cycle, allowing the processor to achieve an optimized **CPI of 1.47**.

### 3. Advanced ALU & Status Flags
Upgraded from basic implementations, this Arithmetic Logic Unit (ALU) natively tracks a 4-bit status register, supporting:
*   **Z** (Zero), **C** (Carry), **N** (Negative), and **V** (Overflow) flags for advanced conditional branching and arithmetic tracking.

### 4. Synthesis-Ready Embedded Memory
The Instruction Memory (`imem.v`) utilizes hardcoded machine code (Built-in ROM) rather than relying on external `.hex` file reads (`$readmemh`). This guarantees strict synthesis reliability during bitstream generation and ensures true "plug-and-play" capability on hardware.

##  Software Toolchain Ecosystem

Beyond the Verilog RTL, this project is supported by a custom software ecosystem built in Python:
*   **Assembler (`asm8.py`):** Translates custom mnemonic assembly instructions into raw 16-bit machine code ready for the processor's ROM.
*   **Golden Reference Model (`golden_model.py`):** An Instruction Set Simulator (ISS) acting as a non-pipelined, hazard-free software baseline. Used for strict differential testing against the Verilog hardware simulation.

##  Verification

Verification is driven by a self-checking testbench (`tb_risc8_5stage.v`) that automates performance metric extraction. The testbench rigorously executes the assembly program and validates the final CPU state against 10 strict automated hardware condition checks, ensuring cycle-accurate execution.

##  Getting Started / Simulation Guide

1.  Download and extract the `P2_8bit_RISC_5Stage.zip` archive.
2.  Launch **Xilinx Vivado**.
3.  Click **Open Project**, navigate into the extracted folder, and open the `.xpr` project file located inside the mistakenly named `P3_8bit_RISC_5Stage` directory.
4.  In the *Sources* panel, expand *Simulation Sources* and set `tb_risc8_5stage.v` as the top module.
5.  Click **Run Behavioral Simulation**.
6.  Open the Tcl Console to view the automated pass/fail condition outputs and analyze the cycle-by-cycle pipeline execution in the Waveform Viewer.
