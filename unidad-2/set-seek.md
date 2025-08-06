# Unidad 2

## 🔎 Fase: Set + Seek

### ACTIVIDAD 1

CODIGO:

```
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
````
## ¿Como funciona este ejemplo?
R/ Basicamente ese programa crea una clase llamada Pixel, que lo que hace es que representa un Led individual en la pantalla del microbit. Cada Pixel tiene su propia posición, y su propio temporizador y recuerda si está encendido o apagado. Cuando se crean dos de estos píxeles el programa consigue que dos luces distintas parpadeen al mismo tiempo, pero cada una con su propio ritmo, osea que no dependen la una de la otra. Dentro de la clase hay un método llamado update(). Lo que hace es que se encarga de revisar si ya pasó cierto tiempo desde el último cambio, y si si paso cambia el estado del Led.

## ¿Cuáles son los estados en el programa?
R// El programa funciona con dos estados principales para cada LED. 
"Init" es como cuando el LED se está preparando, entonces aquí se configura el tiempo que debe esperar antes de cambiar y se muestra si empieza encendido o apagado. El otro estado es "WaitTimeout", en este estado, el LED simplemente espera a que pase el tiempo que se le puso, entonces una vez que ese tiempo termina, cambia su estado.

## ¿Cuáles son los eventos/inputs en el programa?
R// En este programa los cambios de estado ocurren automáticamente sin necesidad de interacción del usuario. Empezando el programa, el método update() hace que cada LED pase de su estado inicial a uno de espera. Luego cuando transcurre el tiempo (el tiempo que se asigno), el LED cambia de encendido a apagado o al reves y reinicia su temporizador, todo el proceso depende únicamente del paso del tiempo, no de entradas externas.

## ¿Cuáles son las acciones en el programa?
R// Cuando ocurre un cambio dentro del programa, este realiza varias acciones de forma automática. Por ejemplo, primero reinicia el temporizador para empezar a contar el tiempo nuevamente, despues actualiza el estado del LED, indicando si debe esperar o cambiar. Y luego enciende o apaga el LED en su lugar correspondiente dentro de la matriz del microbit. Y por ultimo ajusta el brillo del LED, poniéndolo en 9 si está encendido o en 0 si está apagado para que se vea claramente el parpadeo.

### ACTIVIDAD 2

Codigo que soluciona el problema:

````

from microbit import *
import utime

class Semaforo:
    def __init__(self, x):
        self.x = x
        self.estado = 0  # 0: rojo, 1: amarillo, 2: verde
        self.tiempos = [2000, 1000, 2000]  # ms para cada color
        self.ultimo_cambio = utime.ticks_ms()
        self.dibujar()

    def dibujar(self):
        # Apaga todos
        for y in range(3):
            display.set_pixel(self.x, y, 0)
        # Enciende el color correspondiente
        if self.estado == 0:
            display.set_pixel(self.x, 0, 9)  # Rojo arriba
        elif self.estado == 1:
            display.set_pixel(self.x, 1, 9)  # Amarillo medio
        elif self.estado == 2:
            display.set_pixel(self.x, 2, 9)  # Verde abajo

    def update(self):
        ahora = utime.ticks_ms()
        if utime.ticks_diff(ahora, self.ultimo_cambio) > self.tiempos[self.estado]:
            self.estado = (self.estado + 1) % 3
            self.ultimo_cambio = ahora
            self.dibujar()

semaforo = Semaforo(2)  # Columna central

while True:
    semaforo.update()

````

Explicacion e identificación de estados, eventos y acciones:

R// En este codigo el estado representa en que color esta el semaforo en cada momento, en este codigo se guardo en la variable self.estado dentro de la clase Semaforo. 

````

self.estado = 0  # 0: rojo, 1: amarillo, 2: verde

````

El estado cambia para indicar el color actual del semáforo.

R// Ahora, el evento es lo que provoca que el estado cambie. En el codigo el evento seria el paso del tiempo, Cada vez que pasa el tiempo necesario para cada color , ocurre el evento de cambio de color. Esto se controla en el metodo update.

````

if utime.ticks_diff(ahora, self.ultimo_cambio) > self.tiempos[self.estado]:
    # Aquí ocurre el evento: ha pasado el tiempo necesario

````

Por ultimo, la acción es lo que hace el semáforo cuando ocurre el evento. En este caso la acción es cambiar al siguiente color y mostrarlo en la micro:bi

````

self.estado = (self.estado + 1) % 3  # Cambia al siguiente estado
self.ultimo_cambio = ahora           # Actualiza el tiempo del último cambio
self.dibujar()                       # Dibuja el nuevo color en la micro:bit

````








