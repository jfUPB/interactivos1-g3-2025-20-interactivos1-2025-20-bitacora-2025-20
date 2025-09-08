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

Si hablamos de inicio a final, lo más esencial es que importa la librería utime para que se pueda hacer uso de algunas líneas para medir el tiempo que se determine, siguiendo con el código, se crea una clase Pixel dentro la cual se almacena un constructor en donde se almacenan las coordenadas X y Y (self.pixelX y self.pixelY), su brillo inicial (self.pixelState) y un intervalo de prendido y apagado (o parpadeo, tambien referido como self.interval). Cuando pasamos al update(), este actua con dos estados. Siendo Init el de primera vez y WaitTimeout, y ya que hablamos de la primera vez, este entra en un if (ubicado en la linea 15) en donde de paso se guada el tiempo, apartir de la segunda llamda, con display.set_pixel se dibuja en el micro:bit el LED. Asi mismo, hay que destacar que la linea 20 me permite saber si ingreso a ese dato, mientras que al mismo tiempo la linea 21 dice que ya ha pasado un cierto intervalo de tiempo.

En el ejemplo, el programa lo que hace es que desde el MicroBit prende ciertos leds del mismo con un cooldown.
   
*2. ¿Cuáles son los estados en el programa?*

El programa en cuestión cuenta con dos estados: Init y Timeout, siendo Init un seudoestado, dado a que este solo se utiliza al inicio del programa para establecer tiempo de referencia, la posición/coordenadas de los pixeles y mostrar el estado sus estados, mientras que WaitTimeout se encarga de los ciclos de espera. 

*3. ¿Cuáles son los eventos/inputs en el programa?*

Eventos: El evento que logro apreciar prbando el codigo en micro:bit editor, es la espera de prendida de cada pixel posicionado, esto gracias a la linea:

```Python 
utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:"
``` 

En donde utime.ticks_diff mide el tiempo, y cuando pasa el tiempo del interval, se apaga y prende los pixeles.

Inputs: Dado a los ejercicios de la primera unidad, me habia acostumbrado a tomar como base los inputs fisicos del micro:bit, pero dado a que no se esta haciendo uso de un micro:bit fisico (sino del digital que ofrece el editor), y de lo que recuerdo que es un Input (si es que mi cerebro no me juega una mala jugada), ya que lo que recuerdo es que la información de entrada, vendrian siendo los datos asignados a los objetos pixel1 y pixel2 de la clase Pixel.
  
*4. ¿Cuáles son las acciones en el programa?*

### Actividad 2

### Implementando un semáforo con máquina de estados
#### Implementemos juntos un semáforo simple (rojo, amarillo, verde) utilizando una máquina de estados en Micropython. Representaremos cada color del semáforo con un LED del display del micro:bit.

*1. Escribe el código que soluciona este problema en tu bitácora.*

``` Python
from microbit import *
import utime

STATE_INIT = 0
STATE_RED = 1
STATE_GREEN = 2
STATE_YELLOW = 3

TIME_IN_RED = 3000
TIME_IN_GREEN = 2000
TIME_IN_YELLOW = 1000

current_state = STATE_INIT
start_time = 0
interval = 0

while True:

    if current_state == STATE_INIT:
        display.set_pixel (2, 0, 9)
        start_time = utime.ticks_ms()
        interval = TIME_IN_RED
        current_state = STATE_RED

    elif current_state == STATE_RED:
        if utime. ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.clear()    
            display.set_pixel(2, 4, 9)
            start_time = utime.ticks_ms()
            interval = TIME_IN_GREEN
            current_state = STATE_GREEN

    elif current_state == STATE_GREEN:
        if utime. ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.clear()    
            display.set_pixel(2, 2, 9)
            start_time = utime.ticks_ms()
            interval = TIME_IN_YELLOW
            current_state = STATE_YELLOW

    elif current_state == STATE_YELLOW:
        if utime. ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.clear()    
            display.set_pixel(2, 0, 9)
            start_time = utime.ticks_ms()
            interval = TIME_IN_RED
            current_state = STATE_RED
```
Voy a hacer una aclaración. Aunque esta parte de la actividad 2 solo pide el codigo (a juzgar por el enunciado) prefiero mencionar un comentario aqui, dado que tuve demasiadas dificultades para hacer esta parte, y tomando como consejo lo que menciono el profesor sobre que la actividad 3 puede servir para entender mejor esta segunda actividad, use de referencia el codigo de esa actividad para comprobar si era posible simular el semaforo, lo cual sirvio debido a que previamente me estaba complicando demasiado, incluso me atrevo a decir que estuve como tres horas mirando en el editor de micro:bit si se podia cambiar el color de los leds a amarillo y verde para simular un semaforo de verdad, pero no, no es posible, o por lo menos dentro de lo que estuve revisando no se puede.

