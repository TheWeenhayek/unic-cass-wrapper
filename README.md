# UNIC-CASS Mock Tapeout Project  
## Integrated Temperature-Controlled Desorption Driver – 3C-CO₂  
### Digital Design Team Repository

[Librelane Digital Flow (UNIC-CASS)](https://github.com/unic-cass/unic-cass-wrapper/actions/workflows/digital-flow.yaml)

---

## 1. Project Overview

This repository contains the digital design and chip-level integration of a **low-power PWM controller** intended to regulate the thermal desorption stage of a **fixed-bed CO₂ capture system** (3C-CO₂ project).

The work was developed as part of the **UNIC-CASS Mock Tapeout** and demonstrates:

- Digital RTL design  
- Macro hardening using LibreLane  
- Integration into the UNIC-CASS chip wrapper  
- Understanding of chip-level flows, PDN, and layout constraints  

---

## 2. System Context

The overall system is a **temperature-controlled desorption driver**.  
The digital controller operates after the sensing and analog-to-digital conversion stages, which are assumed to be external to this design.

System chain:

Temperature Sensor → ADC → Digital PWM Controller → Heater Driver

The PWM output modulates the power delivered to a heating element used during the CO₂ desorption phase.

---

## 3. Digital Temperature Regulation System

The digital core implements a **simple, low-complexity PWM-based control strategy**, suitable for low-power operation and educational prototyping.

### 3.1 Functional Description

**Inputs (from UNIC-CASS wrapper):**
- `clk_i` — Clock signal  
- `rst_ni` — Active-low reset  
- `ui_PAD2CORE[16:0]` — Input bus  
  - `ui_PAD2CORE[1:0]` → Reference temperature (`ref_bits`)  
  - `ui_PAD2CORE[3:2]` → Measured temperature (`state_bits`)  

**Output:**
- `uo_CORE2PAD[0]` — PWM control signal  

**Internal signals:**
- `counter` — 2-bit PWM counter (period = 4 cycles)  
- `effective_duty` — Computed duty cycle based on reference vs. state  

---

## 4. Repository Structure

Key directories:

```
.
├── unic_cass_wrapper_user_project/
│   └── pwm_desorption/
│       ├── src/
│       │   ├── pwm1.v
│       │   └── user_project_example.v
│       ├── config.json
│       └── final/
│           ├── gds/
│           ├── lef/
│           ├── lib/
│           ├── pnl/
│           └── spef/
│
├── unic_cass_wrapper/
│   ├── src/
│   │   └── user_project_wrapper.sv
│   ├── config.json
│   └── final/
│
├── IHP-Open-PDK/
├── librelane/
├── Makefile
└── README.md
```
---

## 5. UNIC-CASS Wrapper Integration

The hardened PWM macro is instantiated in:

`unic_cass_wrapper/src/user_project_wrapper.sv`

Macro artifacts (GDS, LEF, NL, LIB, SPEF) are registered in:

`unic_cass_wrapper/config.json`

The integration strictly follows the **UNIC-CASS required interface**, without modifying pad ring, clock, reset, or power pad definitions.

---

## 6. Chip-Level Flow Execution and Power Grid Issue

The chip-level flow was executed using LibreLane with the UNIC-CASS wrapper, including floorplanning, macro placement, routing, and pad-ring integration.

---
## 7. Final Remarks and Ongoing Work

The mock tapeout process allowed the team to execute the complete digital integration flow, from RTL design to chip-level assembly using the UNIC-CASS wrapper. Throughout this process, several chip-level challenges were encountered, particularly related to power distribution and physical integration constraints.

These challenges provided valuable insight into power grid design, macro integration, and system-level trade-offs in digital ASIC flows. The team is currently working on refining the power distribution strategy and layout parameters to further improve robustness and prepare the design for future iterations.

Despite these challenges, the project successfully demonstrates the team’s understanding of the UNIC-CASS design methodology, the LibreLane-based flow, and the practical considerations involved in chip-level integration.

