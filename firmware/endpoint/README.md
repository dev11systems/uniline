# Uniline Firmware (Endpoint)

> The firmware powering modem, TTY, line-state detection, mode switching, and WebSerial/WebUSB communication.

This directory contains the embedded firmware that runs on the Uniline hardware. It manages all real-time functions of the device, including modem DSP, TTY encoding/decoding, line interface control, and the USB/BLE communication layer used by the Web GUI.

The firmware is designed to be **portable**, **low-power**, and **modular**, with clear separation between hardware drivers, DSP, protocol handling, and host communication.

---

# 1. Goals

The Uniline firmware aims to:

* Provide a **stable, isolated interface** to analog phone lines
* Support **Data**, **Talk**, and **Monitor** modes
* Implement **low-bandwidth analog modem DSP**
* Implement **full TTY (Baudot) support**
* Expose a **WebSerial/WebUSB API** to the browser-based GUI
* Enforce **safety checks** before off-hook transitions
* Handle **crypto handshakes** (once implemented) above the modem layer
* Maintain predictable behavior under low-SNR conditions

---

# 2. Firmware Architecture

```
┌─────────────────────────────────────────────┐
│                 Uniline MCU                 │
├─────────────────────────────────────────────┤
│  Mode Controller (Data / Monitor / Talk)    │
│  Safety Manager (Voltage / Surge / Watchdog)│
│  Line Interface Driver (FXO, relay, sense)  │
│  Audio Engine (ADC/DAC, filters, AGC)       │
│  Modem DSP (FSK / PSK low-speed modems)     │
│  TTY Engine (Baudot encoder/decoder)        │
│  Tone Detector (DTMF, dialtone, ringback)   │
│  WebSerial / WebUSB Protocol Layer          │
│  BLE Control Interface (optional)           │
└─────────────────────────────────────────────┘
```

Each subsystem is isolated but coordinated through the **Mode Controller**.

---

# 3. Modes Implemented by Firmware

### **Monitor Mode**

Passive, high-impedance monitoring.

* Reads audio via isolated transformer
* Detects tones (TTY, modem, dialtone)
* Provides noise/SNR metrics to the host
* Absolutely no line seizure permitted

### **Talk Mode**

Voice or TTY calls.

* Engages FXO off-hook
* Routes audio bi-directionally
* Allows TTY Baudot encoding/decoding
* Supports VCO/HCO hybrid modes

### **Data Mode**

Encrypted digital communication via modem.

* Negotiates modem handshake (FSK → PSK)
* Exchanges packets with crypto layer (external)
* Sends link quality reports
* Handles retries and fallback rates

---

# 4. DSP Components

### **4.1 Modem DSP**

Support for:

* Bell 103 (300 bps)
* V.21 (300 bps)
* V.23 (1200/75 bps)
* Optional: 2400–9600 bps QAM/PSK (future)

Functions:

* Bandpass filtering
* Frequency/phase detection
* Soft-decision decoding
* Symbol timing recovery

### **4.2 TTY Engine**

Full Baudot implementation:

* 45.45 baud
* 1400/1800 Hz shift frequencies
* LTRS/FIGS state machine
* Burst shaping & amplitude control
* Error-tolerant decoding

TTY is available in **Talk** and **Monitor** modes.

---

# 5. Safety Enforcement

Before switching modes, firmware must validate:

* Line voltage within expected bounds
* No surge or GDT/MOV trigger events
* Relay not stuck
* Audio path stable
* MCU not in fault state

If safety fails → **forced return to on-hook monitor state**.

Watchdog resets prevent unsafe lockups.

---

# 6. Host Communication

Uniline exposes a simple control/data API via:

### **WebSerial (Primary)**

* Full-duplex command channel
* High-priority audio modulation path

### **WebUSB (Optional)**

* Native USB endpoints
* Fewer driver issues on mobile devices

### **BLE (Fallback)**

* Control only (low bandwidth)
* No audio or DSP data

API specification located in:

```
web/js/uniline-api.js
```

---

# 7. Firmware Directory Layout

```
firmware/
  endpoint/
    README.md        ← this file
    drivers/         ← FXO, ADC, DAC, GPIO, timers
    dsp/             ← modem + TTY processing
    protocol/        ← host communication
    modes/           ← Monitor, Talk, Data logic
    utils/           ← helpers, CRC, buffers
```

---

# 8. Development Environment

Recommended MCU families:

* ESP32-S3 (USB + DSP friendly)
* RP2040 (USB + PIO for DSP timing)
* STM32F4/F7 (strong ADC/DAC performance)

Toolchain examples:

* PlatformIO
* ESP-IDF
* Pico SDK
* STM32CubeIDE

---

# 9. Status

Firmware is in early architectural design. DSP components and WebSerial API definitions are being drafted.

---

# License

All firmware is released under **CC0 1.0 (Public Domain)**. You may use, modify, or redistribute freely.
