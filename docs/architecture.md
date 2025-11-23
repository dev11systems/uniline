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
```

---

## Host Interface

The host (phone or laptop) connects via:

* **USB-C**:

  * serial or CDC interface for data control/messages
  * USB Audio Class (UAC) for voice/TTY audio
* **Bluetooth** (optional):

  * BLE for control and data
  * HFP for voice-band audio

The host runs a client app that can:

* send/receive encrypted messages
* drive TTY text calls
* act as a handset for voice calls
* display line and mode status

---

## Line Interface

The line interface provides:

* galvanic isolation
* surge and overvoltage protection
* on-hook / off-hook control (FXO behavior)
* line-voltage and line-current sensing
* audio coupling for modem/voice/TTY signals

It should be designed to:

* respect regulatory constraints in the relevant region
* avoid interfering with life-safety systems
* fail safely if misused

---

## Modem and TTY Layer

The DSP layer is responsible for:

* establishing and maintaining an analog modem link for data mode
* modulating and demodulating TTY Baudot tones
* detecting dial tone and other supervisory tones where appropriate
* reporting line conditions to the host app

Data mode and TTY mode share this layer but use different profiles and framing.

---

## Crypto Layer (Data Mode)

The crypto layer:

* assumes the analog line and PSTN are untrusted
* provides confidentiality and integrity for data messages
* may use pre-shared keys, QR-encoded secrets, or other schemes
  (TBD in `docs/protocol.md`)

TTY and voice may be unencrypted or separately secured at the application level; the base design treats them as plain audio paths.

---

## Operating Modes

Uniline uses three main modes defined by physical line interaction:

* **Data mode**

  * off-hook as needed to establish a modem link
  * DSP dedicated to digital carrier
  * host app presents a message/telemetry UI

* **Monitor mode**

  * high-impedance, on-hook listening
  * does not seize the line
  * used for diagnostics and TTY/modem detection

* **Talk mode**

  * off-hook, audio path active
  * host provides mic/speaker (voice handset)
  * can send/receive TTY tones and DTMF

Mode switching is controlled by the host app and enforced in firmware to prevent conflicting states.
