# Firmware

This is the controller project. It opens in PlatformIO and is set up for a Teensy 4.1.

`src/main.cpp` handles serial commands. `lib/` has the ADC, DAC, and stepper code. `platformio.ini` lists the libraries needed to build it.

The BOM currently lists an ESP32, so this firmware needs to be ported and tested before it is used with that board.
