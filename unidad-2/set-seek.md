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



### Avtividad #3:


__1) Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.__  

*Hay 2 razones principales:*    

a) Aunque en MicroPython no hay múltiples hilos de ejecución, este código simula concurrencia al manejar eventos (como la pulsación de un botón) y temporizadores en un solo ciclo while True.  
  
b) El programa supervisa constantemente dos tipos de eventos:  

- Entrada del usuario (botón A).  
- Paso del tiempo (medido con utime.ticks_diff()).  

Esto significa que, por ejemplo, mientras se espera que pasen 1500 ms, también se puede detectar si el botón fue presionado.  
Así, ambas "tareas" (esperar tiempo y revisar botón) se están gestionando dentro del mismo bucle, como si fueran concurrentes, aunque se ejecutan de forma secuencial pero rápida.  



__2) Identifica los estados, eventos y acciones en el programa.__  

*Estados:*  

- STATE_INIT: Estado inicial.  
- STATE_HAPPY: Estado en el que se muestra una cara feliz.  
- STATE_SMILE: Estado en el que se muestra una sonrisa.  
- STATE_SAD: Estado en el que se muestra una cara triste.  

*Eventos:*  
Los eventos que pueden ocurrir y disparar cambios de estado son:  

- Presión del botón A: Detectado con button_a.was_pressed().  
- Transcurso de un intervalo de tiempo: Detectado con:   
```py  
utime.ticks_diff(utime.ticks_ms(), start_time) > interval  
```

*Acciones:*  
Las acciones son las operaciones que el programa realiza al ocurrir un evento:  

- Mostrar una imagen (display.show(...))  
- Actualizar el tiempo de inicio (start_time = utime.ticks_ms())  
- Cambiar el estado actual  
- Asignar un nuevo intervalo  


__3) Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.__  

__Vector de prueba 1__  
*Condiciones iniciales:*  
El programa comienza en STATE_INIT.  

*Evento generado:*  
Sin presionar botones, solo dejar pasar el tiempo.  

*Resultado esperado:*  
Se muestra Image.HAPPY.    

- Después de 1500 ms, cambia a Image.SMILE y estado STATE_SMILE.  

- Resultado observado: Correcto, el sistema pasa automáticamente del estado INIT → HAPPY → SMILE al cumplirse el tiempo.  

__Vector de prueba 2__  
*Condiciones iniciales:*  
El sistema está en STATE_HAPPY (cara feliz mostrada).  

*Evento generado:*  
Se presiona el botón A antes de que pasen 1500 ms.  

*Resultado esperado:*  
Se muestra Image.SAD, se actualiza el tiempo y cambia a STATE_SAD.  

- Resultado observado: Al presionar A, efectivamente cambia a SAD.  


__Vector de prueba 3__  
*Condiciones iniciales:*  
El sistema está en STATE_SAD (cara triste).  

*Evento generado:*  
Se deja pasar el tiempo (2000 ms) sin presionar botones.  

*Resultado esperado:*  
Se muestra Image.HAPPY, se actualiza tiempo, y cambia a STATE_HAPPY.  

- Resultado observado: Funciona correctamente: el estado cambia tras los 2000 ms.  



