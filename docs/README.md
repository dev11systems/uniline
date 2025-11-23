# Uniline Documentation

This folder contains all technical and conceptual documentation for the Uniline project. Each file focuses on a specific layer of the system. Documents are designed to be modular, readable, and independently improvable.

---

## Contents

### **1. overview.md**

High-level introduction to Uniline:

* What the system is
* Why it exists
* How it fits into resilience communication

### **2. architecture.md**

System architecture:

* Hardware blocks
* Modem/TTY DSP
* Host interfaces (USB-C, Bluetooth)
* Operating modes (Data / Monitor / Talk)

### **3. protocol.md**

Draft data-mode protocol:

* Frame structure
* Message types
* AEAD encryption layer
* Error handling and retransmission

### **4. threat-model.md**

Security boundaries and assumptions:

* In-scope vs out-of-scope threats
* PSTN trust model
* Safety considerations

### **5. use-cases.md**

Where Uniline is useful:

* Disasters
* Community resilience
* Clinics / hubs
* Accessibility / TTY
* Research

### **6. demo-topologies.md**

Example real-world arrangements:

* Clinic → hospital
* Shelter → coordination hub
* Multi-site community network
* Lab testing topologies

### **7. sample-workflows.md**

Practical session examples:

* Secure messaging
* Switching to voice
* TTY calls
* Monitor mode diagnostics
* Store-and-forward workflows

---

## Philosophy

The documentation is intentionally:

* **Modular** — each file stands alone.
* **Clear** — free of jargon whenever possible.
* **Practical** — oriented toward emergencies and real-world conditions.
* **Open** — licensed CC0; remix or extend freely.

---

## Next Steps

Future additions may include:

* Electrical schematics
* Hardware build guide
* Firmware API
* Host app UX documentation

These will live in the appropriate `/hardware` and `/host` subdirectories.

---

All documentation is CC0 / public domain. Enjoy, adapt, extend.
