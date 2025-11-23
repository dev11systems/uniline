# Uniline Web GUI

This folder contains the **primary user interface** for Uniline. The Web GUI replaces native and mobile apps by running fully in the browser using WebSerial, WebUSB, and WebBluetooth, and can function entirely offline.

---

## Goals

* Zero native dependencies
* Works on any device with a modern browser
* Offline-first (no internet or cell required)
* Minimal, dev://systems aesthetic
* Unified interface for Data, Monitor, Talk, and TTY modes
* Leverages WebAudio + WebSerial for real-time line interaction

---

## Structure

```
web/
  README.md        ← this file
  index.html       ← main UI shell
  css/
    style.css      ← theme + layout
  js/
    uniline-api.js ← device API bindings
    ui.js          ← UI logic
```

---

## Features

### 1. Mode Control

* Switch between **Data**, **Monitor**, and **Talk** modes
* Visual status indicators for:

  * device connection
  * line state
  * link health

### 2. Secure Messaging (Data Mode)

* Encrypted text exchange
* Delivery receipts
* Message logs
* Basic link stats

### 3. TTY Support

* Live Baudot decoding
* Typed TTY output
* VCO/HCO hybrid mode
* Tone detection events

### 4. Voice Support (Talk Mode)

* Uses system mic/speaker or headset
* DTMF keypad
* Call timer

### 5. Monitor / Diagnostics

* High-impedance line monitoring
* Noise-level visualization
* Dial tone and ring detection
* Tone detection (TTY / modem)

---

## How It Connects

The Web GUI communicates with the Uniline node using:

### **WebSerial (recommended)**

* USB-C cable required
* Works offline
* Fast and stable data path

### **WebUSB** (optional)

* Device exposes USB endpoints
* Browser communicates natively

### **WebBluetooth** (control-only fallback)

* Low bandwidth
* For situations where USB cannot be used

---

## Loading the Interface

Two approaches are supported:

### **1. Served from the Uniline node**

Visit:

```
http://uniline.local
```

The GUI is hosted by the device itself.

### **2. Static Web App**

Open `index.html` from:

* local filesystem
* dev://systems site
* GitHub Pages

Then click **Connect Device** to initialize WebSerial/WebUSB.

---

## Status

Early but functional skeleton UI exists in `index.html`. JavaScript API and device binding logic to be built next.

---

## License

All Web GUI code is released under **CC0 1.0** (Public Domain).