*2. Identifica los estados, eventos y acciones en tu código.*

Estados: En este caso aunque coloque cuatro estados (si contamos el de INIT que es con el que comienza el micro:bit), aunque si no tomamos en cuenta INIT, seria cada uno de los colores del semaforo, es decir, tres estados. El primero es el estado rojo, es decir, el de freno. Aqui el usuario esta esperando a que ese color pase a verde (o bueno, en este caso, que la luz del LED cambie de posición porque a fin de cuentas ahi todos los LEDs son rojos xD) y debe esperar un determinado tiempo, en el codigo coloque 3000 dado que en la vida real los semaforos rojos suelen ser máx durareros. Luego cuando cambia a verde el usuario debe esperar a que pase algo más, teniendo un tiempo intermedio de 2000, y cuando cambia a amarillo, ahi es donde se debe esperar a que pase un poco más de tiempo para que pase a rojo, y cuando cambia a rojo el proceso que mencione previamente se repite. Para no ser más rebuscado de lo que ya estoy siendo, se puede resumir que los estados en este programa son los tiempos de espera de cada LED.

Eventos: En el programa solo hay un evento presente, siendo este, la espera entre cambios de LEDs, o en otros terminos, la espera de cada cambio de color.

Acciones: Las unicas acciones que tiene el programa en cuestión, es el de apagar y prender cada LED transcurrido el tiempo que se le haya asignado a cada uno de los LEDS, no es necesario oprimir algun botón como 'a' o 'b' del micro:bit, agitarlo u otra acción diferente.

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

Se dice que realiza varias maneras tareas porque el programa no solo cambia de manera consecutiva las caras con un contador, sino que tambien el usuario puede oprimir el botón 'a' para hacer manualmente el cambio de rostros en lugar de esperar el tiempo entre rostros.

*2. Identifica los estados, eventos y acciones en el programa.*

Estados: Aqui hay tres estados, nuevamente, si no tomamos en cuenta STATE_INIT, vendrian siendo las tres caras que el micro:bit enseña: Happy, Smile y Sad (STATE_HAPPY, STATE_SMILE, STATE_SAD).

Eventos: Por lo que puedo apreciar, el evento que esta presente es la espera de tiempo en el cambio de cada rostro.

Acción: Además del apagado y prendido de los LEDs que corresponde a cada rostro, por parte del mismo usuario, cuando se oprime el botón 'a' para cambiar los rostros, tambien se esta llevando a cabo una acción.

*3. Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.*

Voy a intentar responder esto en base a lo que entiendo de este punto, porque no recuerdo si habia que modificar el codigo de la actividad o describirlo en base al codigo ofrecido, en este caso, vot a describirlo en base a lo que hay.

Vector 1:

Condicion Inicial: Cara Happy (Image.HAPPY).

Evento: Cambio de rostro al oprimir el botón 'a'.

Resultado esperado: happy -> si presiono 'a' (if button_a.was_pressed()), mostrar sad (image.SAD).

Resultado obtenido: El sistema pasa el vector de prueba

Vector 2:

Condicion Inicial: Cara Sad (Image.SAD).

Evento: Cambio de rostro al transcurrir el tiempo asignado en el intervalo.

Resultado esperado: sad, si paso 2 segundos (2000) -> mostrar happy (image.HAPPY)

Resultado obtenido: El sistema pasa el vector de prueba también.

PD: No ironicamente cronometre el tiempo para verificar que si se cumpliera.

Vector 3:

Condicion Inicial: Cara Smile (Image.SMILE).

Evento: Al igual que el primer vector, cambio de rostro al oprimir el botón 'a'.

Resultado esperado: happy -> si presiono 'a' (if button_a.was_pressed()), mostrar happy (image.HAPPY).

Resultado obtenido: El sistema nuevamente pasa el vector de prueba.


