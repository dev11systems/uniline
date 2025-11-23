# Example Topologies

This document shows how Uniline nodes might be arranged in real-world scenarios.

---

## Clinic ↔ Hospital

**Scenario:** A small clinic and a regional hospital both retain at least one working analog phone line during an outage.

**Topology:**

* One Uniline node at the clinic, connected to its remaining POTS line.
* One Uniline node at the hospital, connected to a reachable line.
* Calls are routed via the PSTN where possible.

**Usage:**

* Data mode is used for:

  * basic status messages (patient counts, triage categories)
  * supply and transport requests
  * short updates about local conditions
* Talk mode is used occasionally for voice conversations when necessary.
* TTY fallback is available for highly degraded lines.

---

## Shelter ↔ Coordination Hub

**Scenario:** A community shelter maintains a single analog line, and a city coordination hub has multiple lines in service.

**Topology:**

* Uniline at the shelter on its only line.
* Uniline at the coordination hub on a designated inbound line.

**Usage:**

* Shelter sends periodic status updates (occupancy, needs, incidents).
* Coordination hub sends routing info, schedules, and instructions.
* TTY is used as an extreme fallback if noise prevents normal data use.

---

## Multi-Site Community Network

**Scenario:** Several community centers, schools, or libraries each retain a working POTS line, even though residential connections have failed.

**Topology:**

* One Uniline node at each site.
* Sites call each other via PSTN.

**Usage:**

* Regular check-ins between sites.
* Reporting on food/water distribution points.
* Sharing road closure and hazard updates.
* Offering a path for residents to send brief “I’m safe” messages relayed through the hubs.

---

## Minimal Point-to-Point Link

**Scenario:** Two locations each have a single copper line that can call the other, but no internet access.

**Topology:**

* One Uniline node at each endpoint.
* Direct PSTN call between the two lines.

**Usage:**

* Data mode provides an encrypted low-bandwidth link for:

  * short text messages
  * sensor/telemetry feeds (e.g. basic environmental data)
* Talk mode is available as needed for voice calls or TTY.

---

## Testing and Lab Setup

**Scenario:** A lab environment where PSTN access is limited or not available.

**Topology:**

* Two Uniline nodes connected through:

  * an analog line simulator
  * or a simple in-house two-line analog setup

**Usage:**

* Development and debugging of modem and TTY layers.
* Measuring performance under controlled noise and attenuation.
* Demonstrating Uniline’s behavior to students or collaborators.
