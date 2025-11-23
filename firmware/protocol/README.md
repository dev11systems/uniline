# Uniline Firmware: Host Protocol (WebSerial/WebUSB)

> Command and data framing between the Uniline node and the browser-based Web GUI (or CLI tools).

The **protocol layer** defines how the Uniline endpoint firmware communicates with a host (laptop, phone, or other computer) over USB (WebSerial/WebUSB) and, optionally, BLE control channels.

It is intentionally simple, text-lean, and binary-friendly, designed to be easy to implement in multiple languages and tools.

---

## 1. Goals

The host protocol should:

* Work over **USB CDC / WebSerial** in any modern browser
* Support **simple CLI tools** (e.g., Python, Go, Rust)
* Be **robust** on noisy or unstable links
* Support **command/response** and **streaming data**
* Be easy to extend while preserving backward compatibility

The protocol **does not** perform encryption itself; crypto is layered on top of logical data channels.

---

## 2. Transport Assumptions

Primary transport: **USB-Serial (CDC)** exposed as WebSerial/WebUSB.

* Byte stream, full duplex
* No inherent message boundaries
* Application-level framing required

Optional:

* BLE UART-style service (control only, no audio)

---

## 3. Framing Format

All messages follow a **binary framed structure**:

```text
+------------+---------+----------+----------+-----------+
| 0x55 0xAA  |  TYPE   |  LENGTH  |  PAYLOAD |   CRC16   |
+------------+---------+----------+----------+-----------+
   2 bytes    1 byte    2 bytes     N bytes    2 bytes
```

* **Sync (0x55 0xAA)**: start-of-frame marker
* **TYPE**: message type (command, response, stream, event)
* **LENGTH**: payload length in bytes (big-endian)
* **PAYLOAD**: message-specific content
* **CRC16**: CRC-16-CCITT over TYPE + LENGTH + PAYLOAD

Frames may be concatenated; the receiver must parse sequentially.

---

## 4. Message Types

Proposed message type byte values:

```text
0x01  CMD_CTRL        // control commands (set mode, etc.)
0x02  CMD_QUERY       // request status / metrics
0x03  CMD_DATA_TX     // send data (Data mode payload)

0x11  EVT_STATUS      // async status update
0x12  EVT_MODE        // mode changed
0x13  EVT_TONE        // tone detected
0x14  EVT_TTY_TEXT    // TTY text received
0x15  EVT_DATA_RX     // data received (Data mode)

0x21  RSP_OK          // generic success response
0x22  RSP_ERR         // error with code
0x23  RSP_QUERY       // structured reply to CMD_QUERY
```

Exact values can be refined, but separation between CMD/EVT/RSP is important.

---

## 5. Control Commands (CMD_CTRL)

`CMD_CTRL` payloads are small binary command blocks.

### 5.1 Set Mode

```text
TYPE   = CMD_CTRL (0x01)
LENGTH = 2
PAYLOAD:
  0x01   SUBCMD_SET_MODE
  0xMM   MODE (0=Monitor, 1=Talk, 2=Data)
```

### 5.2 TTY Control

```text
TYPE   = CMD_CTRL
LENGTH = 2
PAYLOAD:
  0x02   SUBCMD_TTY_ENABLE
  0x00/1 ENABLE (0=off, 1=on)
```

### 5.3 Line Control (Advanced)

```text
TYPE   = CMD_CTRL
LENGTH = 2
PAYLOAD:
  0x03   SUBCMD_HOOK
  0x00/1 HOOK (0=on-hook, 1=off-hook)
```

The host usually uses **Set Mode**, and firmware decides how to manipulate hook state.

---

## 6. Query Commands (CMD_QUERY)

Used to request status snapshots.

### 6.1 Query Status

```text
TYPE   = CMD_QUERY
LENGTH = 1
PAYLOAD:
  0x01   SUBQUERY_STATUS
```

Response: `RSP_QUERY` with structured status.

### 6.2 Query Line Metrics

```text
TYPE   = CMD_QUERY
LENGTH = 1
PAYLOAD:
  0x02   SUBQUERY_LINE_METRICS
```

Response includes:

* Line voltage
* Loop current (if off-hook)
* Noise level
* Detected tone flags

---

## 7. Data Channel (CMD_DATA_TX / EVT_DATA_RX)

Used in **Data mode** for encrypted payloads or plain test data.

### 7.1 Transmit from Host → Node

```text
TYPE   = CMD_DATA_TX
LENGTH = N
PAYLOAD:
  [N bytes of application data]
```

Firmware segments this into modem frames.

### 7.2 Receive from Node → Host

```text
TYPE   = EVT_DATA_RX
LENGTH = N
PAYLOAD:
  [N bytes of recovered application data]
```

The host is responsible for any higher-level crypto framing.

---

## 8. TTY Text (EVT_TTY_TEXT)

TTY text messages are UTF-8 strings decoded from Baudot.

```text
TYPE   = EVT_TTY_TEXT
LENGTH = N
PAYLOAD:
  [UTF-8 text bytes, no null terminator]
```

For sending TTY text from host to node, either:

* Use a dedicated `CMD_TTY_TX` type, or
* Reuse `CMD_DATA_TX` for TTY-over-data links (future)

A simple dedicated command is recommended:

```text
TYPE   = CMD_CTRL
LENGTH = 1 + N
PAYLOAD:
  0x10   SUBCMD_TTY_TX
  [N bytes of UTF-8 text]
```

---

## 9. Status & Mode Events

The node emits status events asynchronously.

### 9.1 Mode Change

```text
TYPE   = EVT_MODE
LENGTH = 1
PAYLOAD:
  0xMM   MODE (0=Monitor, 1=Talk, 2=Data)
```

### 9.2 Status Event

```text
TYPE   = EVT_STATUS
LENGTH = variable
PAYLOAD:
  [key-value status fields]
```

Fields can be encoded as TLV (Type-Length-Value) for forward compatibility.

---

## 10. Tone Events (EVT_TONE)

The DSP layer reports tone detection events up to the host.

```text
TYPE   = EVT_TONE
LENGTH = 2
PAYLOAD:
  0xTT   TONE TYPE (dial, busy, ringback, TTY, modem, dtmf)
  0xCC   CONFIDENCE (0–255)
```

The Web GUI can display these as small status indicators.

---

## 11. Error Handling (RSP_ERR)

On invalid commands or safety violations:

```text
TYPE   = RSP_ERR
LENGTH = 2
PAYLOAD:
  0xEC   ERROR CODE
  0xXX   DETAIL / SUBCODE
```

Example error codes:

```text
0x01  ERR_UNKNOWN_CMD
0x02  ERR_BAD_LENGTH
0x03  ERR_INVALID_MODE
0x04  ERR_LINE_UNSAFE
0x05  ERR_BUSY
0x06  ERR_UNSUPPORTED
```

---

## 12. Host-Side Implementation Notes

* The host must **buffer bytes** and search for `0x55 0xAA` sync markers
* Use `LENGTH` field to know how many bytes to read
* Verify CRC16 before trusting a frame
* Process frames sequentially, then emit events or responses into the Web GUI

The reference implementation lives in:

```text
web/js/uniline-api.js
```

---

## 13. Extensibility

To extend the protocol safely:

* Add new `TYPE` values for new command families
* Or introduce new **subcommands** under `CMD_CTRL`
* Keep existing codes stable to avoid breaking old GUIs

Unused or unknown message types should be **ignored**, not treated as fatal.

---

## 14. Status

The protocol is in draft form and may evolve as firmware and Web GUI implementations mature.

---

## License

All protocol specifications and reference code are released under **CC0 1.0 (Public Domain)**.
