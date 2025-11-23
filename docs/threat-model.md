# Threat Model

This document identifies risks relevant to Uniline and defines the boundaries of what the system does and does not protect.

---

## Scope

Uniline aims to provide *basic, low-bandwidth, encrypted communication* over analog telephone lines during infrastructure outages. It is not designed to resist nation‑state interception or highly capable adversaries.

Threat modeling is essential so users understand:

* what the system defends against
* what it cannot defend against
* how to use it appropriately in emergencies or research

---

## Assumptions

* The analog line (local loop + PSTN) is **untrusted**.
* Adversaries may intercept analog audio or modem signals.
* Endpoints (phones/laptops) are assumed uncompromised.
* Keys must be exchanged out-of-band.

---

## In-Scope Threats

### 1. **Passive Eavesdroppers on the Analog Line**

* Attackers may attach to exposed copper lines.
* Encrypted data frames provide confidentiality.
* TTY and voice audio are *not* protected unless the app adds higher-layer encryption.

### 2. **Corrupted or Noisy Channels**

* Noise, interference, and degraded copper pairs are expected.
* Modem profile and framing include:

  * error detection
  * retransmission
  * low-rate fallback

### 3. **Replay Attacks on Data Frames**

* AEAD ensures integrity.
* Nonces and counters prevent replays.

### 4. **Impersonation of a Node**

* Prevented by out-of-band key verification.

---

## Out-of-Scope Threats

### 1. **Compromised Endpoints**

* Malware on the phone/laptop defeats encryption.
* Physical compromise of the device is not addressed.

### 2. **Targeted Hardware Attacks**

* TEMPEST/side-channel attacks.
* Physical probing of the Uniline node.

### 3. **Active PSTN Manipulation by Capable Adversaries**

* Government-level PSTN interception or rerouting.
* Line-card level attacks at central offices.

### 4. **High-Bandwidth or High-Performance Cryptography**

* Uniline operates on severely constrained links.
* Not designed for VPN-like security or throughput.

### 5. **Lawful Intercept Evasion**

* Uniline is not an anonymity or anti-surveillance tool.

---

## Safety Considerations

* Life-safety systems (fire alarm circuits, emergency phones) must not be interfered with.
* High-impedance Monitor Mode avoids unintended line seizure.
* Off-hook behavior should be explicit and controlled.
* Firmware should prefer safe fail modes.

---

## Summary

Uniline offers **practical risk reduction** when:

* networks are down
* copper lines remain active
* users need secure-ish low-bandwidth communication

But it should not be relied on for:

* strong anonymity
* resisting nation‑state interception
* high-assurance cryptography

The threat model prioritizes *realistic emergency conditions*—not perfect security.
