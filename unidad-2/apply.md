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
   
*2. La definición de los vectores de prueba básicos que permiten verificar el correcto funcionamiento del programa.*
