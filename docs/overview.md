# Uniline Overview

Uniline is a universal analog-line communication node that turns any surviving POTS/copper line into a multi-mode channel for:

- encrypted low-bandwidth data
- TTY text calls
- voice and buttset-style diagnostics
- high-impedance monitoring

It is designed for disaster resilience, accessibility, and civil infrastructure continuity rather than everyday connectivity.

---

## Goals

Uniline aims to:

- provide a **minimal communication channel** when modern networks are unreliable or unavailable
- support **data, voice, and TTY** without requiring separate hardware for each
- work with **phones and laptops** the user already has
- respect **telecom regulations** and **life-safety systems**
- stay **open, remixable, and hardware-agnostic**

---

## Non-Goals

Uniline is **not** intended to:

- be a high-speed networking solution
- evade lawful monitoring or legal processes
- provide covert interception or unauthorized access to telecom infrastructure
- interfere with fire alarm panels or emergency lines

It is a resilience and research tool, not an exploitation tool.

---

## High-Level Concept

At a high level, Uniline:

1. Connects to an analog phone line (RJ11, 66/110 block, or similar).
2. Exposes a USB-C and/or Bluetooth connection to a host (phone or laptop).
3. Provides three operating modes:
   - **Data mode** for encrypted digital messages.
   - **Monitor mode** for high-impedance listening.
   - **Talk mode** for voice and TTY calls.
4. Uses a modem/TTY DSP layer to translate between:
   - analog audio on the line
   - digital messages and text on the host device.
