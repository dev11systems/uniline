# Uniline Hardware: Power System

> A fully isolated, low-power architecture designed for disaster resilience, device safety, and offline operation.

Uniline’s power system is intentionally simple, efficient, and robust. It must operate reliably in unstable conditions and protect both the device and any connected smartphone, laptop, or host computer.

---

# 1. Power Goals

Uniline’s power subsystem is designed to:

* Operate **from USB-C** or **internal battery** (optional module)
* Provide **complete galvanic isolation** between PSTN and USB domains
* Sustain long runtimes with minimal consumption
* Avoid loading or drawing power from the PSTN line
* Protect against surges, transients, and ESD
* Remain stable during modem/TTY transmission bursts

---

# 2. Power Sources

Uniline can be powered from:

### **1. USB-C (primary)**

* Standard 5V power
* Negotiated via USB-C pull-up resistors (no PD required)
* Compatible with smartphones, laptops, or power banks

### **2. Li-Ion / LiPo Battery (optional)**

* 3.7V nominal
* Uses isolated DC/DC to protect PSTN
* USB-C charging module (TP4056-class or integrated PMIC)

### **3. Auxiliary 5V (rare / debug use)**

* For bench testing or embedded integration

**Uniline never draws power from the telephone line** — this keeps the design legal, safe, and compatible with emergency services infrastructure.

---

# 3. Power Architecture Overview

```
USB-C 5V In
   │
   ▼
[ Input Protection ]
   │   (ESD diodes, surge clamps)
   ▼
[ 5V Regulator / Battery Manager ]
   │
   ▼
[ Isolated DC/DC Converter ]  ← galvanic isolation barrier
   │       (5V → 5V or 5V → 3.3V)
   ▼
MCU + DSP Domain
   │
   ▼
Analog Audio + DAA System
```

Isolation module is critical. It:

* Prevents PSTN voltages from reaching the USB device
* Keeps ground domains separate
* Ensures safety under fault conditions

---

# 4. Isolation Converter

Uniline uses a **reinforced isolation DC/DC module**:

* Typical: 1W–2W isolated converter
* Voltage domains: 5V → 5V (galvanic isolation) or 5V → 3.3V
* Isolation rating: **1kV–3kV**
* Low noise variant required for analog audio stability

Requirements:

* Input filtering capacitor bank (22–47 µF)
* Output LC filter for noise reduction
* Ferrite bead to protect MCU domain

---

# 5. Voltage Regulation

The internal logic domain supplies:

* **3.3V** for MCU, DSP, BLE module
* **5V** for analog path switching and relays

Regulation stack:

1. USB-C 5V → Isolation module
2. Isolation module → 5V isolated
3. 5V isolated → 3.3V LDO for MCU

The LDO must.

* Provide good PSRR (power supply rejection)
* Handle audio-band noise gracefully

---

# 6. Power Consumption Targets

Uniline is designed for **low-power operation**:

| Mode                | Estimated Draw |
| ------------------- | -------------- |
| Idle (connected)    | 20–30 mA       |
| Monitor mode        | 25–40 mA       |
| Talk/Voice          | 40–80 mA       |
| Data/Modem (active) | 50–120 mA      |
| BLE-only mode       | 10–20 mA       |

An optional **1000 mAh battery** can power Uniline for multiple hours of continuous operation.

---

# 7. Protection Components

The power subsystem includes:

### **1. ESD protection**

* USB port protected by TVS arrays
* Required for field operation

### **2. Over-current protection**

* PTC or dedicated power-switch IC

### **3. Reverse polarity protection**

* MOSFET-based reverse protection

### **4. Surge suppression**

* Minor USB surges absorbed safely

### **5. Battery safety (optional)**

* Overcharge & overdischarge protection
* Thermal cutoff
* Short-circuit protection

---

# 8. USB-C Behavior

Uniline uses **USB-C basic mode**, not Power Delivery:

* CC resistors advertise 5V/Default USB current
* No PD negotiation needed
* Ensures compatibility with:

  * iPhones (USB-C)
  * Android phones
  * Laptops
  * Power banks

This is simpler, more robust, and consumes less silicon.

---

# 9. Battery Module (Optional)

If included, the battery module offers:

* 3.7V Li-Ion/LiPo
* Charge via USB-C
* 5V boost converter *before* isolation stage
* Reinforced isolation maintained at all times

Rationale: **Battery must never share ground with PSTN**.

---

# 10. Low-Noise Audio Requirements

Power noise affects modem and TTY reliability.

Noise mitigation:

* LC filters on converter output
* Ferrite beads per analog section
* Star-grounding inside isolated domain
* Shielding where appropriate
* Proper analog/digital ground partitioning

Goal: clean 300 Hz–3400 Hz audio band.

---

# 11. Power State Logic

Firmware tracks:

* USB present
* Battery level
* Mode draw estimation
* Overcurrent events
* Isolation converter fault states

Automatic behaviors:

* Low-battery mode reduces DSP usage
* Line-monitor mode allowed even on minimal power
* Data/Talk mode disabled if voltage unstable

---

# 12. Summary

Uniline’s power architecture provides:

* Safe, fully isolated power delivery
* Clean supply for DSP and analog circuits
* Optional battery for extended offline operation
* Robust USB-C compatibility
* Surge/ESD protection for rugged real-world scenarios

This design ensures Uniline remains operational in emergencies while maintaining strong electrical safety and reliability.
