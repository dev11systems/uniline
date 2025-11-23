# Uniline

> Universal analog-line communication: encrypted data, voice, and TTY text over any surviving phone line.
> A resilience-focused open project from **dev://systems**.

**Uniline** is a compact, multi‑mode analog communication node that turns any working POTS/copper line into a channel for:

* **Encrypted low-bandwidth data** (text, telemetry, coordination)
* **TTY text calls** (Baudot tones for degraded or accessibility communication)
* **Voice calls and buttset functionality** using a smartphone as the handset
* **High‑impedance line monitoring** for diagnostics and safety

It is a single device capable of **DATA**, **VOICE**, and **TTY** across one unified physical interface.

No cell service, wi-fi, or internet required. Just the **Uniline** kit, a phone or laptop, and an analog line.

---

# Status

⚠️ **Early conceptual design.**
Hardware, firmware, and cryptographic components are not production‑ready.

---

# Why Uniline Exists

Modern communication depends on:

* Cell towers
* Power grid
* Routers and fiber/IP networks
* Cloud dependency chains

These layers fail in:

* Disasters
* Infrastructure collapse
* Conflict zones
* Grid outages
* Remote or underserved regions

But copper lines (POTS) often remain functional because they are:

* Centrally powered
* Simple and robust
* Built for disaster survivability
* Geographically diverse in their routing

**Uniline explores how to use these surviving lines for essential communication when everything else is offline.**

---

# Capabilities

Uniline supports three integrated communication layers:

## 1. **Encrypted Low‑Bandwidth Data**

Two Uniline nodes can call each other, negotiate an analog modem link, and establish a secure, encrypted digital channel.

Throughput: **300 bps → ~33.6 kbps**

Enough for:

* Text messages
* GPS/location beacons
* Status updates
* Sensor/telemetry packets
* Command/control signals
* Minimal protocol tunneling

Encryption is layered above the modem to keep the analog side simple.

---

## 2. **Voice + Buttset Functionality**

Uniline can behave like a traditional lineman’s handset:

### **Monitor Mode (High‑Impedance)**

* Passive listening
* Does *not* seize the line
* Check dial tone, noise, alarms, TTY tones, or modem traffic

### **Talk Mode (Off‑Hook Voice)**

* Seizes the line
* Allows voice calls
* Smartphone acts as the microphone/headset

### Phone Audio Options

* **USB‑C (digital audio)** – clean, reliable
* **Bluetooth HFP** – wireless, works offline

This keeps the hardware minimal while leveraging your phone as the handset.

---

## 3. **Integrated TTY Support (Text Calls via Baudot)**

TTY is built directly into Uniline's audio/modem stack.

TTY capability includes:

* Full Baudot send/receive (45.45 baud)
* Text‑only TTY calls
* Auto‑detection of incoming TTY tones (from relay centers or other devices)
* VCO/HCO hybrid modes:

  * **VCO**: User speaks, receives TTY text
  * **HCO**: User types, listens to voice
* TTY text ↔ App UI bridging:

  * Incoming tones → decoded to text
  * Outgoing text → encoded to tones

TTY works in:

* **Talk Mode** (primary for TTY calls)
* **Monitor Mode** (tone detection / passive decode)
* **Data Mode** (optional TTY‑over‑data fallback)

---

# Operating Modes

Uniline is a **multi‑mode node**, but DATA/VOICE/TTY are not separate silos — they share the same line interface.

### **1. DATA MODE — encrypted digital communication**

* Modem DSP establishes link
* Crypto layer secures payloads
* Phone/host exchanges messages via USB‑C or BLE
* Optional TTY‑over‑data fallback

### **2. MONITOR MODE — high‑impedance listening**

* Passive, line not seized
* Safe diagnostics
* Detects:

  * Dial tone
  * Noise level
  * Active modems
  * Alarm dialers
  * TTY tones

### **3. TALK MODE — voice + tty**

* Seizes line (off-hook)
* Smartphone acts as handset
* Used for:

  * Standard voice calls
  * TTY text calls
  * VCO/HCO hybrids
  * DTMF dialing

---

# System Overview

```
PHONE / APP (Android / laptop)
   │ USB‑C / Bluetooth
   ▼
+--------------------------+
|       Uniline node       |
|  modem • tty • buttset   |
+--------------------------+
   │
   ▼
Analog Phone Line (RJ11 / 66 / 110)
   │
   ▼
   PSTN
   │
   ▼
Another Uniline node
```

---

# Architecture

```
PHONE
(Android / laptop)
   │  USB‑C serial + audio
   │  Bluetooth BLE + HFP
   ▼
┌────────────────────────────┐
│         Uniline node        │
├────────────────────────────┤
│ Mode control (Data/Monitor/Talk)
│ Crypto layer (E2EE)
│ Modem + TTY DSP
├────────────────────────────┤
│ Analog audio switch
│ FXO line interface
│ Off‑hook control + isolation
└────────────────────────────┘
   │
   ▼
Analog line / PSTN
```

---

# Why Uniline Matters in Crisis Environments

Uniline is designed as a humanitarian and resilience-focused communication tool. In conflict zones, civil unrest, or infrastructure collapse, it can:

* Support medical aid coordination when cellular and internet services fail
* Provide fallback communication for shelters, clinics, and emergency centers
* Enable family reunification and safety check-ins across regions
* Maintain long-distance contact between communities through surviving PSTN links
* Support NGO and relief operations with simple text, voice, and TTY channels
* Offer degraded-line communication options via TTY when voice/data fail
* Operate during long power outages due to PSTN’s centralized power
* Allow continuity of operations for schools, small local governments, and community hubs

---

# Intended Uses

For **ethical, non‑destructive** use only:

* Disaster response
* Community resilience
* Humanitarian coordination
* Backup communication for shelters/clinics
* Outage conditions (power + internet down)
* Research into PSTN behavior
* Accessibility and text‑call studies

Life‑safety systems (fire panels, emergency circuits) should only be monitored passively unless in legitimate emergencies.

---

# Non‑Goals

Uniline is **not** intended for:

* Unauthorized telecom access
* Unlawful communication
* High‑speed networking
* Any interference with life‑safety systems

---

# Project Philosophy

Uniline is part of **dev://systems** — exploring:

* Universal coherence across infrastructure layers
* Resilient + humane engineering
* Civil benefit through open tools
* Layered communication that survives failure

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
  examples/
    demo-topologies.md
    sample-workflows.md
```

---

# Contributing

Looking for contributors with backgrounds in:

* Telephony/PSTN engineering
* Modem DSP
* Crypto protocol design
* Embedded systems
* Disaster communication
* Accessibility/TTY research

Open an issue before major contributions.

---

# License

This project is licensed under CC0 1.0 Universal (Public Domain Dedication).
You may copy, modify, distribute, and use the work for any purpose without asking permission.
See: https://creativecommons.org/publicdomain/zero/1.0/
