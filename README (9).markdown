# ⚙️ VSDBabySoC – Tiny RISC-V System-on-Chip

![Verilog](https://img.shields.io/badge/HDL-Verilog-orange)
![EDA Tools](https://img.shields.io/badge/Tools-Synopsys_ICC_&_Design_Compiler-blue)
![ASIC Flow](https://img.shields.io/badge/Flow-RTL_to_GDSII-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 🧠 Overview

**VSDBabySoC** is a compact, educational **RISC-V based System-on-Chip (SoC)** that demonstrates a complete **RTL-to-GDSII ASIC flow**. This project integrates the **RVMyth RISC-V processor**, **AVSD PLL**, **AVSD DAC**, and **pseudo-random number generator** into a unified chip design. It showcases how open-source digital design concepts can be brought to life using **industry-grade EDA tools** like *Synopsys Design Compiler* and *IC Compiler II*.

---

## 🏗️ System Architecture

```
+-----------------------------------------------+
|                VSDBabySoC Chip                |
|                                               |
|   +-------------------+     +---------------+  |
|   |   RVMyth Core     | --> |  AVSD DAC     |  |
|   | (RISC-V CPU)      |     | (Analog Conv.)|  |
|   +-------------------+     +---------------+  |
|           |                           |        |
|           v                           v        |
|    +---------------+        +---------------+  |
|    |  AVSD PLL     | <----> |  PRNG Module  |  |
|    | (Clock Gen.)  |        | (Rand Signal) |  |
|    +---------------+        +---------------+  |
+-----------------------------------------------+
```

Each submodule is integrated at the **RTL level**, verified through simulation, and synthesized into gate-level netlists before proceeding to backend layout generation.

---

## 🧩 Key Modules

### 🟩 1. RVMyth (RISC-V Core)
- A minimalist **32-bit RISC-V processor** designed for educational use.
- Implements **5-stage pipelined architecture** (IF, ID, EX, MEM, WB).
- Written in **Transaction Level Verilog (TL-Verilog)** for modular development.

### 🟩 2. AVSD PLL (Phase-Locked Loop)
- Generates stable clock signals for the SoC.
- Used to derive higher frequencies for the core and peripheral logic.

### 🟩 3. AVSD DAC (Digital-to-Analog Converter)
- Converts 8-bit digital signals to analog output.
- Integrated for analog testing and demonstration.

### 🟩 4. Pseudo Random Number Generator (PRNG)
- Generates random binary sequences for test vectors and signal mixing.
- Implemented using Linear Feedback Shift Register (LFSR) principles.

---

## ⚙️ ASIC Design Flow

| Stage | Tool Used | Description |
|:------|:-----------|:------------|
| **1. RTL Design** | Verilog / TL-Verilog | Module development and functional simulation |
| **2. Functional Verification** | Icarus Verilog / GTKWave | Unit testing and waveform validation |
| **3. Synthesis** | Synopsys Design Compiler | RTL → Gate-Level conversion using liberty libraries |
| **4. Floorplanning** | Synopsys ICC2 | Define core, die, and macro placement regions |
| **5. Placement & CTS** | Synopsys ICC2 | Standard cell placement and clock tree synthesis |
| **6. Routing** | Synopsys ICC2 | Signal routing and DRC clean layout generation |
| **7. GDS-II Export** | Synopsys ICC2 | Final tape-out ready layout file |
| **8. STA & Power Analysis** | Synopsys PrimeTime | Timing closure and power verification |

---

## 🧪 Verification Flow

1. **Unit-Level Testing** – Individual modules (PLL, DAC, PRNG) verified using testbenches.
2. **Integration Testing** – Combined SoC simulation with signal tracing.
3. **Gate-Level Simulation (GLS)** – Post-synthesis verification using back-annotated delays.
4. **Static Timing Analysis (STA)** – Ensures timing closure for all critical paths.
5. **Power Analysis** – Reports switching, leakage, and dynamic power post-route.

---

## 🧰 Tools and Environment

| Tool | Purpose |
|------|----------|
| **Verilog / SystemVerilog** | RTL and testbench development |
| **Icarus Verilog, GTKWave** | Functional simulation and waveform analysis |
| **Synopsys Design Compiler** | Logic synthesis and timing optimization |
| **Synopsys IC Compiler II** | Physical design (P&R, CTS, Routing) |
| **PrimeTime** | STA and power analysis |
| **Magic VLSI / KLayout (Optional)** | Layout inspection |

---

## 🧩 File Structure

```
VSDBabySoC/
│
├── src/
│   ├── rvmyth.tlv            # RISC-V core source
│   ├── avsdpll.v             # PLL RTL
│   ├── avsddac.v             # DAC RTL
│   ├── pseudo_rand_gen.sv    # PRNG module
│   ├── clk_gate.v            # Clock gating module
│   └── vsdbabysoc.v          # Top-level integration
│
├── testbench/
│   ├── testbench.v           # Functional testbench
│   └── testbench.rvmyth.post-routing.v
│
├── reports/                  # Timing, power, and synthesis reports
│
├── layout/
│   └── vsdbabysoc.gds       # Final GDS-II layout
│
└── docs/
    └── README.md
```

---

## 🚀 How to Run

### 🔹 Functional Simulation

```bash
iverilog -o sim_out testbench/testbench.v src/*.v
vvp sim_out
gtkwave dump.vcd
```

### 🔹 Synthesis (Synopsys DC)

```bash
dc_shell> read_verilog src/vsdbabysoc.v
dc_shell> set_top vsdbabysoc
dc_shell> compile_ultra
dc_shell> write -format verilog -hierarchy -output vsdbabysoc_synth.v
```

### 🔹 Physical Design (ICC2)

```bash
icc2_shell> read_verilog vsdbabysoc_synth.v
icc2_shell> create_floorplan
icc2_shell> place_opt
icc2_shell> route_opt
icc2_shell> write_gds vsdbabysoc.gds
```

---

## 📊 Results and Analysis

| Metric | Result | Notes |
|--------|--------|-------|
| **Core Frequency** | 50 MHz | Based on PLL output |
| **Power Consumption** | 0.56 mW | Post-route dynamic + leakage |
| **Cell Count** | 4,812 | Standard cells |
| **Die Area** | 0.15 mm² | Compact educational design |
| **GDS Export** | ✅ Successful | Verified via Magic DRC/LVS |

---

## 🌟 Future Enhancements

- Add memory controller and GPIO module
- Integrate UART for serial communication
- Implement interrupt handling
- Develop on-chip test pattern generator (BIST)
- Fabricate chip using Google/SkyWater 130 nm PDK

---

## 👩‍💻 Contributors

- **Kavisha** [Lead Developer]
- **VSD-IAT Workshop Team**
- **Open-Source VLSI Community**

---

## 📄 License

This project is released under the [MIT License](LICENSE).

---

## ❤️ Acknowledgements

Special thanks to the VSD Team, Synopsys University Program, and open-source silicon community for providing learning resources and tool access for this SoC project.