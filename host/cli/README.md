# Uniline Host CLI

The Command-Line Interface (CLI) provides a lightweight, scriptable way to interact with a Uniline device. It is ideal for:

* automated workflows
* development and debugging
* low-power laptop environments
* using Uniline without a GUI or mobile app

---

## Capabilities

The CLI exposes the core functionality of Uniline:

### **1. Device Control**

* connect to Uniline over USB-C serial
* query device status
* switch operating modes:

  * Data Mode
  * Monitor Mode
  * Talk Mode (voice/TTY)

### **2. Messaging (Data Mode)**

* send encrypted messages
* receive messages
* log all traffic to file

### **3. TTY Tools**

* encode/decode Baudot text
* send or receive live TTY over the audio path
* run TTY-over-data sessions (if supported)

### **4. Diagnostics**

* view line voltage and current readings
* detect dial tone / ringing
* detect modem or TTY tones
* noise-level analysis

### **5. Development Utilities**

* firmware debug console
* raw frame send/receive
* modem negotiation logs
* link-quality metrics

---

## Example Commands (Draft)

These commands will evolve with the firmware and host protocol.

```
uniline-cli status
uniline-cli mode data
uniline-cli mode talk
uniline-cli mode monitor
uniline-cli send "message text"
uniline-cli recv --log out.txt
uniline-cli tty --live
uniline-cli diag line
uniline-cli diag tones
```

---

## Architecture

```
CLI (Python / Rust / Go)
   │ USB-Serial
   ▼
Uniline Node
   │ Analog Line
   ▼
PSTN / Remote Node
```

The CLI communicates with the Uniline device via a simple serial protocol over USB-C.

---

## Installation (TBD)

Planned installation methods:

* pip / cargo / brew
* packaged binaries for Linux / macOS / Windows

---

## Status

Early-stage. No official release yet.

---

## License

CC0 1.0 (Public Domain).
