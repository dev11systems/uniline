# Uniline Architecture

This document describes the main components of a Uniline node and how they interact.

---

## Block Diagram

```text
PHONE / HOST
(Android / laptop)
   │  USB-C (serial + audio)
   │  Bluetooth (BLE + HFP)
   ▼
┌────────────────────────────┐
│        Uniline Node        │
├────────────────────────────┤
│ Mode Control (Data/Monitor/Talk)
│ Crypto Layer (E2EE for data)
│ Modem + TTY DSP
├────────────────────────────┤
│ Analog Audio Switch
│ FXO Line Interface
│ Off-Hook Control & Line Sensing
└────────────────────────────┘
   │
   ▼
Analog Line / PSTN
