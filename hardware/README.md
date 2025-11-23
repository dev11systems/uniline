# Uniline Hardware

The Uniline hardware module provides the physical interface between a host device (phone or laptop) and the analog telephone network (POTS). It is designed for resilience, safety, and simplicity — enabling **DATA**, **VOICE**, and **TTY** operation with minimal components.

This directory includes full documentation for:

* The FXO line interface
* Isolation and safety requirements
* Audio routing and switching
* High-impedance line monitoring
* Mode control hardware
* Power design
* Enclosure design
* Testing and validation workflows
* PCB schematics
* Bill of materials (BOM)

Uniline is intended for **educational, humanitarian, and resilience-focused use**.

---

## Goals

* **Safe, isolated access** to analog phone lines
* **Unified hardware path** for data, voice, and TTY
* **Low part-count design** suitable for DIY or small-scale manufacturing
* **High-impedance monitoring** without seizing the line
* **Configurable off-hook control** for Talk/Data modes
* **Modular architecture** (audio, DSP, FXO interface, detection)
* **Fully compatible** with WebSerial/WebUSB firmware

---

# Hardware Document Index

### **1. fxo-interface.md — Line Interface & Signaling**

Explains how Uniline connects to analog lines using an isolated FXO circuit. Covers off-hook control, ring detection, loop current sensing, and surge protection.

### **2. audio-path.md — Analog Audio Architecture**

Details the AC-coupled voiceband path supporting:

* Voice audio
* Modem tones
* Baudot TTY tones

Also includes filtering, gain staging, and transformer coupling.

### **3. isolation-and-safety.md — Electrical Safety Model**

Describes the reinforced isolation barrier that protects the user, host, and PSTN. Includes:

* 1–3 kV isolation design
* GDT/MOV/TVS surge stack
* High-impedance monitor safeguards

### **4. line-monitor.md — High-Impedance Monitoring System**

Passive, non-intrusive line monitoring for diagnostics:

* Zero off-hook risk
* Tone detection (dial tone, TTY, modem)
* Noise floor analysis

### **5. power.md — Power Architecture**

Covers:

* USB-C primary power
* Optional battery module
* Isolated DC/DC supply
* Noise-optimized analog power

### **6. enclosure.md — Mechanical & Field Design**

Rugged field-ready enclosure design:

* Port layout (USB-C, RJ11, clips)
* Shock and moisture resistance
* Cable management

### **7. testing.md — Hardware Validation & QA**

Provides full step-by-step procedures for validating:

* Isolation
* Mode behavior
* Off-hook correctness
* TTY/Modem performance
* Environmental durability

### **8. schematics/ — Electrical Reference Designs**

Contains circuit diagrams, PCB layout drafts, and reference implementations.

### **9. bom.md — Bill of Materials**

Component lists for FXO, audio, power, and enclosure modules.

---

## Schematics

See:

```
hardware/schematics/
```

---

## Bill of Materials

See:

```
hardware/bom.md
```

---

## Safety Notice

Uniline should only be connected to:

* Standard POTS lines
* PBX analog station ports
* Residential/commercial copper loops

Do **not** connect to:

* Direct fire panel circuits (except passive monitoring)
* Alarm reporting lines during operation
* Unknown or unverified telco infrastructure

Damage to life-safety systems is prohibited.

---

## Status

Hardware design is in early conceptual development. Schematics, reference designs, and manufacturing notes are actively evolving.

---

## License

All hardware documentation is released under **CC0 1.0 (Public Domain)**. You may use, modify, or redistribute without permission.
