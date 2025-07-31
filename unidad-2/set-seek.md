# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1.  

Al revisar el código trabajado en clase sobre el parpadeo de píxeles con micro:bit, me concentré en entender el funcionamiento interno del programa a partir de tres aspectos clave: los estados, los eventos/entradas y las acciones. 

#### 1. Estados del programa.  

El objeto "Pixel", creado dentro del programa, funciona como una pequeña máquina de estados. Cada objeto tiene dos estados principales:  

- "Init": Es el estado inicial, donde se configura el píxel por primera vez. Aquí se enciende el píxel y se guarda el tiempo actual.
  
- "WaitTimeout": Es el estado en el que el programa espera que pase un intervalo de tiempo específico. Si ese tiempo ya pasó, el píxel cambia de estado (se apaga si estaba encendido, o se enciende si estaba apagado).

Cada pixel tiene sus propios estados de forma independiente.  

#### 2.
Codigo semaforo:

```javascript
from microbit import *
import time

# Definir estados
RED = 0
YELLOW = 1
GREEN = 2

class TrafficLight:
    def __init__(self):
        self.state = RED
        self.last_change = running_time()  # tiempo del último cambio
    
    def update(self):
        current_time = running_time()
        
        if self.state == RED:
            self.show_red()
            if current_time - self.last_change >= 2000:
                self.state = GREEN
                self.last_change = current_time
        
        elif self.state == GREEN:
            self.show_green()
            if current_time - self.last_change >= 2000:
                self.state = YELLOW
                self.last_change = current_time
        
        elif self.state == YELLOW:
            self.show_yellow()
            if current_time - self.last_change >= 1000:
                self.state = RED
                self.last_change = current_time

    def show_red(self):
        display.clear()
        display.set_pixel(2, 0, 9)  # LED superior para rojo

    def show_yellow(self):
        display.clear()
        display.set_pixel(2, 2, 9)  # LED central para amarillo

    def show_green(self):
        display.clear()
        display.set_pixel(2, 4, 9)  # LED inferior para verde

        # Crear el objeto semáforo
light = TrafficLight()

# Bucle principal
while True:
    light.update()
    sleep(100)  # para que no colapse la CPU
```

- Estados:  
  El semáforo tiene 3 estados principales, que representan el color actual encendido:
  
  RED: el semáforo muestra la luz roja.  
  GREEN: el semáforo muestra la luz verde.  
  YELLOW: el semáforo muestra la luz amarilla.  

- Eventos:  
  Los eventos están relacionados con el paso del tiempo:
  
  Si el semáforo está en RED y han pasado 2 segundos, cambia a GREEN.  
  Si está en GREEN y han pasado 2 segundos, cambia a YELLOW.  
  Si está en YELLOW y ha pasado 1 segundo, cambia a RED.

- Acciónes:  
  Cada vez que se entra a un estado, se ejecuta una acción específica:
  
  En RED: se limpia la pantalla y se enciende el LED superior (posición 2,0).  
  En GREEN: se limpia la pantalla y se enciende el LED inferior (posición 2,4).  
  En YELLOW: se limpia la pantalla y se enciende el LED central (posición 2,2).  

- Resumen del programa:  
  El sistema inicia en el estado RED.    
  Cada cierto tiempo, el estado cambia de acuerdo al temporizador interno.  
  El ciclo de estados es el siguiente:  
  🔴 RED → 🟢 GREEN → 🟡 YELLOW → 🔴 RED … (se repite infinitamente).  
  Entre cada cambio de estado, el micro:bit espera el tiempo correspondiente antes de pasar al siguiente.

- Lógica:
  
  Se utiliza una clase llamada "TrafficLight" que guarda el estado actual y el tiempo del último cambio.  
  En el método "update()" se verifica si ha pasado suficiente tiempo desde el último cambio de color.  
  Si se cumple el tiempo, se cambia de estado y se actualiza la pantalla con el LED correspondiente.  





  
