# Software

- `stm_app.py` is the desktop control app. It sets scan parameters, shows a live image, saves scans, and sends serial commands.
- `stm_control.py` contains the serial-control helpers used by the app.

Run the app from the repository root:

```bash
python3 -m pip install numpy matplotlib pyserial
python3 Software/stm_app.py
```

The app needs the matching controller firmware and connected STM hardware before it can scan.
