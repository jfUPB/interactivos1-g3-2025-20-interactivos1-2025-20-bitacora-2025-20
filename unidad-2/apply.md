# Unidad 2


## 🛠 Fase: Apply


## :tools: Fase: Apply

<img width="1187" height="943" alt="image" src="https://github.com/user-attachments/assets/e1b97933-45c2-4052-aaf1-45beaf2afabb" />

```.py
from microbit import *
import utime

STATE_INT = 0
STATE_CONFIG = 1
STATE_ARMED = 2
STATE_KABOOM = 3

state = STATE_INT
time_value = 20
time_left = time_value
start_time = 0

def show_time_left():
    display.show(str(time_left))

while True:

    if state == STATE_INT:
        start_time = 0
        time_value = 20
        display.show(str(time_value))
        state = STATE_CONFIG

    elif state == STATE_CONFIG:
        if button_a.was_pressed():
            if time_value < 60:
                time_value += 1
            show_time_left()

        if button_b.was_pressed():
            if time_value > 10:
                time_value -= 1
            show_time_left()

        if accelerometer.was_gesture('shake'):
            show_time_left()
            start_time = utime.ticks_ms()
            time_left = time_value
            state = STATE_ARMED

        if pin_logo.is_touched():
            time_value = 20
            show_time_left()

    elif state == STATE_ARMED:
        current_time = utime.ticks_ms()
        if utime.ticks_diff(current_time, start_time) >= 1000:
            start_time = current_time
            time_left -= 1
            show_time_left()

            if time_left == 0:
                state = STATE_KABOOM

    elif state == STATE_KABOOM:
        display.show(Image.SKULL)
 
        if button_a.is_pressed() and button_b.is_pressed():
            state = STATE_INT
```

