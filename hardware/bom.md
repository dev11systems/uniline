# Hardware Bill of Materials (Sketch)

This is a preliminary, non-exhaustive list for a Uniline node. Exact parts and values are **TBD** and will evolve with prototypes.

---

## Major Functional Blocks

* **MCU / SoC**

  * Low-power microcontroller with:

    * enough RAM/flash for DSP + crypto
    * USB device support
    * optional integrated Bluetooth (if not offloaded)
  * Candidates: ESP32-S3, STM32 series, RP2040 + external components.

* **FXO Line Interface**

  * Galvanic isolation
  * Surge and overvoltage protection
  * On-hook / off-hook control (FXO behavior)
  * Two-wire line coupling
  * May use:

    * integrated FXO/DAA module; or
    * discrete transformer-based design + protection network

* **Audio Codec**

  * ADC/DAC channels for:

    * modem tones
    * TTY tones
    * voice-band audio
  * Optionally integrated in MCU for simple builds.

* **Power Subsystem**

  * USB-C input (5V)
  * Voltage regulation for MCU and analog
  * ESD and reverse polarity protection
  * Optional battery input (TBD)

* **Connectivity**

  * USB-C connector (data + power)
  * Bluetooth radio or module (if used)
  * Basic debugging header or pads (SWD/JTAG/UART)

* **UX / Indicators**

  * Status LEDs:

    * Power
    * Line off-hook / on-hook
    * Data link active
  * Optional buttons:

    * Safe reset
    * Mode override / test

---

## Example BOM Skeleton

> Note: Part numbers here are placeholders. Replace with specific choices during schematic and layout design.

### Semiconductors

* `U1` — MCU (e.g. ESP32-S3 / STM32 / RP2040)
* `U2` — Audio codec / ADC-DAC (if not integrated)
* `U3` — FXO/DAA module or line interface IC (if used)
* `U4` — USB-C PD/negotiation IC (optional, if needed)

### Passive Components

* Line isolation transformer
* Resistors/capacitors for:

  * filtering
  * impedance matching
  * biasing
* Protection components:

  * MOVs / TVS diodes
  * resettable fuses (PTC)

### Connectors

* USB-C receptacle (through-hole or mid-mount)
* RJ11 jack or screw terminal block for line
* Programming header (SWD/JTAG/UART)

### Power

* 5V → 3.3V regulator (LDO or switching)
* Bulk and decoupling capacitors

### Indicators / UI

* LEDs (PWR, LINE, DATA)
* Momentary pushbuttons (mode / reset)

---

## Notes

* Final BOM will depend on:

  * regulatory requirements in the deployment region
  * chosen MCU and toolchain
  * whether Bluetooth is required in hardware
* All components should be standard, non-exotic, and reasonably available from major distributors.

As designs stabilize, this document should be updated with real part numbers, suggested vendors, and cost estimates.
