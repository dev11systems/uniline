# Uniline Hardware: Testing & Validation

> Practical checks to ensure Uniline is safe, stable, and ready for real-world use.

This document describes recommended testing procedures for Uniline prototypes and production units. The focus is on safety, isolation, mode correctness, and basic modem/TTY functionality.

These tests should be performed **before** connecting to live PSTN lines in the field.

---

# 1. Test Categories

Uniline hardware should be validated in the following areas:

1. **Visual inspection & assembly checks**
2. **Power & isolation tests**
3. **FXO line interface tests**
4. **Monitor (high-impedance) mode tests**
5. **Talk (off-hook) mode tests**
6. **Data (modem) mode tests**
7. **TTY functionality tests**
8. **Environmental and stress tests** (optional/advanced)

---

# 2. Visual & Assembly Checks

Before applying power:

* Verify correct component orientation:

  * Diodes, TVS, MOVs, GDT
  * Polarity on electrolytic capacitors
  * DC/DC isolation module orientation
* Confirm isolation clearances:

  * Creepage/clearance around line side
  * Isolation slots under transformer and DC/DC module
* Inspect solder joints:

  * No bridges on fine-pitch ICs
  * Good wetting on high-current paths
* Confirm correct connectors:

  * RJ11 pinned as expected
  * USB-C wired correctly

If available, cross-check against the PCB assembly drawing.

---

# 3. Power & Isolation Tests

## 3.1 No-Line, USB-Only Power-Up

1. Leave PSTN line **disconnected**.
2. Connect Uniline to USB-C power (laptop or power bank).
3. Confirm:

   * No excessive current draw
   * No overheating components
   * Status LED pattern matches "idle"/"no line" state

## 3.2 Isolation Check

Using a multimeter:

* Measure resistance between USB ground and line side (tip or ring):

  * Should be **> 10 MΩ** (ideally open circuit on DC)
* Confirm no inadvertent continuity through mounting holes or shields.

## 3.3 DC/DC Stability

* Confirm isolated 5V/3.3V lines within tolerance
* Check for excessive ripple if an oscilloscope is available

---

# 4. FXO Line Interface Tests

Use a **POTS line simulator**, PBX port, or known-safe analog line for initial tests.

## 4.1 Ring Detection

1. Connect to a test line capable of generating ring voltage.
2. Trigger an incoming call.
3. Confirm:

   * Ring detection logic reports an event to firmware
   * Ring indicator LED or log message appears in host UI (if available)

## 4.2 Line Voltage Sensing

* With the device on-hook, read line voltage via firmware/CLI.
* Compare with multimeter reading on tip/ring.
* Ensure values are sensible and scaled correctly.

---

# 5. Monitor Mode Tests

Monitor mode must **not** seize the line.

## 5.1 Non-Seizure Verification

1. Connect Uniline to a live analog line with an analog phone in parallel.
2. Enter **Monitor mode**.
3. Confirm:

   * Phone still behaves normally (can receive calls, no off-hook indication)
   * No dial tone loss when Uniline enters Monitor

## 5.2 Audio Monitoring

1. In Monitor mode, capture audio on the host using the Web GUI or CLI.
2. Confirm presence of:

   * Dial tone when off-hook with a separate phone
   * Ringback tones during outbound calls
   * Busy signal when dialed

Optional: check waveforms via the host’s audio monitor.

---

# 6. Talk Mode Tests

Talk mode **does** seize the line—testing must be done carefully.

## 6.1 Off-Hook Behavior

1. Connect Uniline to a test line or PBX port.
2. Enter **Talk mode**.
3. Confirm:

   * Line transitions from ~48 VDC to ~6–12 VDC
   * Loop current is within expected range
   * Dial tone is audible at the host device

## 6.2 Call Placement

1. From Talk mode, use DTMF dialing via host UI.
2. Place a call to a known test number or second analog endpoint.
3. Confirm:

   * Ringback is heard
   * Remote endpoint rings
   * Voice audio is intelligible in both directions

---

# 7. Data (Modem) Mode Tests

For early testing, use **low-speed modems**.

## 7.1 Loopback / Local Test

1. Use two Uniline units on a controlled analog line (or simulator).
2. Place a call from A → B.
3. Enter **Data mode** on both ends.
4. Confirm:

   * Modem tones are heard
   * Link negotiates at 300–1200 bps

## 7.2 Encrypted Data Path

(Once firmware supports crypto):

1. Send short text bursts between nodes.
2. Confirm:

   * Messages arrive in order
   * No silent corruption at low SNR
   * Link gracefully fails or retries when conditions worsen

---

# 8. TTY Mode & TTY-over-Line Tests

## 8.1 Baudot Tone Generation

1. Use Uniline to originate a TTY call in Talk mode.
2. Connect to a TTY device, relay service, or TTY simulator.
3. Confirm:

   * Baudot tones are generated at expected frequencies
   * Received text is intelligible

## 8.2 TTY Decode in Monitor Mode

1. Connect Uniline in parallel with a known TTY call.
2. Enter Monitor mode.
3. Confirm:

   * Baudot is detected
   * Text appears in host UI/TTY log

---

# 9. Safety & Fault-Condition Tests

These tests ensure failure modes are safe:

* **Short-circuit line legs** briefly (through protection fixtures) and confirm:

  * PTC fuses trip
  * Device recovers after event
* **Simulate lightning surge** using proper test equipment (if available)

  * Confirm surge path goes through GDT/MOV, not into logic
* **Unplug USB during active mode**

  * Device should fail gracefully and return to safe on-hook state

Do **not** attempt improvised surge testing without proper equipment.

---

# 10. Environmental / Stress Testing (Optional)

If possible:

* **Temperature cycling:**

  * Test at low (–10°C) and high (+55°C) ambient
  * Confirm line interface still functions

* **Vibration/shock:**

  * Drop tests from pocket height onto hard surfaces
  * Confirm no internal mechanical failures

* **Humidity:**

  * Expose to high humidity (but not immersion)
  * Confirm no condensation-related faults

---

# 11. Pass/Fail Criteria

A Uniline unit is considered field-ready when:

* Isolation and line safety tests pass
* Monitor, Talk, Data modes all behave as designed
* TTY send/receive works on at least one real or simulated call
* No overheating at max continuous load
* No unexpected off-hook events
* USB host devices remain protected and stable

---

# 12. Summary

This testing plan ensures that Uniline hardware:

* Is safe around PSTN voltages
* Behaves predictably across all operating modes
* Delivers clean audio for modem and TTY
* Survives realistic field conditions

Thorough validation is essential before using Uniline in disaster response, humanitarian, or research scenarios.
