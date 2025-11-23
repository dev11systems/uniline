# Uniline Hardware: Enclosure & Physical Design

> A compact, rugged, field-ready enclosure built for disaster use, safety, and ease of operation.

The Uniline enclosure is designed to survive chaotic real-world environments—disaster zones, outages, conflict conditions, and everyday field work—while remaining simple, intuitive, and safe for any operator.

---

# 1. Design Goals

The enclosure must be:

* **Rugged** — withstand drops, vibration, moisture spray
* **Compact** — easily pocketable or mountable
* **Non-destructive** — cannot damage PSTN wiring
* **Intuitive** — minimal controls, clear indicators
* **Isolated** — protects users from PSTN voltages
* **Serviceable** — openable for repair or modification
* **Stealth-capable** — optional low-visibility operation

---

# 2. Form Factor

Recommended physical footprint:

* **Dimensions:** ~110 × 70 × 28 mm
* **Weight:** < 200 g
* **Material:** ABS, polycarbonate, or aluminum shell
* **Mounting:** rubber feet + optional belt clip / strap points

These dimensions comfortably house:

* FXO interface
* Isolated DC/DC
* MCU / DSP module
* Analog transformers
* USB-C port
* Internal aux battery (optional)

---

# 3. Exterior Features

### **Front / Top Panel**

* **Mode selector button** (Monitor / Talk / Data)
* **Status LEDs**:

  * Power
  * Mode
  * Line state (on-hook/off-hook)
  * Signal/tone detected
  * Encryption active
* **Multicolor LED bar** for:

  * RX/TX activity
  * Noise level
  * Modem handshake stages

### **Side Panels**

* **USB-C port** (data + power)
* **Aux port** (debug UART or GPIO, optional)
* **Vent cutouts** for thermal dissipation

### **Rear Panel**

* **RJ11 jack** (primary line interface)
* **Universal line clip leads** (66/110 block / bare pair)

  * retractable or detachable

---

# 4. Cable Management

Uniline includes multiple ways to attach to analog lines:

* Standard **RJ11 modular jack**
* **Bed-of-nails clips** for 66 blocks
* **Piercing micro-clips** for drop wires
* **Banana or screw terminals** for direct pair access

All connectors should stow into the enclosure when not in use.

---

# 5. Environmental Hardening

### **Ingress Protection**

* Goal: **IP54** (dust-protected, splash-resistant)
* Silicone gasket between shell halves
* Rubber port covers for USB-C & RJ11

### **Temperature**

* Target operational: **–10°C → 55°C**
* PCB components chosen for extended temp range

### **Impact Resistance**

* Internal rubber standoffs or shock posts
* Reinforced corners
* Optional aluminum internal frame

---

# 6. Internal Layout Principles

```
┌─────────────────────────────┐
│   SHOCK-ABSORBING SHELL     │
├─────────────────────────────┤
│  RJ11   Transformer Zone     │
│          (Analog Section)    │
├─────────────────────────────┤
│   Isolation Barrier (DC/DC)  │
├─────────────────────────────┤
│   MCU + DSP + BLE Module     │
├─────────────────────────────┤
│   Battery (optional)         │
└─────────────────────────────┘
```

Layout rules:

* Analog transformer and FXO circuitry **kept separate** from MCU domain
* Isolation module positioned physically between analog & digital sections
* Battery placed away from heat-producing components
* RF/BLE antenna (if present) placed near outer shell

---

# 7. User Interface Philosophy

The enclosure must support a **minimal cognitive load** in high-stress environments.

Principles:

* **One mode button** cycles between Monitor → Talk → Data
* **LEDs convey state**, not complex displays
* **Clear haptics** when changing modes
* **No touchscreens**, no reliance on fragile interfaces
* **All advanced controls handled through Web GUI**

---

# 8. Optional Stealth / Low-Profile Mode

Some scenarios require low visibility and minimal emissions.

Low-profile mode options:

* Dim all LEDs or disable them automatically
* Disable beeps or haptic feedback
* Avoid off-hook states unless explicitly requested
* BLE-off mode (USB only)

Stealth mode is **for safety, privacy, and survival**, not illicit use.

---

# 9. Thermal Management

The enclosure must dissipate heat from:

* DC/DC isolation module
* Modem/TTY DSP bursts
* Charging circuit (if battery present)

Strategies:

* Internal copper pours as heat spreaders
* Side ventilation slots
* Aluminum backing plate (optional)

Even at peak draw (100–120 mA), heat is manageable.

---

# 10. Assembly & Serviceability

Enclosure should use:

* **Standard Phillips screws** (no glue, no security screws)
* Replaceable internal battery
* Modular cable assemblies
* Open-source 3D-printable models
* Easily sourced off-the-shelf enclosure variant

This keeps Uniline repairable anywhere—even in disaster zones.

---

# 11. Branding & Aesthetics

The design aligns with **dev://systems** minimalism:

* Matte black or charcoal enclosure
* Subtle engraved or silk-screened `Uniline` mark
* Optional dev://systems signature mark
* Clean, industrial, understated visual language

---

# 12. Summary

The enclosure for Uniline is designed to be:

* Robust and protected
* Intuitive under stress
* Safe around PSTN voltages
* Compact and field-ready
* Easy to assemble, repair, and manufacture

By prioritizing resilience, the enclosure becomes a core part of Uniline’s philosophy: **communication that survives when everything else fails**.
