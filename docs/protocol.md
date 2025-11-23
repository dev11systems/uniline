# Uniline Protocol (Draft)

This document sketches a draft protocol for data mode. It is intentionally simple and may evolve.

---

## Design Goals

* small, easily implementable on constrained devices
* robust over noisy, low-bandwidth analog links
* message-oriented rather than stream-based
* crypto-friendly (clear separation between framing and payload)

---

## Layers

1. **Physical**: analog line (PSTN / local loop)
2. **Modem**: legacy-like carrier (e.g., V.21/V.22bis class or custom)
3. **Frame**: small packets with IDs, length, checksum
4. **Crypto**: authenticated encryption over frame payload (e.g. AEAD)
5. **Application**: typed messages (text, telemetry, control)

---

## Frame Format (Conceptual)

```text
+---------+---------+----------+-------------+---------+
|  SOF    |  TYPE   |  LENGTH  |  PAYLOAD    |  CRC    |
+---------+---------+----------+-------------+---------+

SOF    : start-of-frame marker (1 byte)
TYPE   : message type (1 byte)
LENGTH : payload length in bytes (2 bytes)
PAYLOAD: encrypted or plain data (0- N bytes)
CRC    : frame checksum (2 bytes)
```

* All multi-byte values use network byte order.
* SOF should be a value unlikely to appear in noise (e.g. 0x7E or similar).
* CRC can be CRC-16-CCITT or similar.

---

## Message Types (Examples)

* `0x01` — plaintext debug / handshake
* `0x10` — encrypted application message
* `0x11` — encrypted telemetry
* `0x20` — link keep-alive / ping
* `0x21` — link stats / health report
* `0x30` — TTY-over-data segment (optional)

Exact values are TBD and should be documented alongside the implementation.

---

## Keys and Identity (TBD)

Key management is not yet defined. Possible approaches:

* manually exchanged pre-shared keys
* QR code exchange for short-term sessions
* device-identity keys with out-of-band verification

The protocol intentionally keeps framing and crypto decoupled so key schemes can be swapped without changing the modem-level design.

---

## Error Handling

* frames with invalid SOF or CRC are discarded
* a simple ACK/NAK scheme can be used:

  * ACK frames reference received frame IDs
  * retransmission window is small due to narrow bandwidth
* repeated failures may trigger a link renegotiation or a fallback to TTY/text-only workflows.
