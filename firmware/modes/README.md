# Uniline Firmware: Mode Logic (Monitor / Talk / Data)

> How the firmware coordinates operating modes, safety checks, and DSP pipelines.

The **modes** layer implements Uniline’s three primary operating modes:

* **Monitor** — passive, high-impedance listening
* **Talk** — voice + TTY over an active off-hook connection
* **Data** — low-bitrate encrypted data over modem

This layer orchestrates the line interface, DSP engines, and protocol events, while enforcing safety constraints before any off-hook or high-power activity.

---

## 1. Design Goals

The mode system is responsible for:

* Keeping the device in a **safe default state** (Monitor / idle)
* Ensuring only **one active mode** at a time
* Applying **safety checks** before mode changes
* Starting/stopping the relevant DSP chains (modem, TTY, tone detection)
* Emitting **clear state signals** to the host (Web GUI / CLI)

Modes are state-machine-driven and must be deterministic.

---

## 2. High-Level State Machine

```text
          +------------------+
          |   START / IDLE   |
          +---------+--------+
                    |
                    v
             +------+------+
             |  MONITOR  |  (default safe state)
             +--+-----+--+
                |     |
        to Talk |     | to Data
                v     v
           +----+-----+----+
           |    TALK MODE   |
           +----+-----+-----+
                |     |
        to Mon. |     | to Data
                v     v
           +----+-----+----+
           |    DATA MODE   |
           +----+-----+----+
                |
                v
             back to
             MONITOR
```

All transitions must pass through **safety checks** and may be vetoed.

---

## 3. Mode Descriptions

### 3.1 Monitor Mode

**Role:**

* Default and fallback mode
* High-impedance, no line seizure

**Behavior:**

* FXO stays **on-hook**
* Line monitor path active
* Tone detector enabled (dial tone, TTY, modem, DTMF)
* Noise/SNR metrics gathered
* Optional TTY detection (presence, not full decode)

**Use cases:**

* Checking line viability
* Diagnosing line state
* Auto-suggesting next mode (Talk vs Data vs TTY)

---

### 3.2 Talk Mode

**Role:**

* Voice calls and TTY text calls

**Behavior:**

* FXO goes **off-hook** (line seized)
* Audio path is full duplex
* Voice path active (host audio ↔ line)
* TTY encoder/decoder may be enabled
* DTMF generator available for dialing

**Profiles within Talk Mode:**

* **Voice-only**: TTY disabled, pure voice
* **TTY**: Baudot DSP enabled, text I/O
* **VCO/HCO hybrid**: voice + TTY mixed according to user preference

**Use cases:**

* Regular voice calls
* TTY relay center calls
* Accessibility communication

---

### 3.3 Data Mode

**Role:**

* Low-bitrate digital data exchange using analog modems

**Behavior:**

* FXO off-hook connection maintained
* Modem DSP active (FSK/PSK)
* Encapsulated payloads exchanged via protocol layer
* Link metrics reported periodically (SNR, error rate)

**Data Mode is typically used for:**

* Encrypted text messaging
* Telemetry and status updates
* Command/control signals

Voice and TTY are **disabled** in this mode to keep DSP simple and predictable.

---

## 4. Safety Checks on Mode Transition

Before leaving Monitor or changing modes, the firmware must verify:

* **Line voltage** is within allowed range
* No **surge** or fault is being reported from the hardware
* FXO relay or solid-state switch is not stuck
* Power rails are stable

If any check fails:

* Transition is **aborted**
* Error reported to host via protocol (RSP_ERR / EVT_STATUS)
* Device remains or returns to **Monitor**

---

## 5. Mode Transition API

The mode module exposes a simple interface to other firmware layers:

```c
typedef enum {
  UNILINE_MODE_MONITOR = 0,
  UNILINE_MODE_TALK    = 1,
  UNILINE_MODE_DATA    = 2,
} uniline_mode_t;

void mode_init(void);
uniline_mode_t mode_get_current(void);
bool mode_request(uniline_mode_t target_mode);
```

* `mode_request()` performs safety checks and returns **true** only if the transition succeeds.
* Mode changes trigger **EVT_MODE** events to the host.

---

## 6. Interaction with DSP & Drivers

Each mode controls which subsystems are active:

### Monitor Mode

* FXO: on-hook
* Audio: monitor path active
* DSP: tone detection, optional TTY detection
* Modem: idle

### Talk Mode

* FXO: off-hook
* Audio: full duplex voice path
* DSP: voice + optional TTY encode/decode
* Modem: idle

### Data Mode

* FXO: off-hook
* Audio: modem-specific path
* DSP: modem Tx/Rx
* Tone detection: basic carrier/dialtone only

This keeps CPU and power usage consistent with the current task.

---

## 7. Mode Persistence & Fallback

The mode controller must handle:

* **Link failure** in Data mode → fallback to Monitor
* **Unexpected line drop** in Talk mode → end call, fallback to Monitor
* **Host disconnect** → gracefully return to safe state

If anything unexpected happens (watchdog, assertion, firmware error), the default behavior is:

* Force **FXO on-hook**
* Re-enter **Monitor mode**
* Emit error/status event to host on reconnect

---

## 8. Host Integration

The Web GUI and CLI tools:

* Request mode changes using `CMD_CTRL / SUBCMD_SET_MODE`
* Display the current mode based on `EVT_MODE`
* Display line/tone status based on `EVT_STATUS` and `EVT_TONE`

The host should **not** assume a mode change will always succeed; it must wait for confirmation from the node.

---

## 9. Testing & Simulation

To validate mode logic:

* Simulate line voltage transitions
* Simulate ring events and off-hook/on-hook sequences
* Run DSP test tones in each mode
* Verify that:

  * Monitor never seizes the line
  * Talk and Data correctly engage FXO
  * Fallback to Monitor occurs on errors

---

## 10. Status

Mode logic is defined at a high level and will be refined alongside hardware prototypes and Web GUI behavior.

---

## License

All mode logic code and documentation are released under **CC0 1.0 (Public Domain)**.
