# STM — an accessible scanning tunneling microscope

> **Project stage: pre-build.** This repository documents an in-progress effort to build a compact, open scanning tunneling microscope (STM). The electronics plan, control software, circuit references, and procurement list are ready for the next step: physical construction and validation. Atomic-resolution imaging is the goal—not a result claimed by this project yet.

## Why this project

Scanning tunneling microscopy makes the quantum world visible. When a sharp conductive tip is brought extremely close to a conductive sample, electrons can tunnel across the tiny gap. That tunneling current changes sharply with tip–sample separation, providing an exquisitely sensitive signal for mapping a surface.

Commercial STMs are powerful but expensive and difficult to access outside specialist labs. This project explores a more approachable path: a thoughtfully engineered instrument built from obtainable electronics, piezo components, and straightforward mechanical hardware. The aim is to turn a challenging piece of nanoscience equipment into a transparent, hands-on learning platform.

## Funding request

**Request: $600**

The $600 grant request covers the components required to move from design to a testable prototype: the controller electronics, low-noise tunneling-amplifier hardware, custom PCBs, scanner mechanics, piezo elements, and battery power system. The itemized grant-funded parts total **$599**; the full list is available in [BOM.csv](BOM.csv).

This funding directly unlocks the most important project risk: integrating the analog front end, piezo scanner, mechanical head, and control software into one working system.

## What is being built

The proposed STM has four coordinated subsystems:

| Subsystem | Role in the instrument |
| --- | --- |
| **Controller** | An ESP32-based control board provides digital coordination, DAC outputs, ADC acquisition, and serial communication with the desktop application. |
| **Piezo scanner** | A quadrant piezo scanner receives differential drive signals to position the tip in X, Y, and Z. |
| **Tunneling preamplifier** | A shielded, high-impedance transimpedance front end converts the extremely small tunneling current into a measurable voltage. |
| **Control software** | The Python application exposes manual controls, PID parameters, scan configuration, live plots, and data export. |

![System architecture: controller, piezo driver, scanner, preamplifier, bias, and sample](docs/images/system-architecture.png)

*Figure 1. System-level signal flow. The controller drives the piezo scanner, the preamplifier measures tunneling current, and the sample receives a controllable bias.*

## How it works

An STM measures a quantum-mechanical tunneling current rather than using lenses or light. The tip is approached toward a conductive surface until the gap is small enough for a measurable current to flow. Because that current varies strongly with distance, the instrument can use it as an error signal while scanning.

Two core measurement modes guide the design:

- **Constant-current mode:** feedback continually adjusts the Z position to hold the tunneling current near a setpoint. The Z correction becomes a surface-height signal.
- **Constant-height mode:** the Z position remains fixed while current changes are recorded across the scan. This can be faster, but requires a sufficiently flat sample and careful operation.

The initial build will prioritize stable approach, safe current detection, repeatable raster motion, and closed-loop constant-current operation before attempting demanding high-resolution measurements.

## Electronics architecture

### Piezo drive and scanner motion

The scanner uses four quadrant electrodes. From the requested X, Y, and Z control values, the driver generates the differential combinations `Z + X`, `Z − X`, `Z + Y`, and `Z − Y`. Differential drive bends the piezo for lateral motion while the common Z component moves the tip toward or away from the sample.

The circuit reference below illustrates the four-channel, filtered analog drive topology. The precise parts and quantities selected for this build are listed in the [BOM](BOM.csv).

![Piezo driver schematic](docs/images/piezo-driver-schematic.png)

*Figure 2. Four-channel piezo-drive topology for combining X, Y, and Z commands into quadrant outputs.*

### Low-current measurement

The tunneling signal is expected to be extremely small, so measurement quality depends on the preamplifier, shielding, grounding, component cleanliness, and careful physical layout. The transimpedance stage uses a high-value feedback resistor to convert current into a voltage that the controller can acquire. The preamplifier is isolated in a shielding can, with its layout and power decoupling treated as first-order design concerns.

![Preamplifier schematic](docs/images/preamplifier-schematic.png)

*Figure 3. Transimpedance preamplifier reference: the high-value feedback path converts tunneling current into a usable output signal.*

## Software and scan workflow

The repository includes a Python desktop application and a serial-control layer for the planned hardware. Together they provide a practical interface for the build, debug, and scan phases:

- Set sample bias and X/Y/Z DAC values.
- Configure feedback gains (`Kp`, `Ki`, and `Kd`).
- Start a configurable raster scan and receive scan-line data over serial.
- View current-derived and Z-feedback images in the desktop interface.
- Save scan arrays and figures for later analysis.

Raster scanning sweeps the X axis with a triangular waveform while incrementing Y with a ramp. This traces adjacent lines across the sample and builds a two-dimensional data set.

![Raster scan trajectory and drive waveforms](docs/images/raster-scan.png)

*Figure 4. Raster scan concept: a triangular X sweep paired with a monotonic Y ramp covers the sample line by line.*

## Build and validation plan

The project will progress through measurable stages rather than treating final imaging as a single all-or-nothing milestone.

1. **Assemble and verify the electronics** — populate the controller and preamplifier boards; verify supply rails, serial communication, DAC response, and ADC acquisition.
2. **Build the scan head** — prepare the piezo quadrant scanner, bond leads, mount the tip, and complete the grounded mechanical assembly.
3. **Characterize motion and noise** — confirm independent X/Y/Z response, evaluate preamplifier stability and noise, and establish a safe approach procedure.
4. **Close the feedback loop** — tune constant-current control and verify stable retraction on a conductive test sample.
5. **Acquire and document scans** — collect repeatable raster data, publish raw outputs and analysis, and refine the design from measured performance.

## Expected outcomes

With the requested funding, the immediate deliverable is a complete, testable STM prototype and an open record of its engineering process. The project will produce:

- A documented bill of materials and circuit-level design references.
- A working controller-to-desktop communication path.
- A piezo scanning head and low-current measurement chain ready for characterization.
- Reproducible scan data and validation notes as results are obtained.

Success will be reported honestly and incrementally: electronics bring-up, stable tunneling detection, closed-loop control, repeatable scan data, and finally resolution performance. No atomic-resolution result is assumed in advance.

## Repository guide

| File | Description |
| --- | --- |
| [BOM.csv](BOM.csv) | Grant-funded component list and $600 funding request. |
| [stm_app.py](stm_app.py) | Tkinter desktop interface for controls, plotting, and scan-data export. |
| [stm_control.py](stm_control.py) | Serial protocol client and scan-data handling layer. |
| [docs/images](docs/images) | System and circuit diagrams used in this README. |

## Software setup

The application is intended to communicate with the custom controller hardware over a serial connection. Hardware operation is not available until the prototype is assembled and firmware is loaded.

For interface development, install the Python dependencies and launch the app:

```bash
python3 -m pip install numpy matplotlib pyserial
python3 stm_app.py
```

## Safety and responsible testing

This project uses sensitive analog electronics, sharp conductive tips, and dual-rail power. Assembly and testing should begin with power-off continuity checks, current-limited bench verification, and deliberate grounding/shielding practices. The STM is an experimental research and education instrument—not a finished commercial product or a diagnostic device.

---

**A small, transparent instrument for learning how quantum-scale measurement is actually built.**
