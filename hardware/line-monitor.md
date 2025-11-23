# Uniline Hardware: Line Monitoring System

> High-impedance, passive, non-intrusive listening for diagnostics, safety checks, and early signal detection.

Uniline’s line-monitoring subsystem allows the device to observe PSTN activity **without seizing the line** or affecting other equipment. This mode is essential for safety, disaster use, and protocol negotiation.

---

# 1. Purpose

Line monitoring enables:

* Checking **line health** before going off-hook
* Detecting **dial tone**, **busy tone**, **ring**, or **dead line** conditions
* Identifying **TTY** or **modem** activity
* Listening for **alarm panel dialers** or **fax/handshaking tones**
* Measuring **noise floor** and interference
* Capturing early audio for auto-mode detection

This is the safest way to determine whether the line is viable.

---

# 2. Requirements

A proper line monitor must:

* Present **1 MΩ or greater** effective impedance
* Pass **audio-band AC** only (no DC path)
* Withstand **ring voltage** and transients
* Provide clean analog for DSP analysis
* Guarantee zero line seizure

---

# 3. Electrical Architecture

Uniline’s line monitor path:

```
PSTN Line
   │
   ▼
[ Surge Protection ]
   │
   ▼
[ Isolation Transformer (Monitor) ]
   │   AC-coupled, no DC path
   ▼
[ High-Value Resistors (1 MΩ+) ]
   │
   ▼
[ Anti-alias filter + gain stage ]
   │
   ▼
[ ADC → DSP → Mode-detection ]
```

Key aspects:

* A dedicated transformer is used for Monitor Mode only.
* Separate from the Talk/Data transformer to prevent accidental off-hook conditions.
* High-value resistors enforce near-zero loading on the line.

---

# 4. Signal Conditioning

Before reaching the ADC, the audio is processed through:

* **RC anti-alias filter** (approx 4–8 kHz cutoff)
* **JFET input buffer** for high impedance
* **Low-gain amplifier** to bring levels into ADC range
* **Automatic gain control (optional)** for loud ring voltage waveforms

This ensures:

* Detectability of tones (TTY, modem, DTMF)
* Accurate SNR measurement
* Stable silence/noise readings

---

# 5. Firmware Responsibilities

The firmware continuously analyzes audio for:

### 5.1 Line Presence

* Detects whether line is connected
* Flags abnormal voltages or surges

### 5.2 Tone Detection

* Dial tone
* Busy signals
* Ringback
* DTMF keys
* Modem carriers (V.21, V.23, Bell 103/212)
* TTY Baudot (1400/1800 Hz shifts)

### 5.3 Noise Analysis

* Measures dBm noise floor
* Tracks spike events
* Determines line stability for data mode

### 5.4 Auto-Mode Recommendations

* Suggests switching to **Talk** when voice is likely
* Suggests **TTY mode** when Baudot detected
* Suggests **Data mode** when modem carrier detected

---

# 6. Safety: Guaranteed Non-Seizure

Monitor Mode is physically unable to seize the line because:

* No off-hook relay path is connected
* Transformer is AC-coupled only
* DC is fully blocked by capacitors
* Resistance is too high to meet off-hook current threshold

This ensures compatibility with:

* Fire alarm panels
* Alarm dialers
* Elevator phones
* Multi-drop wiring
* Shared analog lines

---

# 7. Use Cases

### Disaster / Collapse

* Verify a line still has power
* Check for long-distance call viability
* Detect alarm or fax machines reattempting dial-outs

### PSTN Research

* Study tone cadences across regions
* Analyze noise patterns

### TTY Operations

* Passive detection of incoming Baudot
* Monitor for auto-initiated relay center calls

### Data Mode Preparation

* Ensure line is stable enough before modem negotiation

---

# 8. Summary

Uniline’s line monitor subsystem provides:

* Completely passive, safe line observation
* Full transformer and resistor isolation
* Real-time DSP for tone/noise detection
* Automatic mode suggestions
* Zero risk of off-hook seizure

This monitoring foundation ensures all other modes—Talk, Data, TTY—operate safely and intelligently.
