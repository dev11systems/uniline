# Uniline Firmware: Utils Layer

> Shared helpers for buffers, CRC, framing, logging, and small system utilities.

The **utils** layer provides common functionality used across the firmware: protocol framing, CRC calculation, ring buffers, simple logging, and miscellaneous helpers. Keeping these utilities centralized avoids duplication and keeps the codebase easier to maintain and port.

---

## 1. Responsibilities

The utils layer includes:

* **CRC utilities** (CRC-16-CCITT for protocol frames)
* **Ring buffers / FIFOs** for audio, bytes, and events
* **Framing helpers** for building and parsing host protocol messages
* **Small math helpers** (fixed-point, clamping, etc.)
* **Logging hooks** (compile-time-on/off)

These utilities must be **hardware-agnostic** and should not depend on any MCU-specific APIs.

---

## 2. Directory Structure

Example layout:

```text
firmware/endpoint/utils/
  crc16.h / crc16.c        ← CRC-16-CCITT implementation
  ringbuf.h / ringbuf.c    ← generic ring buffer
  frame.h / frame.c        ← build/parse protocol frames
  log.h / log.c            ← logging macros & sinks
  fixed.h / fixed.c        ← optional fixed-point helpers
```

You can adjust naming or merge files as needed, as long as the functionality remains clearly separated.

---

## 3. CRC Utilities

The host protocol uses **CRC-16-CCITT** for frame integrity.

### Requirements

* Standard CRC-16-CCITT polynomial (0x1021)
* Initial value configurable (commonly 0xFFFF)
* Byte-wise incremental API

### Example API

```c
#include "crc16.h"

uint16_t crc16_ccitt_init(void);
uint16_t crc16_ccitt_update(uint16_t crc, const uint8_t *data, size_t len);
```

Used by both:

* Firmware protocol layer
* Host-side reference implementations

---

## 4. Ring Buffers

Ring buffers are used for:

* Audio sample queues
* Byte streams from USB-serial
* DSP events (tones, text, frames)

### Example API

```c
#include "ringbuf.h"

typedef struct {
  uint8_t *data;
  size_t   size;
  size_t   head;
  size_t   tail;
} ringbuf_t;

void   ringbuf_init(ringbuf_t *rb, uint8_t *storage, size_t size);
size_t ringbuf_available(const ringbuf_t *rb);
size_t ringbuf_space(const ringbuf_t *rb);
size_t ringbuf_write(ringbuf_t *rb, const uint8_t *data, size_t len);
size_t ringbuf_read(ringbuf_t *rb, uint8_t *out, size_t maxlen);
```

Guidelines:

* No dynamic allocation inside ringbuf
* Single-writer/single-reader assumptions preferred

---

## 5. Framing Helpers

These helpers build and parse the binary host protocol frames described in `firmware/endpoint/protocol/README.md`.

### Responsibilities

* Construct frames with sync header, type, length, payload, and CRC
* Incrementally parse incoming bytes and emit completed frames
* Validate CRC before passing frames to the protocol layer

### Example API

```c
#include "frame.h"

#define UNILINE_FRAME_MAX_PAYLOAD 256

typedef struct {
  uint8_t  type;
  uint16_t length;
  uint8_t  payload[UNILINE_FRAME_MAX_PAYLOAD];
} uniline_frame_t;

void frame_builder_reset(void);
size_t frame_build(uint8_t type, const uint8_t *payload,
                   uint16_t length, uint8_t *out_buf, size_t out_max);

void frame_parser_reset(void);
bool frame_parser_push_byte(uint8_t byte, uniline_frame_t *out_frame);
```

The parser must:

* Search for sync bytes (0x55 0xAA)
* Read type/length
* Collect payload
* Verify CRC

---

## 6. Logging Utilities

Logging in embedded environments must be:

* Optional (compile-time or runtime disabled)
* Low overhead
* Safe in ISR contexts (or avoided there)

### Example API

```c
#include "log.h"

void log_init(void);
void log_set_level(int level);

void log_debug(const char *fmt, ...);
void log_info(const char *fmt, ...);
void log_warn(const char *fmt, ...);
void log_error(const char *fmt, ...);
```

Output options:

* USB serial (debug channel)
* SWO / semihosting (platform-dependent)

When building release firmware for field use, logging can be compiled out entirely.

---

## 7. Fixed-Point & Small Math Helpers (Optional)

Modem and TTY DSP may benefit from:

* Fixed-point multiplication helpers
* Saturating arithmetic
* Lookup-table based sine/cosine

All of these can live in `fixed.h` / `fixed.c` if needed.

---

## 8. Coding Guidelines

Utilities must:

* Use no dynamic allocation (`malloc`/`free`)
* Avoid platform-specific calls
* Be re-usable on host (for tests) when possible
* Be covered by unit tests where it makes sense

---

## 9. Testing

Unit tests (run on host PC) should cover:

* CRC correctness vs known test vectors
* Ring buffer behavior under wraparound conditions
* Frame parser robustness with malformed input and partial frames
* Logging macros compile with and without debug enabled

---

## 10. Status

The utils layer is a supporting component. Its API will evolve as protocol and DSP layers firm up, but the role—shared helpers for the entire firmware—will remain the same.

---

## License

All utility code is released under **CC0 1.0 (Public Domain)**.
