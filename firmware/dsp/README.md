# Uniline Firmware: DSP Layer (Modem + TTY)

> Signal processing for low-bandwidth data, TTY, and tone detection over analog phone lines.

The **DSP layer** is responsible for converting between raw audio samples and higher-level symbols: modem bits, TTY characters, and detected tones. It sits between the low-level audio driver and the mode logic (Monitor, Talk, Data).

This layer is performance-sensitive and should be kept as lean and deterministic as possible.

---

## 1. Goals

The DSP layer should:

* Provide **robust low-bitrate modem links** over noisy POTS lines
* Implement full **Baudot TTY encode/decode**
* Detect tones such as **dialtone, busy, ringback, DTMF, and carriers**
* Be MCU-agnostic (no direct hardware calls)
* Use fixed or bounded CPU and memory
* Expose simple, event-based APIs to higher layers

---

## 2. Architecture Overview

```text
          Audio Driver (ADC/DAC)
                   │
                   ▼
        +------------------------+
        |        DSP Core        |
        |------------------------|
        |  Modem Tx / Modem Rx   |
        |  TTY Tx   / TTY Rx     |
        |  Tone Detection        |
        +------------------------+
                   │
                   ▼
        Mode Logic (Monitor/Talk/Data)
```

All DSP components operate on **fixed-size audio frames** (e.g., 10–20 ms) pulled from the audio driver.

---

## 3. Sample Rate & Frame Size

Recommended defaults:

* **Sample rate:** 8 kHz or 16 kHz (compile-time configurable)
* **Frame size:** 80–160 samples per processing tick

Guidelines:

* Smaller frames → lower latency, higher CPU overhead
* Larger frames → more efficient but higher latency

TTY and low-speed modem work well at 8 kHz. For more advanced DSP, 16 kHz is preferred.

---

## 4. Modem DSP

The modem subsystem provides **FSK / PSK-based low-bitrate data links**.

### 4.1 Supported Modes (initial targets)

* **Bell 103**: 300 bps FSK
* **V.21**: 300 bps full duplex FSK
* **V.23**: 1200/75 bps (for asymmetric links, optional)

Future:

* 2400–9600 bps QAM/PSK for higher-quality lines

### 4.2 Transmit Path

Steps:

1. Input: bytes/frames from higher-level protocol
2. Encode to symbol stream (bits → tones)
3. Generate FSK/PSK audio samples
4. Write to TX audio buffer

API example:

```c
void modem_tx_reset(void);
void modem_tx_push_bytes(const uint8_t *data, size_t len);
size_t modem_tx_render(int16_t *out_samples, size_t max_samples);
```

### 4.3 Receive Path

Steps:

1. Input: audio samples from audio driver
2. Bandpass filter around modem frequencies
3. Detect symbol timing and phase/frequency
4. Decode bits into bytes
5. Push frames to higher layer with CRC result

API example:

```c
void modem_rx_reset(void);
void modem_rx_process(const int16_t *samples, size_t count);
size_t modem_rx_pop_bytes(uint8_t *out, size_t max_len);
```

### 4.4 Link Metrics

The modem layer should expose:

* SNR estimate
* Carrier detect flag
* Symbol error rate estimate
* Current bitrate / mode

These are used by the protocol layer to adjust link usage or fallback modes.

---

## 5. TTY DSP (Baudot)

The TTY subsystem handles **45.45 baud Baudot** over the audio path.

### 5.1 Encoding (Tx)

1. Input: UTF-8 or ASCII characters
2. Map to Baudot code points
3. Manage **LTRS/FIGS** state transitions
4. Generate 1400/1800 Hz mark/space tones
5. Output: audio samples for transmission

API example:

```c
void tty_tx_reset(void);
void tty_tx_push_text(const char *s);
size_t tty_tx_render(int16_t *out_samples, size_t max_samples);
```

### 5.2 Decoding (Rx)

1. Input: audio samples from line
2. Bandpass filter around TTY band
3. Detect mark/space using Goertzel or similar
4. Recover bit timing (45.45 baud)
5. Reassemble characters and manage LTRS/FIGS
6. Output: decoded UTF-8/ASCII text

API example:

```c
void tty_rx_reset(void);
void tty_rx_process(const int16_t *samples, size_t count);
size_t tty_rx_pop_text(char *out, size_t max_len);
```

### 5.3 Error Handling

TTY is tolerant but fragile on bad lines. Decoder should:

* Use hysteresis for tone detection
* Drop obviously invalid frames
* Optionally mark uncertain characters

---

## 6. Tone Detection

The tone detector runs in **Monitor** and **Talk** modes.

Detected signals:

* Dial tone
* Busy tone
* Ringback
* DTMF keys
* Modem carriers (Bell 103 / V.21 / V.23)
* TTY presence

Implementation guideline:

* Use **Goertzel filters** or short FFT per frame
* Represent results as simple events with confidence values

API example:

```c
typedef enum {
  TONE_NONE,
  TONE_DIAL,
  TONE_BUSY,
  TONE_RINGBACK,
  TONE_DTMF_0_9_STAR_HASH,
  TONE_MODEM_CARRIER,
  TONE_TTY_PRESENCE,
} tone_type_t;

void tone_detect_reset(void);
void tone_detect_process(const int16_t *samples, size_t count);
bool tone_detect_pop_event(tone_type_t *out_type, float *out_confidence);
```

---

## 7. Integration with Mode Logic

The DSP layer does **not** know about session states or encryption. It simply:

* Processes audio frames every tick
* Emits events (bytes, text, tones)
* Consumes commands (push bytes, push text)

Mode logic (Monitor/Talk/Data) decides:

* Whether modem or TTY is active
* When to enable/disable detection
* How to interpret DSP events

---

## 8. Performance & Resource Constraints

Target environment:

* Small MCU (tens of kB RAM for audio + buffers)
* No floating-point required (fixed-point strongly preferred)

Guidelines:

* Prefer integer or fixed-point DSP
* Use precomputed tables for sin/cos where needed
* Avoid dynamic allocation in DSP paths
* Keep per-frame processing bounded and deterministic

---

## 9. Testing Strategy

Recommended tests:

* Synthetic tone injection (TTY and modem) via test WAVs
* Loopback tests between two Uniline boards
* Noise and SNR sweep tests
* Real PSTN recordings for robustness

Unit tests should live alongside the DSP implementation and run on host (e.g., via simulated audio files).

---

## 10. Status

DSP design is currently architectural. Specific modem and TTY implementations will be iterated and optimized based on real-line testing.

---

## License

All DSP code is released under **CC0 1.0 (Public Domain)**.
