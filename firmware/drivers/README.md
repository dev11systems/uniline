# Uniline Firmware: Drivers Layer

> Hardware abstraction for FXO, audio, timing, and I/O.

The **drivers** layer provides all low-level hardware access used by the Uniline firmware. It abstracts MCU- and board-specific details behind a common interface so that higher-level modules (DSP, modes, protocol) can remain portable across platforms.

This directory is where you implement and port Uniline to different microcontrollers (ESP32, RP2040, STM32, etc.).

---

## Goals

The drivers layer should:

* Isolate all **MCU-specific details** from the rest of the firmware
* Provide a **stable, documented interface** for higher layers
* Safely control the **FXO interface** (off-hook, ring detect, line sense)
* Provide **timing primitives** required for TTY and modem DSP
* Manage **ADC/DAC or codec** configuration for audio I/O
* Handle **USB** or **serial endpoint** setup as required by the protocol layer

---

## Responsibilities

The drivers layer covers:

1. **FXO + Line Interface Control**
2. **ADC/DAC / Codec Configuration**
3. **GPIO Management** (LEDs, relays, hookswitch)
4. **Timers & Interrupts** for DSP
5. **USB / Serial / UART** device setup
6. **Non-volatile storage access** (optional)

---

## Module Overview

Recommended structure:

```text
firmware/endpoint/drivers/
  fxo.h / fxo.c          ← line interface control (off-hook, ring, voltage)
  audio.h / audio.c      ← codec, ADC/DAC, audio buffers
  gpio.h / gpio.c        ← LEDs, mode button, relays
  timers.h / timers.c    ← periodic timers, high-precision ticks
  usb_serial.h / .c      ← WebSerial / CDC endpoint setup
  platform.h             ← MCU + board config
```

You can adapt this structure to your preferred style (C, C++, Rust, etc.).

---

## FXO Driver

The FXO driver controls and monitors the PSTN line interface.

### Responsibilities

* Engage / disengage off-hook relay or solid-state hook switch
* Read line voltage and loop current (via ADC or digital sensor)
* Report ring detection events
* Guard against illegal or unsafe states

### Key API (example)

```c
void fxo_init(void);
void fxo_set_offhook(bool active);
bool fxo_get_offhook(void);
bool fxo_ring_detected(void);
float fxo_get_line_voltage(void);
float fxo_get_loop_current(void);
```

---

## Audio Driver

The audio driver configures the ADC/DAC or external codec and provides access to audio frames for the DSP stack.

### Responsibilities

* Initialize codec or internal ADC/DAC
* Configure sample rate (8–16 kHz)
* Manage RX/TX audio buffers
* Provide interrupt or DMA callbacks for continuous streaming

### Key API (example)

```c
void audio_init(void);
void audio_start(void);
void audio_stop(void);
size_t audio_read_samples(int16_t *buf, size_t max_samples);
size_t audio_write_samples(const int16_t *buf, size_t count);
```

---

## GPIO Driver

Handles all simple digital I/O.

### Responsibilities

* Mode button input
* LED outputs (power, mode, link, error)
* Control lines for relays / analog switches

### Key API (example)

```c
void gpio_init(void);
bool gpio_read_mode_button(void);
void gpio_set_led_power(bool on);
void gpio_set_led_mode(int mode);   // e.g., MONITOR, TALK, DATA
void gpio_set_led_error(bool on);
```

---

## Timers & Interrupts

Stable timing is critical for modem and TTY DSP.

### Responsibilities

* Millisecond-level system ticks
* High-resolution timers for symbol timing (e.g., 45.45 baud)
* Scheduling periodic DSP tasks

### Key API (example)

```c
void timers_init(void);
uint32_t timers_get_ms(void);
void timers_delay_ms(uint32_t ms);

// Optional: register callbacks
void timers_register_periodic(void (*cb)(void), uint32_t period_ms);
```

---

## USB / Serial Driver

Provides the transport used by the **protocol layer** to talk to the host.

### Responsibilities

* Initialize USB-CDC or USB custom interface
* Present a serial-like endpoint for WebSerial
* Provide read/write primitives with buffering

### Key API (example)

```c
void usb_serial_init(void);
int  usb_serial_read(uint8_t *buf, size_t max_len);
int  usb_serial_write(const uint8_t *buf, size_t len);
bool usb_serial_connected(void);
```

This layer does **not** parse messages — it only moves bytes.

---

## Platform Configuration

`platform.h` (or equivalent) should describe:

* MCU type (e.g., ESP32-S3, RP2040)
* Pin assignments (FXO control, LEDs, buttons, audio, USB)
* Available peripherals (I2S, I2C, SPI, UART)

This makes it easier to create **board profiles** and support multiple reference designs.

---

## Porting Guidelines

To port Uniline to a new MCU:

1. Implement all mandatory driver APIs for that platform.
2. Verify FXO control and isolation behavior in **Monitor** and **Talk** modes.
3. Validate audio sample paths and timing (TTY and modem test tones).
4. Confirm USB-serial connectivity with the Web GUI or a terminal.
5. Run through `hardware/testing.md` procedures as a sanity check.

---

## Status

The drivers layer is in early design. Exact APIs may change as DSP and protocol layers evolve, but the separation of concerns will remain.

---

## License

All driver code is released under **CC0 1.0 (Public Domain)**.
