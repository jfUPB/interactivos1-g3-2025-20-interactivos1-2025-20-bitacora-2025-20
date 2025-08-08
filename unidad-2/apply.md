# Unidad 2


## 🛠 Fase: Apply

### Actividad 4:  
<img width="540" height="272" alt="image" src="https://github.com/user-attachments/assets/8711c931-4007-4848-98fc-b79a98c75d73" />

_____________________________________________________________________

### Actividad 5:  

from microbit import *
import utime

STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

time_set = 20   # Tiempo inicial en segundos
min_time = 10
max_time = 60

current_state = STATE_CONFIG
start_time = 0
remaining_time = time_set

while True:
    if current_state == STATE_CONFIG:
        display.show(str(time_set))
        
        if button_a.was_pressed():
            if time_set < max_time:
                time_set += 1
        
        if button_b.was_pressed():
            if time_set > min_time:
                time_set -= 1
        
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            remaining_time = time_set
            start_time = utime.ticks_ms()
    
    elif current_state == STATE_ARMED:
        display.show(str(remaining_time))
        
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= 1000:
            remaining_time -= 1
            start_time = utime.ticks_ms()
        
        if remaining_time <= 0:
            current_state = STATE_EXPLOSION
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
    
    elif current_state == STATE_EXPLOSION:
        for i in range(5):
            display.show(Image.SKULL)
            utime.sleep_ms(200)
            display.clear()
            utime.sleep_ms(200)
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
