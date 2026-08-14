# ChronoBit'26 — Digital Clock Using Discrete IC Components
<p align="center">Team Quantum spark · Hackathon Project</p>

---

## ● Overview

**ChronoBit'26** is a fully discrete-logic digital clock built entirely without a microcontroller.

The clock displays:

- **Hours, minutes, and seconds** in 24-hour format
- Manual time adjustment using push buttons

The project uses CMOS counter ICs, logic gates, a 555 timer, 7-segment displays, LEDs, and discrete logic components.

---

## ● Block Diagram

### Signal Flow Sequence

```text
1 Hz Clock (555 Astable)
          ↓
    Seconds (00–59)
          ↓
    Minutes (00–59)
          ↓
     Hours (00–23)
          ↓
        Date
          ↓
     Display Bank
```

## ● Breadboard

<img width="500" alt="Circuit image" src="https://github.com/user-attachments/assets/987b3121-390f-46b6-a017-3e553d71b398" />

## ● Demo video

https://github.com/user-attachments/assets/d25bee1f-16f6-432a-9312-e56a1962d13d

## ● Theory of Operation

### 1. Clock Source

A **555 Timer IC configured as an astable multivibrator** generates the continuous 1 Hz square wave that acts as the master time base for the clock. The 555's charge/discharge cycle through external resistors and a capacitor sets the oscillation frequency, producing a stable square-wave output at the CLK pin. Each rising edge of this signal increments the seconds counter by one, forming the heartbeat for the entire clock chain.

### 2. Seconds, Minutes, and Hours Modules

Each time unit (Seconds, Minutes, Hours) is built from a pair of **CD4026 decade counters with integrated 7-segment decoder/drivers**:

- **Units counter (0–9):** Counts up on every input pulse and drives one 7-segment digit directly.
- **Tens counter:** Clocked by the *Carry Out* of the units counter, incrementing once per full units rollover.

This mirrors a standard BCD ripple-counting scheme, but the CD4026 conveniently combines the counter and 7-segment driver in a single package, eliminating the need for a separate BCD-to-7-segment decoder like the 7447.

- **Seconds Module:** Counts 00–59
- **Minutes Module:** Counts 00–59, clocked by Seconds' overflow (Carry Out)
- **Hours Module:** Counts 00–23, clocked by Minutes' overflow (Carry Out)

### 3. Reset Logic — CD4073 Triple 3-Input AND Gates (U5:A / U5:B / U5:C)

Since the CD4026 is a natural decade (0–9) counter, each stage needs external logic to force a reset at the correct rollover point:

- **U5:A** monitors the Seconds counters and asserts **S_RST** when the count reaches 60, resetting both seconds ICs to 00.
- **U5:B** monitors the Minutes counters and asserts **M_RST** at count = 60, also generating the **carry pulse into the Hours module**.
- **U5:C** monitors the Hours counters and asserts **H_RST** at count = 24, rolling Hours back to 00 and simultaneously issuing a **midnight pulse** that auto-advances the Day-of-Week module.

Each AND gate effectively decodes a specific counter state and feeds that decoded pulse back to the **Master Reset (MR)** pins of the relevant CD4026 pair.

### 4. Manual Time-Set Inputs

Three push buttons — **SET HOUR**, **SET MIN**, and **SET DAY** — allow direct user correction of the clock:

- Each button pulses the clock input of its respective counter stage, incrementing it by one press.
- **1N4148 diodes** are used to **OR-gate** the manual pulse with the automatic carry/rollover pulse, allowing both sources to drive the same clock line without unwanted backfeeding.

### 5. Display Bank

The clock drives **six common-cathode 7-segment displays** (HH:MM:SS), with each segment current-limited by **330Ω resistors**

This provides a outputs, all driven directly by CMOS logic without a microcontroller or dedicated display driver IC.

### Summary Signal Flow

```text
555 Astable (1 Hz)
        ↓
Seconds (00–59)
        ↓
Minutes (00–59)
        ↓
Hours (00–23)
        ↓
Date

Reset @ 60 → Seconds / Minutes
Reset @ 24 → Hours + Midnight Pulse → Date Module




