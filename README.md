[![Librelane Digital Flow (UNIC-CASS)](https://github.com/unic-cass/unic-cass-wrapper/actions/workflows/digital-flow.yaml/badge.svg?branch=main&event=push)](https://github.com/unic-cass/unic-cass-wrapper/actions/workflows/digital-flow.yaml)

# UNIC-CASS Mock Tapeout Project  
## Integrated Temperature-Controlled Desorption Driver - 3cco2
### Digital Design Team Repository

---

## 1. Project Overview



The project implements a **low-power digital PWM controller** intended to regulate the thermal desorption stage of a **fixed-bed CO₂ capture system**. The controller operates as part of a temperature regulation loop where a digital representation of temperature is compared against a reference value, and heating power is modulated accordingly.



---

## 2. System Context

The overall application is a **temperature-controlled desorption driver** for CO₂ capture. The digital controller is placed after the temperature sensing and analog-to-digital conversion stages. It converts the measured temperature into a **PWM signal** to regulate the heating element.

---

## 3. Digital Temperature Regulation System

The digital core implements a **multi-stage temperature regulation system** based on on-off control combined with PWM modulation.

### 3.1 Functional Description

The PWM controller regulates the power supplied to a heating resistance.

**Inputs:**

- `clk` — Clock signal from wrapper  
- `rst_ni` — Active-low reset  
- `ref_bits[1:0]` — Reference temperature level  
- `state_bits[1:0]` — Measured temperature level (from ADC)

**Output:**

- `pwm_out` — Pulse-width modulated control signal

**Internal Signals:**

- `counter` — 2-bit counter generating PWM period (0–3)  
- `effective_duty` — Computed duty cycle based on state and reference  

---
