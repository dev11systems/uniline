# Uniline Host Mobile App

The mobile app is the primary interface for everyday Uniline use. It is designed for:

* field responders using Android devices
* off-grid or low-resource scenarios
* situations where voice, TTY, or text must work without cell or Wi-Fi
* rapid deployment with minimal configuration

The Uniline mobile app aims to be simple, fast, offline-first, and reliable under stressful conditions.

---

## Features

### **1. Automatic Device Discovery**

The app connects to the Uniline node via:

* **USB-C** (preferred — stable, low-latency, supports audio)
* **Bluetooth BLE** (control + data)
* **Bluetooth HFP** (voice audio path)

USB-C requires no pairing and works even in airplane mode.

---

### **2. Mode Switching UI**

Large, single-tap controls for:

* **Data Mode**
* **Monitor Mode**
* **Talk Mode (voice + TTY)**

Each mode displays real-time status:

* line state (on/off-hook)
* signal/noise levels
* presence of dial tone, ringing, or tones

---

### **3. Secure Messaging (Data Mode)**

A lightweight chat UI for:

* encrypted messages
* short status/telemetry
* queued messages when the link is unstable
* message receipts (ACK)

Works entirely offline — the phone only needs USB or BLE.

---

### **4. Integrated TTY Support**

TTY is a first-class feature:

* full Baudot send/receive
* automatic tone detection
* hybrid modes:

  * **VCO** (voice carry over)
  * **HCO** (hearing carry over)
* scrolling transcript window
* TTY-over-data fallback for extremely noisy lines

---

### **5. Voice Calls (Talk Mode)**

Using the phone’s native audio stack:

* voice calls use USB-C digital audio or Bluetooth HFP
* DTMF keypad
* call timer
* inline error cues (noise warnings, clipping, etc.)

---

### **6. Diagnostics Dashboard (Monitor Mode)**

Displays:

* line voltage/current
* dial tone presence
* noise floor
* tone detection (modem / TTY)
* ring cadence detection (if available)

Useful for checking which lines at a site are functional.

---

## Architecture

```
Android App
   │ USB-C / BLE / Bluetooth HFP
   ▼
Uniline Node
   │ Analog Interface
   ▼
PSTN / Remote Node
```

App communicates via:

* USB serial (commands, data frames)
* USB audio (voice/TTY)
* Bluetooth (optional control)

---

## Permissions & Offline Operation

The app is designed to function with:

* Airplane mode ON
* No SIM card
* No Wi-Fi

Permissions required:

* USB host access
* Bluetooth (optional)
* Audio input/output
* Local storage (logs + message queues)

No servers, tracking, analytics, or external dependencies.

---

## Status

Early conceptual stage.
UI mockups and interaction flow diagrams are forthcoming.

---

## License

CC0 1.0 (Public Domain).
