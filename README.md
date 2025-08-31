# 🎮 PygameController API

A Python API by **Steven Andrews II** that provides a **virtual port system** for handling multiple game controllers using Pygame.  
It abstracts raw controller inputs into a clean interface with support for **button mapping, axis handling, D-Pad, and rumble control**.

---

## ✨ Features

- 🔌 Virtual ports – attach multiple controllers dynamically  
- 🕹️ Button / axis abstraction – simple API to read states  
- ↔️ Stick deadzone handling – configurable per instance  
- 🎚️ Trigger + stick angle calculations  
- 🎮 Controller hot-plugging – automatically detects added/removed devices  
- 💥 Rumble control – per-port vibration with duration and strength  
- ⏱️ Timeout + activity state machine – auto-cleans inactive or disconnected controllers  

---

## 🚀 Installation

This API depends on **pygame**. Install it with:

```bash
pip install pygame
```

Clone this repo:

```bash
git clone https://github.com/StevenAndrews-II/PygameController
.git
cd PygameController
```

---

## 📖 Usage Example

```python

import pygame, math, time
from PyGC import CM

pygame.init()
pygame.joystick.init()

EXIT         = False
FPS_LOCK     = 60
FPS_INTERVAL = 1 / FPS_LOCK
delta_time   = 0
last_tick    = time.time()

# Create controller manager for 1 virtual port
cm = CM(pygame, math, number_of_ports=1, FPS_LOCK)

def APP():
    global EXIT

    # update the internal state machines 
    cm.update()

    # Example: check button A & B on port 0
    if cm.get_button(0, "A"):
        print("Button A pressed on port 0")

    if cm.get_button(0, "B"):
        print("Exiting program...")
        EXIT = True

    # Example: set rumble 
    if cm.get_button(0, "X"):
        cm.set_rumble(0, [0.5, 0.5, 2])  # 50% L_motor, R_motor, duration in seconds

    # Example: get stick angle + magnitude
    angle, mag = cm.get_stick_angle(0, "L_stick")
    if angle is not None:
        print(f"L-stick angle: {angle:.2f}°, magnitude: {mag:.2f}")

# Main loop, fps locked to 60
while not EXIT:
    tick = time.time()
    delta = tick - last_tick
    last_tick = tick

    delta_time += delta

    if delta_time >= FPS_INTERVAL:
        APP()
        delta_time -= FPS_INTERVAL


```



---

## 📚 API Reference

### Core Methods:
- `update()`                        – updates all state machines, handles events  
- `get_button(port_id, button)`     – get button state (supports remapping)  
- `get_axis(port_id, axis)`         – get trigger/stick values (supports remapping + inversion)
- `get_stick_angle(port_id, axis)`  – stick angle + magnitude (supports remapping + inversion)
- `get_dpad(port_id, pad_select)`   – D-Pad state (supports inversion )

### Management - internal:
- `__attach(port_id, joy_id)`                           – attach controller to port  
- `__detach(port_id)`                                   – detach controller from port  
- `__set_rumble(port_id, [L_motor, R_motor, duration])` – rumble control
- `__port_manager()`                                    – handles the virtual port assignments 
- `__input_handler()`                                   – updates the internal states for a controllers mapping
- `__time_out()`                                        – handles timeout state checking for controllers

---

## ⚙️ Configuration:

Default settings can be adjusted in `self.settings`:
- `time_out_`          – timeout for inactive controllers  
- `port_activity`      – port inactivity threshold  
- `port_read_deley`    – USB read delay  
- `stick_deadZone`     – deadzone for analog sticks  
- `trigger_deadZone`   – deadzone for triggers  

---

## 🛠️ Project Info

- Author: **Steven Andrews II**  
- License: MIT ( open source )  
- Year: 2025  
