# Uniline Hardware

This directory contains all hardware-related documentation, design notes, and build resources for the Uniline node.

The hardware is intentionally simple, modular, and open—designed for resilience, maintainability, and educational clarity.

---

## Hardware Goals

* **Safe** interaction with analog phone lines (PSTN / local loop)
* **Low-complexity** design using widely available components
* **Modular** architecture (FXO module, DSP/MCU module, power subsystem)
* **Hackable** and easy to repair or modify
* **Survivable** in field conditions
* **Open-source** for community improvement

Uniline is *not* intended to replace certified telecom equipment. It is a resilience tool, not a commercial telephone device.

---

## Core Components

### **1. FXO Line Interface Module**

Responsible for:

* line isolation
* surge protection
* on-hook / off-hook control
* 2–4 wire conversion
* audio coupling

Typically includes:

* isolation transformer
* protection components (MOVs, sidactors, resettable fuses)
* SLIC/DAA module (if available)

### **2. MCU / DSP Module**

Handles:

* modem DSP (analog carrier encode/decode)
* TTY Baudot encode/decode
* mode control
* line-state sensing
* USB-C or BLE communication with host

Candidate platforms:

* ESP32-S3
* STM32 variants
* RP2040 + external codec

### **3. Audio Codec**

Provides:

* ADC/DAC paths for modem/TTY/voice
* filtering
* gain control

### **4. Power Subsystem**

* USB-C power input
* optional battery (TBD)
* protection and regulation

Uniline does *not* draw power from the phone line.

---

## Safety Notes

* The analog telephone network can carry high voltages (ring voltage 70–90 Vrms).
* Hardware **must** maintain isolation and meet basic telecom safety practices.
* High-impedance monitor mode should default to safe, non-seizing behavior.
* Do **not** attach Uniline to:

  * fire-alarm control loops
  * elevator emergency circuits
  * other life-safety systems

---

## Directory Structure

```text
hardware/
  README.md        ← this file
  bom.md           ← bill of materials (components + sourcing)
  schematics/      ← diagrams, PDFs, KiCad files
```

---

## Status

The hardware is **early-stage**. Schematics, enclosure design, and PCB files are forthcoming.

Contributions from telecom, embedded, and RF engineers are welcomed.

---

## License

All hardware documentation and designs are released under **CC0 1.0** (Public Domain).
