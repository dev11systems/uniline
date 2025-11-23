# Uniline Web GUI

This folder contains the **primary user interface** for Uniline. The Web GUI is a **single-file, offline-capable console** that replaces native and mobile apps by running entirely in the browser using WebSerial, WebUSB, and WebBluetooth.

It is designed for resilience: one file that always works, even in disaster, conflict, or infrastructure-limited environments.

---

## Goals

- Zero native dependencies
- Runs on any device with a modern browser
- Fully offline (no internet, no cell service required)
- Minimal, dev://systems aesthetic
- Unified interface for Data, Monitor, Talk, and TTY modes
- Uses WebAudio + WebSerial for real-time line interaction
- Can be saved locally or served directly from a Uniline device

---

## Structure

Uniline is intentionally shipped as a **single self-contained HTML file**:

```
web/
  README.md   ← overview
  index.html  ← all HTML, CSS, JS inline
```

This ensures:
- no missing assets
- no broken paths
- no dependency loading issues
- compatibility with minimal embedded web servers
- ease of distribution and remixing

A multi-file structure (css/, js/) may optionally be used *during development*, but the primary and recommended distribution remains a single-file console.

---

## Features

### 1. Mode Control
- Switch between **Data**, **Monitor**, and **Talk** modes
- Status indicators for:
  - device connection
  - line state
  - link health

### 2. Secure Messaging (Data Mode)
- Encrypted text exchange
- Delivery receipts
- Message logs
- Basic link statistics

### 3. TTY Support (Integrated)
Uniline includes full TTY functionality directly inside the web console:
- Live Baudot decoding
- Typed TTY output
- Hybrid VCO/HCO support via system audio
- Tone-detection events

TTY is not a separate mode—it's a **capability baked into Talk + Monitor + Data** as needed.

### 4. Voice Support (Talk Mode)
- Uses system mic/speaker or a wired/Bluetooth headset
- DTMF keypad
- Call timer

### 5. Monitor / Diagnostics
- High-impedance passive line monitoring
- Noise-level visualization
- Dial tone + ring detection
- Tone detection (modem + TTY)

---

## How It Connects

The Web GUI communicates with the Uniline device through modern browser APIs.

### **WebSerial (Recommended)**
- USB-C cable
- Fully offline
- Fast and stable

### **WebUSB (Optional)**
- Uses native USB endpoints exposed by the device

### **WebBluetooth (Fallback)**
- Low-bandwidth control channel
- Useful when USB is impossible or unsafe

---

## Loading the Interface

### **1. Served from a Uniline Node**
If Uniline hosts the console directly:
```
http://uniline.local
```

### **2. Local or Static Web App**
Open `index.html` from:
- local storage
- dev://systems
- GitHub Pages
- the Uniline device’s USB storage mode (future option)

Then click **Connect Device** to initialize WebSerial/WebUSB.

---

## Status
- `index.html` contains a functional UI skeleton with inline CSS + JS.
- Mode switching, message logs, TTY view, and diagnostics panels are implemented.
- WebSerial/WebUSB/WebBluetooth device-binding logic is planned next.

---

## License
All Web GUI code is released under **CC0 1.0 (Public Domain)**.
