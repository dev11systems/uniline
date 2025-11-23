# Audio Path

The audio path is the heart of Uniline’s VOICE, MODEM, and TTY functionality. It manages the conditioned analog audio flowing between the FXO line interface and the DSP/MCU subsystem, while allowing the host device (phone or laptop) to send and receive audio through USB-C or Bluetooth.

This document outlines the recommended audio topology, filtering, switching, and design considerations.

---

# Goals

The audio subsystem must:

* Support **voice (300–3400 Hz)**
* Pass **modem tones** (V.21 → V.34 range)
* Cleanly handle **TTY Baudot tones** (1400/1800 Hz)
* Provide **monitor mode** (read-only)
* Provide **talk/data mode** (active send/receive)
* Avoid distortion or clipping across the entire band
* Seamlessly switch between modes with minimal audible artifacts
* Interface with **WebAudio** on the host

---

# Audio Path Overview

```
 Analog Line
    │
    ▼
[ FXO + Isolation ]
    │
    ├──► High-Impedance Monitor Path
    │
    └──► Off-Hook Audio Path
             │
             ▼
       Audio Conditioner
             │
             ▼
       MCU / DSP Codec
             │
             ▼
     USB-C / Bluetooth Audio
```

---

# Components and Stages

## 1. **Coupling + Isolation**

Audio enters the subsystem after FXO isolation.

Requirements:

* AC coupling capacitors sized for good low-frequency response
* Maintain transformer linearity
* Avoid DC offsets into codec

Capacitor sizing target:

* 1–10 µF for transformer-based coupling

---

## 2. **Audio Conditioning**

Functions:

* Impedance matching to codec
* Level scaling
* Anti-alias filtering

Typical design:

* Simple op-amp stage (e.g., TLV900x, LMV321)
* Gain: unity or slight boost depending on transformer output
* Low-pass filter at ~4 kHz

TTY and modem tones require minimal filtering to avoid distortion.

---

## 3. **Switching (Mode Control)**

The audio path must change depending on the selected Uniline mode.

### Monitor Mode

* Audio tapped via high-impedance sense
* No TX audio back into line
* MCU receives RX-only audio

### Talk Mode

* Two-way audio to/from line
* MCU handles encoding/decoding

### Data Mode

* MCU generates/receives modem audio
* Full duplex optional; half duplex acceptable for low complexity

Switching technologies:

* Analog multiplexers (e.g., TS5A3359)
* Micro-relays (slower, but acceptable for early prototypes)

---

## 4. **Codec / ADC-DAC Stage**

The MCU or external audio codec converts between analog and digital audio.

Requirements:

* 8 kHz sampling minimum (voice)
* 16 kHz preferred for clean TTY and modem work
* 12–16 bit resolution
* Low latency (<10 ms ideal)

Options:

* ESP32 internal ADC/DAC (usable but noisy)
* I2S codec (recommended) such as:

  * PCM1808 (ADC)
  * CS4272 or WM8731 (full codec)

---

## 5. **Host Audio Interface**

Uniline transmits audio to the host device in two ways:

### A. USB-C (WebAudio + WebSerial)

* MCU streams PCM frames via USB
* Browser processes audio in WebAudio
* Supports:

  * Real-time voice calls
  * TTY tone detection
  * Modem tone generation

### B. Bluetooth (HFP Profile)

* Alternative for wireless handsets
* Lower bandwidth than USB, but adequate for voice
* Not recommended for modem use

---

# Frequency Response Targets

To support all modes:

* **Low end**: down to ~200 Hz
* **Upper end**: at least 3500 Hz
* **Flatness**: ±2 dB in the voice band
* **TTY tones**: preserve 1400/1800 Hz without distortion

---

# Gain Structure

Recommended approach:

* Keep the FXO transformer output near unity
* Apply small gain at the conditioning stage if needed
* Implement AGC (automatic gain control) in software when possible

Avoid clipping:

* Telephone lines can vary widely in amplitude
* DSP should detect and auto-limit peaks

---

# Noise Considerations

Key contributors:

* Transformer hum
* MCU ground coupling
* USB noise
* Bluetooth RF

Mitigations:

* Star grounding
* Shield audio traces
* Isolate digital and analog planes
* Use ferrite beads on analog lines

---

# TTY-Specific Considerations

TTY (Baudot) tones are extremely sensitive to distortions.

Ensure:

* No aggressive filtering
* No heavy AGC
* Maintain consistent tone spacing
* Avoid phase shifts around 1400–2000 Hz

TTY decoding can occur:

* On-device (firmware)
* In-browser via WebAudio (future)

---

# Modem-Specific Considerations

Modem tones (V.21/V.22/V.32/V.34) require:

* Very low distortion
* Minimal phase rotation
* Clean pass-through across the band

Higher-speed modems rely more on:

* Line quality
* Isolation transformer response
* Software DSP quality

For early versions, target:

* **V.21 (300 bps)**
* **V.22 (1200 bps)**
* **V.32bis (~14.4 kbps)**

Higher speeds may be attempted later.

---

# Status

Audio path design is in early exploration. Real-world testing will determine the best transformer, switching hardware, and codec configuration.

---

# License

Released under **CC0 1.0 Universal** (Public Domain).
