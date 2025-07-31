# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01
**Describe detalladamente cómo funciona este ejemplo.**  
El programa usa la pantalla LED del micro:bit para controlar dos píxeles, dependiendo de los tiempos se prenderá uno u otro, los tiempos serían 1 segundo y 0,5 segundos.   

**¿Cuáles son los estados en el programa?**  
Init (Es el estado inicial) y WaitTimeout (espera para encender o apagar).  

**¿Cuáles son los eventos/inputs en el programa?**  
Update  

¿Cuáles son las acciones en el programa?  
* Prender el led  
* Encender y apagar cuando pasa el tiempo
* El tiempo
 
## Actividad 02
Implementando un semáforo con máquina de estados.  
Implementemos juntos un semáforo simple (rojo, amarillo, verde) utilizando una máquina de estados en Micropython. Representaremos cada color del semáforo con un LED del display del micro:bit.  

**Escribe el código que soluciona este problema en tu bitácora.**
``` js
from microbit import *
import utime

class Semaforo:
    def __init__(self):
        self.state = "Init"
        self.startTime = 0
        self.interval = 0
        self.color = "RED"  

    def mostrar_color(self):
        display.clear()
        if self.color == "RED":
            display.set_pixel(2, 0, 9)
            self.interval = 3000
        elif self.color == "GREEN":
            display.set_pixel(2, 4, 9)
            self.interval = 3000
        elif self.color == "YELLOW":
            display.set_pixel(2, 2, 9)
            self.interval = 1000

    def cambiar_color(self):
        if self.color == "RED":
            self.color = "GREEN"
        elif self.color == "GREEN":
            self.color = "YELLOW"
        elif self.color == "YELLOW":
            self.color = "RED"

    def update(self):
        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            self.mostrar_color()

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval:
                self.cambiar_color()
                # Reiniciar tiempo y mostrar el nuevo color
                self.startTime = utime.ticks_ms()
                self.mostrar_color()

semaforo = Semaforo()

while True:
    semaforo.update()
```

**Identifica los estados, eventos y acciones en tu código.**  
* **Estados:** Init y WaitTimeou, ademas de estar "YELLOW" "RED" "GREEN"  
* **Eventos:** El evento sería un Timeout el cual se encarga de que el tiempo asignado se cumpla.  
* **Acciones en tu código:** Limpiar pantalla, encender el LED correspondiente al color, configurar el tiempo de espera y cambiar al siguiente color cuando el tiempo se cumpla.  
