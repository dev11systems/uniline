# Uniline Documentation

This directory contains the high‑level documentation for Uniline. It ties together the **hardware**, **firmware**, and **web host interface**, and provides conceptual guidance, architecture overviews, threat models, and practical examples.

This README has been updated to reflect the newly added hardware + firmware systems and to provide clearer navigation.

---

## Purpose of `/docs`

The `/docs` section is the **human-readable, system-level explanation** of Uniline. It does **not** contain source code or low-level implementation; instead it explains:

* what Uniline does
* why it exists
* how it works as a full system
* what problems it solves
* how the pieces fit together
* how to use it in practice

This makes `/docs` the conceptual layer above:

* `/hardware` (electrical + mechanical design)
* `/firmware` (embedded logic + DSP + protocol)
* `/web` (host GUI + API bindings)

---

# Document Index

Below is an indexed description of every document in this directory.

---

## **1. overview.md — System‑Level Summary**

High-level introduction to Uniline:

* core capabilities (Data, Talk, TTY)
* disaster & resilience use cases
* why analog lines are worth leveraging
* how Uniline integrates hardware + firmware + web

Useful for newcomers and for orienting readers.

---

## **2. architecture.md — Full End‑to‑End Architecture**

Shows how each subsystem connects:

* analog line interface (FXO)
* audio/DSP processing
* modem/TTY layers
* host protocol (WebSerial/WebUSB)
* Web GUI layers
* safety boundaries & isolation

Now updated to reference:

* the modular hardware documents
* firmware mode FSM
* protocol message structure

---

## **3. protocol.md — Host ↔ Node Communication**

Explains the binary protocol:

* command frames
* response frames
* async events
* CRC
* framing rules
* example transactions

Updated to align with the new `/firmware/endpoint/protocol` spec.

---

## **4. threat-model.md — Safety & Risk Framework**

Covers:

* analog line hazards
* misuse risks
* safety requirements
* cryptographic threat considerations
* PSTN uncertainty
* design assumptions and non-goals

Cross-linked to:

* hardware isolation requirements
* firmware safety checks
* Web GUI confirmation rules

---

## **5. use-cases.md — Intended Ethical Uses**

Describes real-world scenarios:

* disaster response
* medical coordination
* NGO operations
* text-first accessible communication
* offline field comms

Updated to integrate:

* TTY fallback behavior
* Web GUI offline-first flows
* Data/Monitor/Talk relationships

---

## **6. demo-topologies.md — Example Networks**

Visual examples such as:

* two-node point-to-point modem link
* clinic hub ↔ remote shelter
* multi-city PSTN hopping
* talk/tty mixed branches

Updated to reflect:

* new firmware constraints
* real-world PSTN line behavior

---

## **7. sample-workflows.md — Practical Step-by-Step Guides**

Includes walk-throughs for:

* verifying a line
* establishing voice or TTY contact
* establishing a data link
* switching modes from the Web GUI

Updated to reflect:

* WebSerial-based UX
* the new DSP mode behaviors

---

# How `/docs` Maps to the Rest of the Repo

```
/docs             ← human-facing explanations
  │
  ├── maps to →   /hardware
  │                 FXO, audio path, power, isolation, enclosure
  │
  ├── maps to →   /firmware
  │                 DSP, modes, protocol, drivers, utilities
  │
  └── maps to →   /web
                    Web GUI, JS APIs, offline operation
```

`/docs` is the layer that explains **why** the other directories look the way they do.

---

# Status

The documentation is aligned with the latest hardware + firmware modules. Additional diagrams may be added once prototypes stabilize.

---

# License

All documentation is released under **CC0 1.0 (Public Domain)**.
