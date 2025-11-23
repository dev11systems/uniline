# Uniline Web GUI

The Uniline Web GUI is the **primary user interface** for operating a Uniline node. It runs entirely in the browser and is designed to work:

- offline
- without cell or Wi-Fi
- on phones, tablets, and laptops

All you need is:

- a Uniline node
- a USB-C cable (preferred) or Bluetooth
- a modern browser with WebSerial/WebUSB/WebBluetooth support

---

## Capabilities

The Web GUI provides:

### 1. Mode Control

- switch between **Data**, **Monitor**, and **Talk** modes
- see current line state:
  - on-hook / off-hook
  - dial tone present / absent
  - ringing / idle
  - link quality indicators

### 2. Secure Messaging (Data Mode)

- send and receive encrypted text messages
- view message logs and delivery state
- show basic link stats (throughput, retries, error rate)
- support small telemetry/status messages

### 3. TTY Text Support

- live TTY (Baudot) decode and display
- type-to-TTY input
- auto-detection of incoming TTY tones
- hybrid modes such as VCO/HCO
- optional “TTY-over-data” fallback for very noisy lines

### 4. Voice / Talk Mode

- use the device mic/speaker or headset
- call controls (dialpad, hangup, timer)
- visual status for audio path and line state

### 5. Monitor / Diagnostics

- high-impedance Monitor mode
- noise level visualization
- dial tone / ring detection
- TTY/modem tone detection
- quick indication of whether a line is usable

---

## Architecture

Two main deployment patterns:

### A. Served from the Uniline Node

The Uniline device runs a tiny HTTP server:

```text
Browser (phone/laptop)
   │  USB-NIC / Wi-Fi AP
   ▼
Uniline Web Server
   │
   ▼
Device Control + Line Interface
