# FXO Line Interface

The FXO (Foreign Exchange Office) interface is the core analog front-end of the Uniline device. It provides safe, isolated, standards-compliant access to the POTS/PSTN copper loop while enabling DATA, VOICE, and TTY functionality.

This document describes the requirements, recommended topology, and safety considerations for building Uniline’s FXO hardware.

---

# Goals

The FXO interface must:

* Provide **galvanic isolation** between line and device
* Support **monitor mode** (high-impedance passive listening)
* Support **talk mode** (off-hook, seizing the line)
* Detect: ring, loop current, polarity, voltage levels
* Handle **voice, modem, and TTY tones** cleanly
* Survive surge/lightning transients
* Avoid interfering with life-safety systems
* Be safe to touch, modify, and prototype

---

# Functional Overview

```
Analog Line (RJ11 / 66 / 110)
      │
      ▼
[ Surge + Lightning Protection ]
      │
      ▼
[ Isolation Barrier ] ← transformer or solid-state
      │
      ├──► Monitor path (high impedance)
      │
      └──► Off-hook / Seize path (relay or SSS)
              │
              ▼
        Audio + DSP Section
```

---

# Core Components

## 1. **Surge + Lightning Protection**

POTS lines can carry high-energy spikes.
Recommended components:

* **GDT** (Gas Discharge Tube) across tip+ring
* **PTC resettable fuse** in series with each leg
* **MOV** or **TVS diode** on each leg (post-GDT)
* **RC snubbers** depending on relay use

This protects the device during storms, faults, and inductive kicks.

---

## 2. **Isolation Barrier**

Two recommended approaches:

### **A. Transformer-Coupled (Traditional FXO)**

Pros:

* Excellent isolation
* Clean audio band response
* Works with modems + TTY

Cons:

* Larger components
* Requires impedance matching

### **B. Solid-State Isolation (Capacitive or Opto)**

Pros:

* Compact
* Easier PCB routing

Cons:

* Must ensure proper AC coupling
* Audio quality varies

Transformer isolation is preferred for first-generation Uniline.

---

## 3. **High-Impedance Monitor Path**

Used in MONITOR mode.

Requirements:

* 470 kΩ – 1 MΩ resistor network
* Differential sensing
* DC blocking (capacitors)
* Does **not** trip off-hook conditions

This allows:

* Listening to dial tone
* Detecting modems or TTY tones
* Inspecting line noise

---

## 4. **Off-Hook / Seize Circuit**

Used in TALK and DATA modes.

Approaches:

### **A. Relay-Based**

* Simple SPST or DPST relay
* Click sound is acceptable
* Robust

### **B. Solid-State Hook Switch (Silicon Labs / Clare)**

* Silent
* Lower power
* More compact

Requirements:

* Presents ~600 Ω load when active
* Current sensing for loop detection
* Off-hook detection logic for firmware

---

## 5. **Ring Detection**

Ring specs:

* 70–90 Vrms
* 20–25 Hz

Typical method:

* Optocoupler with rectified detection
* Series resistor + zener clamp

Provide ring events to firmware via interrupt.

---

## 6. **Line Voltage + Polarity Sensing**

Useful for:

* Detecting active lines
* Differentiating PBX extensions vs CO lines
* Diagnostic data

Implementation:

* Rectified sense path
* Divider to MCU ADC
* Polarity detection diode network

---

# Audio Path Considerations

The FXO must pass:

* Voice (300–3400 Hz)
* Modem tones (V.21 → V.34)
* TTY Baudot (45.45 baud, 1400/1800 Hz tones)

Guidelines:

* Keep audio filtering minimal
* Use coupling capacitors sized for low-frequency response
* Maintain transformer linearity

---

# Safety and Compliance

Uniline is not a commercial telecom device, but the hardware must still follow safe design patterns.

### Isolation Guidelines

* ≥ 1500 Vrms isolation recommended
* Creepage/clearance ≥ 2–3 mm
* Maintain separate analog/digital grounds

### Do **Not** Connect Directly To:

* Life-safety circuits (alarm panels, fire systems) except in **monitor mode only**
* Unknown telco feeds
* Damaged lines with unknown voltage

---

# Testing Procedures

Prototype testing should include:

* Ring detection verification
* On-hook/off-hook transitions
* Audio quality monitoring
* Modem tone pass-through tests
* TTY call simulation (send + receive)
* Surge protection functionality (bench testing)

---

# Status

FXO design is early and evolving. Improvements, reference circuits, and validated schematics will be added as hardware development progresses.

---

# License

Released under **CC0 1.0 Universal** (Public Domain).
