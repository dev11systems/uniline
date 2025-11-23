# Uniline

> Universal analog-line communication: encrypted data, voice, and TTY text over any surviving phone line.
> A resilience-focused open project from **dev://systems**.

**Uniline** is a compact, multi‑mode analog communication node that turns any working POTS/copper line into a channel for:

* **Encrypted low-bandwidth data** (text, telemetry, coordination)
* **TTY text calls** (Baudot tones for degraded or accessibility communication)
* **Voice calls and buttset functionality**, using a smartphone or laptop as the handset
* **High‑impedance line monitoring** for diagnostics and safety

It is a single device capable of **DATA**, **VOICE**, and **TTY** across one unified physical interface.

**No native apps are required** — all interaction happens through a **single-file Web GUI** (`web/index.html`) that runs fully offline in any modern browser.

**, **VOICE**, and **TTY** across one unified physical interface.

No cell service, Wi‑Fi, or internet required — just the **Uniline** device, a phone or laptop, and an analog line.

---

# Status

⚠️ **Early conceptual design.** Hardware, firmware, API, and crypto components are not production‑ready.

---

# Why Uniline Exists

Modern communication depends on:

* Cell towers
* Power grid
* Routers and IP networks
* Cloud services

These layers fail in:

* Natural disasters
* Grid collapse
* Conflict zones
* Infrastructure outages
* Remote or underserved regions

Yet copper lines (POTS) often remain functional because they are:

* Centrally powered
* Shockingly robust
* Built for emergency survivability
* Routed through diverse infrastructure

**Uniline explores how these surviving lines can carry essential communication when everything else is offline.**

---

# Capabilities

Uniline integrates three communication layers:

## 1. **Encrypted Low‑Bandwidth Data**

Two Uniline nodes dial each other, negotiate an analog modem link, then establish an encrypted digital channel.

Throughput: **300 bps → ~33.6 kbps**

Suitable for:

* Text messages
* Status beacons
* GPS/location bursts
* Sensor telemetry
* Command/control packets
* Minimal protocol tunneling

Encryption sits *above* the modem for simplicity and resilience.

---

## 2. **Voice + Buttset Functionality**

Uniline can emulate a lineman’s handset.

### **Monitor Mode (High‑Impedance)**

* Passive listening
* Does *not* seize the line
* Diagnose dial tone, noise, alarms, modem activity, and TTY

### **Talk Mode (Off‑Hook Voice)**

* Seizes the line
* Allows normal voice calls
* Phone acts as headset/microphone

### Supported Audio Paths

* **USB‑C digital audio**
* **Bluetooth HFP** (offline-capable)

---

## 3. **Integrated TTY Support**

TTY is part of Uniline’s modem/audio stack.

* Full Baudot send/receive
* Auto-detection of incoming TTY tones
* Relay center compatibility
* VCO/HCO hybrid modes
* Text ↔ tone conversion

TTY operates across:

* **Talk Mode** (primary TTY mode)
* **Monitor Mode** (passive decode)
* **Data Mode** (TTY‑over‑data fallback)

---

# Operating Modes

Uniline is multi‑mode, but DATA/VOICE/TTY share a unified interface.

### **1. DATA MODE — encrypted digital link**

* Modem DSP + crypto layer
* Host communicates over USB‑C/BLE
* Optional TTY‑over‑data

### **2. MONITOR MODE — passive listening**

* High‑impedance
* Safe diagnostics
* Detects tone/noise/modem/TTY activity

### **3. TALK MODE — voice + tty**

* Off‑hook control
* Voice calls
* TTY calls
* VCO/HCO hybrids
* DTMF dialing

---

# System Overview

```
PHONE / LAPTOP
   │ USB‑C / Bluetooth
   ▼
+--------------------------+
|       Uniline Node       |
|   modem • tty • buttset  |
+--------------------------+
   │
   ▼
Analog Phone Line (RJ11 / 66 / 110)
   │
   ▼
         PSTN
   │
   ▼
 Another Uniline Node
```

---

# Architecture

```
PHONE (Android / Laptop)
   │  USB‑C Serial + Digital Audio
   │  Bluetooth BLE + HFP
   ▼
┌────────────────────────────┐
│        Uniline Node        │
├────────────────────────────┤
│ Mode Controller (Data/Monitor/Talk)
│ Crypto Layer (E2EE)
│ Modem + TTY DSP
├────────────────────────────┤
│ Analog Audio Switch
│ FXO Line Interface
│ Isolation + Off‑Hook Control
└────────────────────────────┘
   │
   ▼
Analog Line / PSTN
```

---

# Why Uniline Matters in Crisis Environments

Uniline is designed for resilience and humanitarian use. In conflict zones, civil unrest, or infrastructure collapse, it can:

* Support medical/shelter communication when cell networks fail
* Enable long‑distance PSTN‑based contact between communities
* Provide emergency text/voice channels for NGOs and relief teams
* Support accessibility via TTY when voice or data fail
* Operate during long power outages due to PSTN’s centralized power
* Act as a fallback coordination tool for community hubs

---

# Intended Uses

Ethical, non‑destructive use only:

* Disaster response
* Community resilience
* Humanitarian coordination
* Crisis shelters and clinics
* Power + internet outages
* PSTN research
* Accessibility/TTY studies

Life‑safety systems (e.g., fire panels) must be monitored passively unless in legitimate emergencies.

---

# Non‑Goals

Uniline is **not** for:

* Unauthorized telecom access
* Unlawful communication
* High‑speed networking
* Interfering with life‑safety systems

---

# Project Philosophy

Uniline is part of **dev://systems**, exploring:

* Universal coherence in infrastructure
* Humane, resilient engineering
* Open tools for public benefit
* Communication layers that survive systemic failure

---

# Repo Structure

```
Uniline/
  README.md
  LICENSE
  docs/
    overview.md
    architecture.md
    protocol.md
    threat-model.md
    use-cases.md
  hardware/
    schematics/
    bom.md
  firmware/
    endpoint/
  host/
    cli/
    gui/
    mobile/
  web/
    index.html
    README.md
  examples/
    demo-topologies.md
    sample-workflows.md
```

---

# Contributing

Looking for contributors with experience in:

* Telephony / PSTN systems
* Modem DSP
* Crypto protocol design
* Embedded systems
* Disaster-resilient communication
* Accessibility / TTY research

Please open an issue before large contributions.

---

# License

Released under **CC0 1.0 Universal** (Public Domain Dedication).
You may copy, modify, distribute, and use the work without restriction.
See: [https://creativecommons.org/publicdomain/zero/1.0/](https://creativecommons.org/publicdomain/zero/1.0/)
