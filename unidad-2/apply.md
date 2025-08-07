# Unidad 2


## 🛠 Fase: Apply

### Actividad 4

#### Diseño de la lógica de una bomba temporizada

Diseña la máquina de estados para solucionar el siguiente problema:

En un escape room se requiere construir una aplicación para controlar una bomba temporizada. El circuito de control de la bomba está compuesto por cuatro sensores, denominados UP (botón A), DOWN (botón B), touch (botón de touch) y ARMED (el gesto de shake de acelerómetro). Tiene dos actuadores o dispositivos de salida que serán un display (la pantalla de LEDs) y un speaker.

El controlador funciona así:

Inicia en modo de configuración, es decir, sin hacer cuenta regresiva aún, la bomba está desarmada. El valor inicial del conteo regresivo es de 20 segundos.

En el modo de configuración, los pulsadores UP y DOWN permiten aumentar o disminuir el tiempo inicial de la bomba.

El tiempo se puede programar entre 10 y 60 segundos con cambios de 1 segundo. No olvides usar utime.ticks_ms() para medir el tiempo. Además, 1 segundo equivale a 1000 milisegundos.

Hacer shake (ARMED) arma la bomba, es decir, inicia el conteo regresivo.

Una vez armada la bomba, comienza la cuenta regresiva que será visualizada en la pantalla de LED

La bomba explotará (speaker) cuando el tiempo llegue a cero.

Para volver a modo de configuración deberás tocar el botón touch.

*1. Construye un diagrama detallado de la máquina de estados, incluyendo estados, eventos, transiciones y acciones.*

### Actividad 6

Implementa el código para la bomba temporizada usando mycropython y el micro:bit, incluyendo la funcionalidad básica: configuración del tiempo, cuenta regresiva y detonación.
Reporta en un tu bitácora lo siguiente:

*1. El código que implementa la bomba temporizada.*

```Python

from microbit import *
import utime
import music

STATE_INIT       = 0
STATE_SETTINGS   = 1
STATE_COUNTDOWN  = 2
STATE_EXPLODED   = 3


current_state = STATE_INIT
time_set      = 20  
time_stand    = 20  
last_tick     = 0    

while True:
    if current_state == STATE_INIT:
        display.clear()
        time_set = 20
        display.show(str(time_set))
        current_state = STATE_SETTINGS    

    elif current_state == STATE_SETTINGS:
        display.show(str(time_set))   

        if button_a.was_pressed():
            music.play(['C4'])
            if time_set < 60:
                time_set += 1
            display.show(str(time_set))

        if button_b.was_pressed():
            music.play(['G3'])
            if time_set > 10:
                time_set -= 1
            display.show(str(time_set))

        if accelerometer.was_gesture('shake'):
            music.play(['C4:2'])
            time_stand = time_set
            last_tick  = utime.ticks_ms()
            current_state = STATE_COUNTDOWN

    elif current_state == STATE_COUNTDOWN:
        if utime.ticks_diff(utime.ticks_ms(), last_tick) >= 1000:
            time_stand -= 1
            last_tick = utime.ticks_ms()
            display.scroll(str(time_stand))
            music.play('C4:2')   

        if time_stand == 0:
            current_state = STATE_EXPLODED

    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        music.play(['A4:1','F4:1','C4:1'])
        
        if pin_logo.is_touched():
            current_state = STATE_SETTINGS
            display.clear()

```

   
*2. La definición de los vectores de prueba básicos que permiten verificar el correcto funcionamiento del programa.*



