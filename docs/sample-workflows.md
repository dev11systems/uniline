# Sample Workflows

This document demonstrates how real-world Uniline sessions might unfold, using the three core modes: Data, Monitor, and Talk (with integrated TTY).

---

## 1. Basic Encrypted Message Exchange

**Scenario:** Two sites have Uniline nodes and functioning analog lines.

**Steps:**

1. Node A user opens the app and selects **Data Mode**.
2. App instructs the node to go off-hook and dial Node B’s number.
3. Node B auto-answers (auto-answer optional in settings).
4. Modem handshake begins; link stabilizes.
5. App displays “Secure Link Established.”
6. Users exchange:

   * short text
   * telemetry updates
   * status reports

**Result:** Low-bandwidth encrypted message channel over analog copper.

---

## 2. Switching to Talk Mode for Voice

**Scenario:** A user needs to speak to the remote operator after exchanging text.

**Steps:**

1. Data Mode is active.
2. User taps **Switch to Talk Mode**.
3. App signals the node to stop the modem carrier and enable the audio path.
4. Smartphone becomes the handset (USB-C or Bluetooth).
5. Users talk normally.

**Result:** Seamless fallback from digital to voice without hanging up.

---

## 3. TTY Text Call (Direct Baudot)

**Scenario:** A user needs a text-only call that works even on degraded lines.

**Steps:**

1. User selects **Talk + TTY** mode.
2. Node goes off-hook and dials.
3. Upon answer, the TTY DSP:

   * detects Baudot tones
   * begins bidirectional encode/decode
4. App shows typed text in a chat-like UI.

**Result:** Reliable text communication, even in poor conditions.

---

## 4. Monitoring a Line for Activity

**Scenario:** A responder wants to check which lines at a shelter are live.

**Steps:**

1. User plugs Uniline into the block.
2. Sets mode to **Monitor (High Impedance)**.
3. App shows:

   * presence/absence of dial tone
   * noise level / hum
   * modem/TTY tone detection
   * ring voltage events (optional)

**Result:** Safe diagnostics without affecting line state.

---

## 5. Outbound Emergency Coordination Message

**Scenario:** Coordination hub broadcasts status to multiple small sites.

**Steps:**

1. Hub queues a templated message (e.g., shelter supply update).
2. App places sequential outbound calls to each site.
3. Each site:

   * auto-answers in Data Mode
   * receives and stores the message
   * optionally sends an ACK

**Result:** Slow but effective store-and-forward disaster messaging.

---

## 6. Minimal Off-Grid Workflow

**Scenario:** User has a phone, a Uniline node, no internet, and a working copper line.

**Steps:**

1. Phone connects to node via USB-C.
2. Node uses the line to call another Uniline.
3. Users exchange secure text.
4. If needed, switch to Talk or TTY.

**Result:** End-to-end workflows without cell, Wi-Fi, or external infrastructure.
