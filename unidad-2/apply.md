# Unidad 2


## 🛠 Fase: Apply


### Actividad 4.  
![Diagrama de estados](https://github.com/user-attachments/assets/56efb5f4-cc53-4f4a-a63d-79a91c50e4cb)  

### Actividad 5.  
```javascript
from microbit import *
import music
import utime

# ------------------------------
# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

# ------------------------------
# Variables globales
current_state = STATE_CONFIG
timer_value = 20  # Tiempo inicial en segundos
min_time = 10
max_time = 60
last_time = utime.ticks_ms()
countdown_time = 0 
# ------------------------------
# Funciones auxiliares

def show_time(t):
    display.show(str(t)[-1]) if t < 10 else display.scroll(str(t))

def explode():
    display.show(Image.SKULL)
    music.play(music.WAWAWAWAA)

# ------------------------------
# Bucle principal
while True:
    if current_state == STATE_CONFIG:
         # Modo configuración
        show_time(timer_value)
        
        # Aumentar tiempo
        if button_a.is_pressed():
            timer_value += 1
            if timer_value > max_time:
                timer_value = max_time
            show_time(timer_value)
            sleep(300)

        # Disminuir tiempo
        if button_b.is_pressed():
            timer_value -= 1
            if timer_value < min_time:
                timer_value = min_time
            show_time(timer_value)
            sleep(300)

        # Armar bomba con gesto shake
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            countdown_time = timer_value
            last_time = utime.ticks_ms()
            display.clear()

    elif current_state == STATE_ARMED:
        current_time = utime.ticks_ms()
        if utime.ticks_diff(current_time, last_time) >= 1000:
            countdown_time -= 1
            last_time = current_time
            if countdown_time >= 0:
                show_time(countdown_time)
        
        # Si llega a cero, explota
        if countdown_time == 0:
            explode()
            current_state = STATE_EXPLODED

    elif current_state == STATE_EXPLODED:
        # Esperar toque para reiniciar
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            timer_value = 20
            display.clear()
```
