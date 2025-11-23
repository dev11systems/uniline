# Uniline Hardware: Isolation & Safety

> Protecting users, equipment, and the PSTN through proper electrical, acoustic, and logical isolation.

This document describes all isolation and safety systems required for Uniline’s FXO-based analog interface. Because the Public Switched Telephone Network carries its own power and signaling voltages, proper protection is essential for both compliance and user safety.

---

# 1. Goals

Uniline’s isolation and safety design must:

* Prevent hazardous voltages from reaching the user or host device
* Protect the PSTN from damage caused by faults or miswiring
* Maintain high-impedance monitoring capability
* Ensure regulatory-aligned behavior (FCC/CE style) without requiring certification
* Guarantee clean analog paths for modem, TTY, and voice
* Support Talk, Monitor, and Data modes without electrical conflicts

---

# 2. Line Isolation Requirements

## 2.1 PSTN Voltage Characteristics

A standard analog line can present:

* **On-hook:** ~48–52 VDC
* **Off-hook:** ~6–12 VDC
* **Ring Voltage:** 70–150 VAC (20–25 Hz)
* **Transient spikes:** 1–2 kV possible during lightning or switching events

Uniline must withstand all of these without passing risk to the user or phone.

## 2.2 Mandatory Isolation Components

Uniline’s line interface includes:

* **High-voltage isolation barrier** (transformer + optocouplers)
* **Surge protection:** gas discharge tube (GDT), PTC resettable fuse, MOV pair
* **Series protection resistors** for signal attenuation & limiting
* **Transient suppression diodes** (TVS diodes)
* **AC-coupling capacitors** for audio path safety

These ensure:

* No direct electrical continuity between PSTN and the MCU side
* Protection from ring voltage and lightning
* Stable modem/TTY audio

---

# 3. Isolation Architecture

```
PSTN Line
   │
   ▼
[ GDT ]──[ PTC Fuse ]──[ MOV ]
   │
   ▼
[ Surge-Protected High-Impedance Split ]
   │             │
   │             └─► Monitor Path (isolated transformer)
   │
   └─► Talk/Data Path (transformer + DAA)
                     │
                     ▼
             MCU / DSP Domain
             (fully isolated)
```

Key idea: **monitor**, **talk**, and **data** share physical isolation but use separate signal routing.

---

# 4. High-Impedance Monitor Safety

Monitor Mode uses:

* 1+ MΩ effective input impedance
* AC-coupling only (no DC path)
* Dedicated sense-transformer with reinforced insulation

This prevents line seizure and avoids triggering alarms or dial-out devices.

Safety concern addressed: **zero chance of off-hook state through monitoring**.

---

# 5. Talk/Data Mode Safety

When the device goes off-hook:

* A **DAA (data access arrangement)** handles DC termination
* Current is limited and regulated
* Isolation transformer prevents DC leakage to the user/host

Key protections:

* Off-hook relay cannot activate unless MCU explicitly commands
* Watchdog and firmware sanity checks prevent stuck off-hook conditions
* Line-current limiting avoids overstress on PSTN or the device

---

# 6. Ring Voltage Protection

PSTN ring voltage (up to 150 VAC) is mitigated by:

* GDT and MOV clamping
* Transformer isolation
* Input series resistors
* Transient suppression diodes

This ensures the electronics receive only low-voltage audio-frequency signals.

---

# 7. Host Device Protection

The USB-C side is protected by:

* Complete galvanic isolation from PSTN
* ESD diodes and surge clamps
* Isolated DC/DC converter for power
* No direct electrical connection between analog and USB domains

This ensures:

* Your smartphone/laptop can **never** see PSTN voltages
* USB negotiation stays compliant and safe

---

# 8. Acoustic Isolation (TTY + Modem)

Transformers provide:

* AC-coupling
* Ground loop elimination
* Stable impedance matching
* Clean analog bandwidth for:

  * Baudot TTY tones
  * V.21/V.23 modems
  * Bell 103/212A
  * Voice audio

---

# 9. Mode-Based Safety Logic

### 9.1 Firmware checks

Before switching modes, firmware validates:

* No pending DAA errors
* Line not in unsafe voltage range
* No surge events detected by GDT/MOV sensors
* Host connected and ready

### 9.2 Hardware interlocks

* Physical relay cannot energize without MCU output + watchdog confirmation
* Monitor and Talk modes **cannot** be active at the same time
* TTY and Data cannot drive the line without confirmed off-hook state

---

# 10. User Safety Guidelines

Recommended usage guidelines:

* Do not use during an active lightning storm if possible
* Do not connect to unknown wiring in severely damaged buildings
* Monitor Mode first: verify line safety before seizing
* Ensure cabling is intact and not exposed
* Use only via isolated USB-C or BLE

---

# 11. Long-Term Reliability & Fail-Safe Behavior

* PTC fuses self-reset after faults
* MOVs/GDTs take transient stress instead of the device
* Isolation transformer prevents DC damage
* Watchdog resets ensure firmware cannot latch into unsafe states

Overall effect: **Uniline fails safe**, not destructively.

---

# 12. Summary

Uniline provides:

* Full electrical isolation between PSTN and user devices
* Protection from hazardous voltages and surges
* Safe operation in Monitor, Talk, and Data modes
* Standards-aligned behavior (FCC/CE style) without bureaucratic overhead
* Hardware + firmware interlocks for maximum reliability

This isolation platform enables all higher-level Uniline capabilities — encrypted data, voice, and TTY — while maintaining safety across every operating cond
