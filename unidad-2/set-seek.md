# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

**Describe detalladamente cómo funciona este ejemplo.**

* Controla dos leds o pixeles en el microbit y con un temporizador los enciende y apaga dependiendo de un intervalo de tiempo.

**¿Cuáles son los estados en el programa?**

* Init, WaitTimeout.

**¿Cuáles son los eventos/inputs en el programa?**

* Interval, Update.

**¿Cuáles son las acciones en el programa?**

* Iniciar temporizador, mostrar pixel y alternar brillo de los leds.

### Actividad 2

**Escribe el código que soluciona este problema en tu bitácora.**

``` js
from microbit import *
import utime

class Semaforo:
    def init(self):
        self.state = "Init"
        self.startTime = 0
        self.interval = 0
        self.color = "ROJO"

    def mostrar_color(self):
        display.clear()
        if self.color == "ROJO":
            display.set_pixel(2, 0, 9)
            self.interval = 3000
        elif self.color == "VERDE":
            display.set_pixel(2, 4, 9)
            self.interval = 3000
        elif self.color == "AMARILLO":
            display.set_pixel(2, 2, 9)
            self.interval = 1000

    def cambiar_color(self):
        if self.color == "ROJO":
            self.color = "VERDE"
        elif self.color == "VERDE":
            self.color = "AMARILLO"
        elif self.color == "AMARILLO":
            self.color = "ROJO"

    def update(self):
        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            self.mostrar_color()

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval:
                self.cambiar_color()
                self.startTime = utime.ticks_ms()
                self.mostrar_color()

semaforo = Semaforo()

while True:
    semaforo.update()
```

**Identifica los estados, eventos y acciones en tu código.**

* Estados: VERDE, AMARILLO Y ROJO.

* Eventos: Que el tiempo necesario para las acciones ocurra.

* Acciones:  Encender el LED correspondiente al estado, actualizar temporizador y cambiar el estado.

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

