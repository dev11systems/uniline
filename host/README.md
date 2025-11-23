# Uniline Host Software

The `/host` directory contains all host-side software and interfaces that talk to a Uniline node.

Uniline is designed to be **web-first**:

- the primary interface is a **browser-based Web GUI**
- no native apps are required
- everything runs locally and offline

Other host tools (CLI, native GUI, mobile wrappers) are optional and may reuse the same device-level API.

---

## Host Interfaces

### 1. Web GUI (Primary)

Located in [`host/web`](./web).

- Runs in any modern browser (desktop or mobile)
- Can be served directly from the Uniline node **or** loaded as a static app
- Uses WebSerial/WebUSB/WebBluetooth or a USB-NIC + HTTP to talk to the device
- Provides:
  - mode control (Data / Monitor / Talk)
  - secure messaging and logs
  - TTY view and controls
  - line diagnostics and status

### 2. CLI (Optional)

Located in [`host/cli`](./cli) (conceptual).

- Scriptable interface for development, debugging, and automation
- Talks to the device over the same control protocol
- May be implemented later in Python / Rust / Go

### 3. Native Desktop / Mobile (Optional)

[`host/gui`](./gui) and [`host/mobile`](./mobile) describe possible native wrappers:

- may embed the Web UI
- or reuse the same underlying API as the web client
- considered **nice-to-have**, not required for core Uniline functionality

---

## Web-First Philosophy

The host layer is intentionally:

- **device-agnostic** — anything with a browser can be a console
- **offline-first** — no cloud, no tracking, no external dependencies
- **maintainable** — one primary UI codebase instead of many
- **open** — HTML/JS/CSS and simple APIs that are easy to inspect and extend

---

## Status

- Web GUI: in early design; see [`host/web`](./web)
- CLI / native GUIs: conceptual, not implemented

All host software is released under **CC0 1.0** (Public Domain).
