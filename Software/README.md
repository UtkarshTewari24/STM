# Software

- `stm_app.py` is the PC app.
- `stm_control.py` sends serial commands to the controller.
- `stm_control_test.py` is a small serial test.

Run the app from the repository root:

```bash
python3 -m pip install numpy matplotlib pyserial
python3 Software/stm_app.py
```

It needs the controller firmware and STM hardware connected before it can scan.
