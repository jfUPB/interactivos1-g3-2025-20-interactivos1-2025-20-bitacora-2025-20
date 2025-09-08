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

### Actividad 3  
**Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.**  

No se queda pendiente de una sola cosa, porque puede estar atenta tanto al botón A como al tiempo que pasa.  

**Identifica los estados, eventos y acciones en el programa.**

**Estados:**  
* **STATE_INIT:** cuando inicia.     
* **STATE_HAPPY:** muestra la carita feliz.   
* **STATE_SMILE:** muestra una sonrisa.   
* **STATE_SAD:** muestra la carita triste.   

**Eventos:**
* Cuando se unde el botón A.    
* Cuando pasa cierto tiempo.  

**Acciones:**
* Cuando se muestra la imagen de la carita feliz, triste, etc.  
* Cuando se reinicia el tiempo.  

**Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.**

**Prueba 1:**  
* **Estado inicial:** El sistema está recién encendido, en el estado inicial (STATE_INIT).  
* **Acción:** El programa se ejecuta sin intervención del usuario.  
* **Resultado esperado:** Se muestra una carita feliz por un segundo y medio y luego el sistema pasa al estado STATE_HAPPY.  

**Prueba 2:**
* **Estado inicial:** El micro:bit se encuentra mostrando la carita feliz (STATE_HAPPY) y no ha pasado el tiempo del ciclo.  
* **Acción:** El usuario unde el botón A.  
* **Resultado esperado:** La expresión cambia de inmediato a una carita triste y se actualiza el estado a STATE_SAD.  

**Prueba 3:**
* **Estado inicial:** El dispositivo está en STATE_SMILE.  
* **Acción:** No se unde ningún botón y se deja correr el tiempo.  
* **Resultado esperado:** Después de 1 segundo, el sistema muestra una cara triste y pasa a STATE_SAD.  




