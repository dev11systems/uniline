# Uniline Hardware

The Uniline hardware module provides the physical interface between a host device (phone or laptop) and the analog telephone network (POTS). It is designed for resilience, safety, and simplicity — enabling DATA, VOICE, and TTY operation with minimal components.

This directory contains documentation and references for:

* The FXO line interface
* Isolation and safety requirements
* Audio routing and switching
* Mode control hardware
* Power design
* PCB schematics
* Bill of materials (BOM)

Uniline is intended for educational, humanitarian, and resilience-focused use.

---

## Goals

* **Safe, isolated access** to analog phone lines
* **Unified hardware path** for data, voice, and TTY
* **Low part-count design** suitable for small-scale or DIY manufacturing
* **High-impedance monitoring** without seizing the line
* **Configurable off-hook control** for voice or data
* **Modular sections**: audio, DSP, detection, FXO interface
* **Compatible with WebSerial/WebUSB firmware** running on the MCU

---

## Hardware Modules

### 1. FXO Line Interface

Handles:

* Line connection (RJ11 / 66 block / 110 block)
* Ring detection
* Off-hook relay or solid-state switch
* Isolation barrier
* Surge protection (MOVs, TVS diodes)

### 2. Audio Path

Supports:

* Voice band audio (300–3400 Hz)
* Modem data
* Baudot TTY tones
* WebAudio routing through host

### 3. High-Impedance Monitor

Allows safe passive listening without off-hook events.

* High-value resistor network
* Differential sensing
* DC blocking

### 4. DSP + Control MCU

* Modem DSP
* TTY encoding/decoding
* Line state sensing
* Mode switching
* WebSerial/WebUSB/BLE control

### 5. Power System

* USB-C input
* Filtering + regulation
* Overcurrent protection
* Optional battery-backed real-time clock

### 6. Protection + Isolation

* Transformer or solid-state isolation
* Surge and lightning protection components
* Fail-safe monitoring path

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
* PBX analog stations
* Residential/commercial copper loops

Do **not** connect to:

* Direct fire panel circuits (except passive monitoring)
* Alarm reporting lines during operation
* Unknown or unverified telco infrastructure

Damage to life-safety systems is prohibited.

---

## Status

Hardware design is in early conceptual development.
Schematics, reference designs, and manufacturing notes are evolving.

---

## License

All hardware documentation is released under **CC0 1.0 (Public Domain)**.
You may use, modify, or redistribute without request.
