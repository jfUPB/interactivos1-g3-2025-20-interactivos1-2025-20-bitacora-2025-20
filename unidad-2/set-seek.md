# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

### Analizando un programa con una máquina de estados simple

#### Analicemos juntos el siguiente código identificando estados, eventos y acciones. Responde las preguntas planteadas.

```Python
from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()

```

##### Escribe en tu bitácora lo siguiente:

*1. Describe detalladamente cómo funciona este ejemplo.*

En el ejemplo, el programa lo que hace es que desde el MicroBit prende ciertos leds del mismo con un cooldown.
   
*2. ¿Cuáles son los estados en el programa?*

Los dos estados con los que cuenta el programa son Init y TimeWithout

*3. ¿Cuáles son los eventos/inputs en el programa?*

Eventos: 

Inputs: 
  
*4. ¿Cuáles son las acciones en el programa?*

### Actividad 2

### Implementando un semáforo con máquina de estados
#### Implementemos juntos un semáforo simple (rojo, amarillo, verde) utilizando una máquina de estados en Micropython. Representaremos cada color del semáforo con un LED del display del micro:bit.

*1. Escribe el código que soluciona este problema en tu bitácora.*



*2. Identifica los estados, eventos y acciones en tu código.*

Estados:

Eventos:

Acciones

### Actividad 3

#### Controlando la pantalla con una máquina de estados y concurrencia

```Python

from microbit import *
import utime

STATE_INIT = 0
STATE_HAPPY = 1
STATE_SMILE = 2
STATE_SAD = 3

HAPPY_INTERVAL = 1500
SMILE_INTERVAL = 1000
SAD_INTERVAL = 2000

current_state = STATE_INIT
start_time = 0
interval = 0

while True:
    # pseudoestado STATE_INIT
    if current_state == STATE_INIT:
        display.show(Image.HAPPY)
        start_time = utime.ticks_ms()
        interval = HAPPY_INTERVAL
        current_state = STATE_HAPPY
    elif current_state == STATE_HAPPY:
        if button_a.was_pressed():
            # Acciones para el evento
            display.show(Image.SAD)
            # Acciones de entrada para el siguiente estado
            start_time = utime.ticks_ms()
            interval = SAD_INTERVAL
            current_state = STATE_SAD
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            # Acciones para el evento
            display.show(Image.SMILE)
            # Acciones de entrada para el siguiente estado
            start_time = utime.ticks_ms()
            interval = SMILE_INTERVAL
            current_state = STATE_SMILE
    elif current_state == STATE_SMILE:
        if button_a.was_pressed():
            display.show(Image.HAPPY)
            start_time = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            current_state = STATE_HAPPY
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.show(Image.SAD)
            start_time = utime.ticks_ms()
            interval = SAD_INTERVAL
           current_state = STATE_SAD
    elif current_state == STATE_SAD:
        if button_a.was_pressed():
            display.show(Image.SMILE)
            start_time = utime.ticks_ms()
            interval = SMILE_INTERVAL
            current_state = STATE_SMILE
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.show(Image.HAPPY)
            start_time = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            current_state = STATE_HAPPY
```
#### En tu bitácora:

*1. Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.*

Por que no solo el programa cambia de manera consecutiva las caras con un contador, sino que tambien el usuario puede oprimr el boton 'a' para hacer manualmente el cambio de rostros.

*2. Identifica los estados, eventos y acciones en el programa.*

Estados: Happy, Smile, Sad

Eventos: El coold down de tiempo entre los cambios de rostros

Acción: Oprimir el botón 'a' para cambiar

*3. Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.*

