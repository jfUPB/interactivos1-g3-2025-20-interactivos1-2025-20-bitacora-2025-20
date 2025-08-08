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

**Vectores de prueba**

**1) Aumentar el tiempo de configuración:**

- Condición inicial: El sistema inicia en el estado CONFIG con un tiempo de 20 segundos.
- Evento generado: Presionar el botón A.
- Resultado esperado: El tiempo aumenta a 21 segundos y la pantalla muestra el número 21 luego de presionar una sola vez el botón A.
- Resultado obtenido: El tiempo aumento de 20 a 21 segundos luego de presionar el botón; este número también se ve en la pantalla.

**2) Disminuir el tiempo de configuración:**

- Condición inicial: El sistema inicia en el estado CONFIG con un tiempo de 20 segundos.
- Evento generado: Presionar el botón B.
- Resultado esperado: El tiempo disminuye a 19 segundos y la pantalla muestra el número 19 luego de presionar una sola vez el botón B.
- Resultado obtenido: El tiempo disminuyó de 20 a 19 segundos luego de presionar el botón; este número también se ve en la pantalla.

**3) No disminuir el tiempo por debajo del mínimo**

- Condición inicial: El sistema inicia en el estado CONFIG con un tiempo de 10 segundos.
- Evento generado: Presionar varias veces el botón B.
- Resultado esperado: El tiempo no baja de 10 segundos.
- Resultado obtenido: No se muestra cambio alguno a un número menor a 10.

**4) Iniciar cuenta regresiva:**

- Condición inicial: El sistema está en el estado CONFIG con tiempo de 20 segundos
- Evento generado: Agitar ek micro:bit (shake).
- Resultado esperado: Se inicia la cuenta regresiva.
- Resultado obtenido: Aparece cada segundo en pantalla de forma descendente hasta llegar a 0, luego "explota".






