# ChronoBit'26 — Digital Clock Using Discrete IC Components
<p align="center">**Team Quantum spark · Hackathon Project**</p>

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
    Day of Week
          ↓
     Display Bank
```

## ● Breadboard

<p align="center">
  <img src="./Circuit%20image.png" width="400" alt="Circuit Image">
</p>

## ● Demo video

<p>
  <a href="./Circuit1.mp4">▶️ Watch Circuit Working</a>
</p>
