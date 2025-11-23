# Uniline Architecture

This document describes the main components of a Uniline node and how they interact.

---

## Block Diagram

```text
PHONE / HOST
(Android / laptop)
   │  USB-C (WebSerial +  USB Audio)
   │  Bluetooth (BLE control +  HFP audio)
   ▼
┌───────────────────────────────┐
│          Uniline Node         │
├───────────────────────────────┤
│ Mode Manager (Monitor/Talk/Data)
│ DSP Layer (Modem + TTY + Tone Detectors)
│ Protocol Engine (WebSerial/WebUSB framing)
│ Crypto Layer (E2EE for Data Mode)        
├───────────────────────────────┤
│ Audio Router (Voice/TTY/Modem paths)
│ FXO Line Interface (Isolation + Sensing)
│ Off-Hook Control & Protection Stack
└───────────────────────────────┘
   │
   ▼
Analog Line / PSTN
```

---

## Host Interface

The host uses **browser-based software** instead of native apps. It connects through:

### **USB‑C (primary)**

* WebSerial for command/data channel
* Optional USB Audio Class for voice/TTY audio
* Fully offline, lowest latency

### **Bluetooth (fallback)**

* BLE for control & data (low-bandwidth)
* HFP for voice-band audio
* Used when USB is unavailable

### Host responsibilities

The browser-based Web GUI provides:

* Encrypted messaging UI
* TTY call UI (Baudot, VCO/HCO)
* Voice handset (via mic/speaker)
* Mode selection & status display
* Line diagnostics (Monitor mode)

No native apps required—entire host stack runs in the Web GUI.

---

## Line Interface (FXO)

The FXO subsystem provides:

* Transformer or solid‑state **galvanic isolation**
* Surge protection (MOV + TVS stack)
* **On-hook / off-hook** control
* Ring detection
* Voltage & current sensing
* Safe high‑impedance monitor path

It must:

* Respect telco electrical standards
* Avoid false off‑hook conditions
* Fail safely
* Protect both host and PSTN

This subsystem maps directly to the hardware docs:

* `hardware/fxo-interface.md`
* `hardware/isolation-and-safety.md`
* `hardware/line-monitor.md`

---

## DSP Layer (Modem + TTY)

A unified DSP layer supports all analog signaling:

### **For Data Mode**

* FSK/PSK modem
* Carrier detect
* SNR + BER estimation
* Framing & error stats to host

### **For TTY**

* Baudot encode/decode
* Tone presence detection
* Support for TTY, VCO, HCO hybrids

### **For Monitor Mode**

* Dial tone detection
* Modem carrier detect
* TTY tone recognition
* Noise floor measurement

The DSP architecture mirrors:

* `firmware/endpoint/dsp/README.md`

---

## Protocol Layer (Host ↔ Node)

Communication with the Web GUI uses a binary protocol over:

* **WebSerial** (USB‑C)
* **WebUSB** ()
* **BLE** (slow fallback)

Protocol features:

* Consistent framing
* CRC‑16 integrity
* Commands (SET_MODE, CALL, SEND_DATA, etc.)
* Async events (EVT_MODE, EVT_TONE, EVT_STATUS)

Defined in:

* `firmware/endpoint/protocol/README.md`

---

## Crypto Layer (Data Mode)

Because PSTN is untrusted, Data Mode uses:

* E2EE encryption
* Optional pre-shared keys
* QR‑scannable secrets
* Ephemeral session keys (TBD)

Crypto applies **after** modem framing, making the analog side simple.

Voice and TTY paths remain plain audio unless application-level encryption is layered on top.

---

## Operating Modes

Uniline supports three unified modes, each enforced by firmware safety rules.

### **1. Monitor Mode**

* High‑impedance
* Passive listen only
* Line state + tone detection
* Safe default

### **2. Talk Mode**

* Off‑hook
* Voice path active (mic/speaker)
* TTY decode/encode
* DTMF dialing

### **3. Data Mode**

* Off‑hook modem link
* Encrypted data exchange
* Link quality stats

Modes reference:

* `firmware/endpoint/modes/README.md`

---

## How This Fits Into the Repo Structure

```
/docs/architecture.md      ← this document (system overview)
/hardware/                 ← physical/electrical implementation
/firmware/endpoint/        ← embedded logic, DSP, protocol, modes
/web/                      ← browser-based GUI (no native apps)
```

---

## Status

Architecture updated to match the new hardware and firmware documentation. May evolve as prototypes develop.

---

## License

CC0 1.0 (Public Domain).
