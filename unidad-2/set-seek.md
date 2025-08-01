# Unidad 2

## 🔎 Fase: Set + Seek  

### Avtividad #1:  

__1) ¿Cómo funciona este ejemplo?__  
*A) Se define una clase Pixel*   

*B) Cada objeto Pixel tiene:*  
- Una posición (pixelX, pixelY).  
- Un estado interno (self.state) que controla el flujo del programa.  
- Un intervalo de tiempo en milisegundos (self.interval) que indica cada cuánto debe cambiar el estado del píxel (encendido o apagado).  
- Un valor de brillo (self.pixelState), que alterna entre 0 (apagado) y 9 (encendido máximo).  
  
*C) El método update() gestiona el comportamiento de cada píxel:*  
- En el estado "Init", se inicializa el tiempo y se enciende o apaga el píxel según su pixelState.  
- Luego cambia al estado "WaitTimeout".  
- En "WaitTimeout", espera hasta que haya transcurrido el interval. Una vez transcurrido, cambia el estado del píxel (encender si estaba apagado, y viceversa) y reinicia el contador de tiempo.  

*D) En el while True:, se llama continuamente a update() para dos píxeles:*  
- Uno en la posición (0,0) con intervalo de 1000 ms.  
- Otro en la posición (4,4) con intervalo de 500 ms. 
- Resultado: los píxeles parpadean independientemente, uno más lento que el otro.  

__2) ¿Cuáles son los estados en el programa?__  
Los estados están representados por el atributo self.state. En este caso, hay dos estados:  
*Init* – Estado inicial donde se configura el tiempo de inicio y se enciende/apaga el píxel por primera vez.  
*WaitTimeout* – Estado de espera donde el programa comprueba si ha transcurrido el tiempo para cambiar el estado del píxel.  

__3) ¿Cuáles son los eventos/inputs en el programa?__  
Aunque no hay entradas físicas del usuario (como botones o sensores), los eventos aquí están basados en tiempo:  
El evento principal es: “¿Ha pasado el tiempo especificado por el intervalo?”  
Esto se evalúa con utime.ticks_diff(...) > self.interval.  

__4) ¿Cuáles son las acciones en el programa?__  
*Cambiar el estado del píxel:*  
-Si está apagado (0), se enciende (9).  
-Si está encendido (9), se apaga (0).  

*Mostrar el píxel en el display con display.set_pixel(...).*  
-Actualizar el tiempo de inicio con self.startTime = utime.ticks_ms().  

______________________________________________________________________________________________________  

### Avtividad #2:  

__1) Escribe el código que soluciona este problema en tu bitácora.__  

```py
from microbit import *
import utime

class SemaforoSimbolico:
    def __init__(self):
        self.estado = "Rojo"
        self.tiempo_inicio = utime.ticks_ms()

    def actualizar(self):
        ahora = utime.ticks_ms()
        transcurrido = utime.ticks_diff(ahora, self.tiempo_inicio)

        if self.estado == "Rojo":
            display.show("R")
            if transcurrido >= 3000:
                self.estado = "Verde"
                self.tiempo_inicio = ahora

        elif self.estado == "Verde":
            display.show("V")
            if transcurrido >= 3000:
                self.estado = "Amarillo"
                self.tiempo_inicio = ahora

        elif self.estado == "Amarillo":
            display.show("A")
            if transcurrido >= 1000:
                self.estado = "Rojo"
                self.tiempo_inicio = ahora

# Crear instancia
semaforo = SemaforoSimbolico()

# Bucle principal
while True:
    semaforo.actualizar()
    sleep(100)  # Reducción de carga

```

__2) Identifica los estados, eventos y acciones en tu código.__  
*Estados:*  
- "Rojo"  
- "Verde"  
- "Amarillo"  

*Eventos:*  
- Tiempo transcurrido para cada estado.  

*Acciones:*  
- Encender el LED correspondiente y apagar los demás.  




______________________________________________________________________________________________________  
