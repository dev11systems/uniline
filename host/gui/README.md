# Uniline Host GUI

The graphical desktop application provides a user-friendly interface for operating a Uniline node. It is ideal for:

* field responders using laptops
* clinic/shelter operators
* non-technical users
* demonstrations and training

The GUI mirrors all core Uniline functionality while offering visual clarity and guided workflows.

---

## Features

### **1. Mode Management**

Switch between:

* **Data Mode** (encrypted messages)
* **Monitor Mode** (high-impedance diagnostics)
* **Talk Mode** (voice + TTY)

Each mode displays its own status indicators and context-specific tools.

---

### **2. Messaging Interface (Data Mode)**

A simple chat-style panel for:

* sending/receiving encrypted text
* viewing delivery confirmations
* automatic message logs
* optional telemetry or location updates

Messages are queued and retried automatically on unstable links.

---

### **3. TTY Interface**

TTY is fully integrated:

* live TTY encode/decode view
* chat-like transcription of Baudot text
* automatic detection of incoming TTY tones
* support for:

  * direct TTY calls
  * VCO/HCO modes
  * TTY-over-data fallback

---

### **4. Voice Call Support**

The GUI connects to the system’s audio devices (USB or Bluetooth headset).

Includes:

* call timer
* volume control
* simplified dialpad for DTMF

---

### **5. Diagnostics Panel (Monitor Mode)**

Displays:

* line voltage/current
* dial tone detection
* ringing events
* noise floor measurement
* TTY/modem tone detection

Useful for responders determining which lines are live.

---

### **6. Device Status Bar**

Always visible:

* connection status (USB/BLE)
* current mode
* line state (on-hook/off-hook)
* link quality indicators

---

## Architecture

The GUI communicates with the Uniline device via:

* USB-C serial
* USB audio (voice/TTY)
* optional BLE for control

```
GUI (Electron / Qt / GTK)
   │ USB/BLE
   ▼
Uniline Node
   │
   ▼
Analog Line / PSTN
```

---

## Technology Options (TBD)

Possible implementation stacks:

* **Electron** (cross-platform, easiest to ship)
* **Qt** (native performance)
* **GTK** (Linux-first)

The choice will depend on contributor skill and long-term maintainability.

---

## Status

Design stage. Interaction flows and mockups pending.

---

## License

CC0 1.0 (Public Domain).
