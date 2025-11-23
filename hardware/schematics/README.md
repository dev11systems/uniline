# Schematics

This directory holds all schematics, diagrams, and layout files for the Uniline hardware.

---

## Purpose

The goal of this directory is to collect:

* **electrical schematics** showing the full Uniline circuit
* **block diagrams** of the FXO interface, MCU, audio codec, and power subsystem
* **KiCad source files** for anyone to modify or build their own hardware
* **PDF exports** for quick viewing

Schematics should be readable, well-labeled, and annotated with version numbers.

---

## Contents (Planned)

```
schematics/
  README.md        ← this file
  uniline-main.sch (KiCad)
  uniline-main.pdf
  fxo-interface.sch
  power-subsystem.sch
  audio-codec.sch
  mcu-core.sch
```

Over time, this may expand to include:

* variations of the line interface
* alternative MCU designs
* simplified educational schematics
* wiring diagrams for prototyping and breadboards

---

## Contributing

If submitting schematics:

* include both the **source file (KiCad preferred)** and a **PDF export**
* keep naming consistent
* add notes for any part that requires special sourcing or safety attention

---

## License

All schematics and hardware documentation are released under **CC0 1.0** (Public Domain).
