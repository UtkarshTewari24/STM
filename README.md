# Build a scanning tunneling microscope

A scanning tunneling microscope (STM) is a machine that moves a very sharp metal tip across a conductive surface and measures a tiny electric current to learn about that surface.

> **Current status:** this is a planned build. The software, parts list, and circuit references are here; the physical microscope still needs to be assembled and tested. Atomic-resolution images are the goal, not a result this project has already achieved.

![Planned STM signal path](docs/images/system-architecture.png)

*The planned signal path: the controller moves the piezo scanner, the preamplifier measures current, and the sample receives a small bias voltage.*

## What an STM does

An STM does not use a camera or a lens. Instead, it brings a metal tip extremely close to a conductive sample. Electrons can cross the tiny gap through a quantum effect called **tunneling**. The current is very sensitive to distance: move the tip a little closer and the current changes a lot.

That sensitivity lets the microscope scan a surface one line at a time. It can either:

- keep the current steady by moving the tip up and down (**constant-current mode**), or
- keep the tip height steady and record changes in current (**constant-height mode**).

## What this build is made of

- **Controller board:** an ESP32 communicates with the computer, sends position values to DAC boards, and receives the measured signal through an ADC board.
- **Piezo scanner:** a piezo disk is cut into four sections. Applying different voltages makes it bend, moving the tip in X, Y, and Z.
- **Preamplifier:** a shielded circuit turns the extremely small tunneling current into a voltage the controller can read.
- **Scan head:** a kinematic mount, cage hardware, tip, and sample holder keep the tip and sample aligned.
- **Desktop program:** the included Python app has controls for bias, position, feedback settings, live plots, and saving scan data.

## Parts and cost

The exact part list, quantities, current budget estimates, and purchase links are in [BOM.csv](BOM.csv).

The estimated parts subtotal is **exactly $600 before tax and shipping**. The budget includes a $25 EMI and grounding kit—copper foil tape, ferrite clamps, and shielded hookup wire—to help keep electrical noise away from the sensitive preamplifier. The budget was reduced elsewhere by using a $2.95 Micro‑USB cable, a $6 terminal/header set, and a $1.05 PL2303TA serial cable instead of the earlier higher estimates.

## Build plan

This is the intended order of work. It keeps the tricky, sensitive parts until after the basic electronics are behaving.

1. **Build and test the controller board.** Check the power rails first. Then verify the ESP32 can talk to the computer and that each DAC and ADC channel responds.
2. **Build the preamplifier.** Keep this circuit clean, short, and inside its shielding can. It is responsible for measuring the tiny current at the tip.
3. **Make the scanner.** Cut the piezo disk into quadrants, attach the pins and flexible wires, and test X, Y, and Z movement one axis at a time.
4. **Assemble the scan head.** Mount the tip and sample in the kinematic mount and cage frame. Ground the metal shielding and keep the preamplifier close to the tip.
5. **Start with safe tests.** Verify motion and noise before bringing the tip close to a sample. Then tune the feedback loop and try a slow scan.

## How the scanner moves

The driver combines the three requested movements into four outputs: `Z + X`, `Z − X`, `Z + Y`, and `Z − Y`. Sending different voltages to opposite piezo quadrants bends the disk sideways. Adding the same Z value to every quadrant moves the tip toward or away from the sample.

![Piezo-driver circuit reference](docs/images/piezo-driver-schematic.png)

*Piezo-driver circuit reference. Four filtered amplifier channels create the quadrant voltages.*

## How the current is measured

The current from the tip is too small for the microcontroller to read directly. The preamplifier uses a very large feedback resistor to turn that current into a voltage. Shielding, short connections, and clean assembly matter here because electrical noise can hide the signal.

![Preamplifier circuit reference](docs/images/preamplifier-schematic.png)

*Preamplifier circuit reference. The high-value feedback path converts tunneling current into a readable voltage.*

## How a scan is made

The tip moves back and forth in X while Y moves forward a small step after each line. This is called a raster scan, and it is the same basic pattern used to draw an image line by line.

![Raster scan pattern](docs/images/raster-scan.png)

*A triangular X movement plus a Y ramp covers the sample line by line.*

## Run the desktop program

The program is useful for developing the interface now, but it needs the finished controller hardware and matching firmware before it can control a real microscope.

```bash
git clone https://github.com/UtkarshTewari24/STM.git
cd STM
python3 -m pip install numpy matplotlib pyserial
python3 stm_app.py
```

The app can set X/Y/Z values, set PID feedback gains, start a raster scan, display the current and Z-feedback data, and save scan arrays and figures.

## Important limitations

- The physical STM has not been assembled or validated yet.
- The controller firmware and PCB fabrication files are still needed before another person can reproduce the whole instrument from this repository alone.
- A first-time build should begin with low-voltage checks and current-limited power, not with an approaching tip.
- This is an experimental educational project, not a commercial or diagnostic instrument.

## Repository guide

| File | What it is |
| --- | --- |
| [BOM.csv](BOM.csv) | Parts list with quantities, budget estimates, and purchase links. |
| [stm_app.py](stm_app.py) | Desktop control window and plot display. |
| [stm_control.py](stm_control.py) | Serial commands and incoming scan-data handling. |
| [docs/images](docs/images) | The images shown above. |

## Safety

This build uses sharp metal wire, sensitive parts, and positive and negative power supplies. Check wiring with the power off, use a current-limited supply for the first test, and take care not to short the preamplifier input. Stop if anything gets hot or a power rail is not correct.
