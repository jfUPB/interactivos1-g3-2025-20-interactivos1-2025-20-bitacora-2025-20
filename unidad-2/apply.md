# Unidad 2


## 🛠 Fase: Apply

### Actividad 04 - 06/08/2025

**Diagrama máquina de estados:**

|     Estado     |         Evento (input)         |               Acción               |   Estado siguiente   |
|:--------------:|:------------------------------:|:----------------------------------:|:--------------------:|
|    `CONFIG`    |    Botón A (UP)                | Aumentar tiempo inicial (+1 s)     |       `CONFIG`       |
|    `CONFIG`    |    Botón B (DOWN)              | Disminuir tiempo inicial (–1 s)    |       `CONFIG`       |
|    `CONFIG`    |    Acelerómetro: *shake*       | Iniciar cuenta regresiva           |      `COUNTDOWN`     |
|  `COUNTDOWN`   | 1 segundo transcurrido         | Restar 1 segundo al contador       | `COUNTDOWN` o `EXPLODE` |
|  `COUNTDOWN`   | Tiempo == 0                    | Mostrar calavera y activar buzzer  |      `EXPLODE`       |
|  `COUNTDOWN`   | Toque en botón touch           | Detener bomba, volver a configuración |    `CONFIG`      |
|   `EXPLODE`    | Toque en botón touch           | Silenciar explosión, volver a configuración | `CONFIG`    |


## Actividad 05

**Código bomba temporizada**

```python
from microbit import *
import utime

# Estados
CONFIG = 0
COUNTDOWN = 1
EXPLODE = 2

# Estado inicial
state = CONFIG
countdown_time = 20
last_tick = utime.ticks_ms()

# Límites del temporizador
MIN_TIME = 10
MAX_TIME = 60

def show_number(n):
    display.show(str(n))  # Mostrar número directamente en pantalla

def explode():
    for _ in range(5):
        display.show(Image.SKULL)
        sleep(200)
        display.clear()
        sleep(200)

while True:
    if state == CONFIG:
        display.show(Image.ASLEEP)

        if button_a.was_pressed() and countdown_time < MAX_TIME:
            countdown_time += 1
            show_number(countdown_time)
            sleep(1000)  # Espera un segundo para que se vea el número

        elif button_b.was_pressed() and countdown_time > MIN_TIME:
            countdown_time -= 1
            show_number(countdown_time)
            sleep(1000)

        elif accelerometer.was_gesture('shake'):
            state = COUNTDOWN
            show_number(countdown_time)  # Mostrar inmediatamente el número actual
            last_tick = utime.ticks_ms()

    elif state == COUNTDOWN:
        now = utime.ticks_ms()
        if utime.ticks_diff(now, last_tick) >= 1000:
            countdown_time -= 1
            last_tick = now
            if countdown_time > 0:
                show_number(countdown_time)
            else:
                state = EXPLODE
                explode()

        if pin_logo.is_touched():
            state = CONFIG
            countdown_time = 20

    elif state == EXPLODE:
        display.show(Image.SKULL)
        if pin_logo.is_touched():
            state = CONFIG
            countdown_time = 20
```







            state = CONFIG
            countdown_time = 20
```
