Homemade Scanning Tunneling Microscope

Building a scanning tunneling microscope from scratch capable of atomic resolution imaging in air. Target sample: HOPG graphite.

How it works:
A sharp tungsten tip is brought within 1nm of a conductive surface. Electrons tunnel quantum mechanically across the gap producing a current that changes exponentially with distance. This sensitivity enables atomic resolution.

Components
- Electrochemically etched tungsten tip
- Piezo disk scanner for sub-angstrom motion
- Custom transimpedance preamp (OPA828, 100MΩ feedback)
- PID feedback loop on Teensy 4.1
- Bungee cord vibration isolation

Based on
Dan Berard's open source hobbyist STM: dberard.com/home-built-stm (not too much to go off of, i like a challenge :D)
