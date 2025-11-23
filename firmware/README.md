# Uniline Firmware

The Uniline firmware powers the embedded node that interfaces between the analog phone line (FXO) and the host device (browser-based Web GUI, CLI, or other tools). It manages mode control, DSP (modem + TTY), protocol communication, safety logic, and low-level drivers.

The firmware is designed to be:

* **Simple** — minimal layers, predictable behavior
* **Deterministic** — mode logic and DSP pipelines are state-machine driven
* **Safe** — defaults to on-hook Monitor mode, isolation-first
* **Portable** — hardware-agnostic utilities, modular drivers
* **Web-friendly** — integrates cleanly with WebSerial/WebUSB
* **Resilience-focused** — prioritizes reliability over throughput

---

## Architecture Overview

The firmware stack is composed of several layers:

```
+------------------------------+
|   Host Interface (Protocol)  |
+------------------------------+
|   Mode Manager (Monitor /    |
|   Talk / Data FSM)           |
+------------------------------+
|   DSP Layer (Modem / TTY)    |
+------------------------------+
|   Drivers (FXO / ADC / DAC / |
|   GPIO / Timers / Audio)     |
+------------------------------+
|   Utils (CRC / Ringbuf /     |
|   Framing / Logging)         |
+------------------------------+
```

Each layer is documented in more detail in its own subdirectory.

---

## Key Responsibilities

The firmware handles the following major responsibilities:

### **1. Mode Control**

Implements the logic for:

* **Monitor mode** — high-impedance listening
* **Talk mode** — voice + TTY with off-hook control
* **Data mode** — modem-based low-bitrate communications

All transitions enforce **safety checks** before the FXO is taken off-hook.

### **2. DSP Processing**

* Analog modem DSP (FSK/PSK)
* Baudot TTY encode/decode
* Tone detection (dial tone, modem carrier, TTY, DTMF)
* Audio routing for voice functionality

### **3. Protocol Interface**

A binary WebSerial/WebUSB protocol connects the embedded node to:

* The Web GUI
* CLI tools
* Automated tests

Responsibilities include framing, CRC validation, event emission, and command execution.

### **4. Drivers**

Low-level access to:

* FXO interface (on/off hook, ring detect, voltage)
* ADC/DAC for audio
* GPIO for mode switches and monitoring
* Timers for DSP tick scheduling

### **5. Utilities**

Shared helpers used across multiple firmware layers:

* CRC-16
* Ring buffers
* Frame builder/parser
* Logging (configurable)
* Optional fixed-point helpers

---

## Directory Structure

```
firmware/
  README.md                    ← this file
  endpoint/
    drivers/                   ← FXO, ADC/DAC, GPIO, timers
    dsp/                       ← modem + TTY DSP pipelines
    protocol/                  ← binary host protocol
    modes/                     ← state machines for Monitor/Talk/Data
    utils/                     ← buffers, CRC, framing, logging
```

Each subdirectory contains its own detailed README.

---

## Development Philosophy

The firmware is built to maximize **robustness**, **clarity**, and **survivability** under degraded conditions. Design choices prioritize:

* Predictability over performance
* Safety over convenience
* Minimal dependencies
* Deterministic state behavior

This makes it suitable for field use, disaster scenarios, and long-term maintenance.

---

## Hardware Targets

The firmware is MCU-agnostic but expecting:

* ARM Cortex-M (M0+/M3/M4)
* ESP32-S3 (if isolation path is external)
* Atmel SAMD (optional)

Actual MCU choice depends on prototype performance and analog-interface needs.

---

## Status

Early development. DSP specifications, safety logic, and protocol framing are stable at the conceptual level. Hardware bring-up will drive the next stage.

---

## License

All firmware code and documentation are released under **CC0 1.0 (Public Domain)**.
