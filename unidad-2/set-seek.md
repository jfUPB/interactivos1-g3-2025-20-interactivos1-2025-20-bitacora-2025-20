# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01 - 30/07/2025

**Describción de cómo funciona el ejemplo.**

Este programa define una clase llamada `Pixel`, la cual controla el encendido y apagado de un píxel específico en el display del micro:bit, utilizando una **máquina de estados**. 

Cada instancia de `Pixel` comienza en el estado "Init", en donde registra el tiempo inicial y enciende el píxel en una intensidad inicial. Luego, pasa al estado "WaitTimeout", donde espera a que transcurra un intervalo de tiempo definido. Si el tiempo ha pasado, alterna el estado del píxel entre encendido (9) y apagado (0), y actualiza el display.

Se crean dos instancias:  
- `pixel1` controla el LED en la posición (0, 0) y cambia cada segundo.  
- `pixel2` controla el LED en la posición (4, 4) y cambia cada 500 milisegundos.  
- Ambos se actualizan continuamente dentro del ciclo while True.

**¿Cuáles son los estados en el programa?**

- **Init**: Estado inicial, se configura el tiempo de referencia y se enciende el píxel.
- **WaitTimeout**: Estado en espera, verifica si ha pasado el intervalo de tiempo para alternar el estado del píxel.
  
**¿Cuáles son los eventos/inputs en el programa?**

- **Tiempo transcurrido**: Activa el cambio de estado.

**¿Cuáles son las acciones en el programa?**

- Registrar el tiempo de inicio.
- Mostrar el píxel en el display. 
- Alternar el estado del píxel entre 0 y 9.
- Cambiar de estado.

### Actividad 02 

**Código que soluciona el problema**

```python
from microbit import *
import utime

state = "RED"
startTime = utime.ticks_ms()

while True:
    if state == "RED":
        display.clear()
        display.set_pixel(2, 0, 9)  # Rojo arriba
        if utime.ticks_diff(utime.ticks_ms(), startTime) > 3000:
            state = "GREEN"
            startTime = utime.ticks_ms()

    elif state == "GREEN":
        display.clear()
        display.set_pixel(2, 2, 9)  # Verde centro
        if utime.ticks_diff(utime.ticks_ms(), startTime) > 3000:
            state = "YELLOW"
            startTime = utime.ticks_ms()

    elif state == "YELLOW":
        display.clear()
        display.set_pixel(2, 4, 9)  # Amarillo abajo
        if utime.ticks_diff(utime.ticks_ms(), startTime) > 1500:
            state = "RED"
            startTime = utime.ticks_ms()
```

**Estados, eventos y acciones**

**Estados**
- `"RED"`: Se enciende el LED superior (rojo).
- `"GREEN"`: Se enciende el LED del medio (verde).
- `"YELLOW"`: Se enciende el LED inferior (amarillo).

**Eventos / Inputs**
- Tiempo transcurrido en cada estado.
- Cambia de rojo a verde después de 3 s.
- Cambia de verde a amarillo después de 3 s.
- Cambia de amarillo a rojo después de 1.5 s.

**Acciones**
- Borrar el display.
- Encender el LED correspondiente según el estado.
- Cambiar de estado tras cumplirse el intervalo de tiempo.
- Reiniciar el contador de tiempo.
 
### Actividad 03 - 01/08/2025

**Descripción del comportamiento del sistema**

Este programa implementa una máquina de estados que alterna entre diferentes expresiones faciales en el display de la micro:bit (feliz, sonriente y triste) siguiendo un **ciclo de tiempo predefinido**. Adicionalmente, el sistema puede **interrumpir este ciclo en cualquier momento** si el usuario presiona el botón A, y cambiar la imagen mostrada según el estado actual.

**¿Por qué decimos que este programa maneja tareas de forma concurrente?**

Decimos que este programa maneja tareas de forma concurrente porque el programa supervisa continuamente dos tipos de eventos simultáneamente de la siguiente forma:

1. El `paso del tiempo`, para cambiar automáticamente la expresión en pantalla según el intervalo establecido.
2. La `presión del botón A`, que interrumpe ese ciclo para cambiar la imagen de manera inmediata, sin esperar a que termine el intervalo.

El programa revisa constantemente, en cada vuelta del ciclo, si pasó el tiempo o si se presionó el botón, lo que permite que reaccione a diferentes eventos al mismo tiempo sin que uno bloquee al otro.

**Estados del sistema**

- `STATE_INIT:` Estado inicial de configuración, transiciona inmediatamente a STATE_HAPPY.
- `STATE_HAPPY:` Muestra Image.HAPPY.
- `STATE_SMILE:` Muestra Image.SMILE.
- `STATE_SAD:` Muestra Image.SAD.
  
**Eventos / Entradas**

- Presión del botón A.
- Paso del tiempo definido para cada estado.

**Acciones**

- Mostrar una imagen en el display.
- Cambiar de estado.
- Reiniciar el temporizador.
- Establecer nuevo intervalo.

**Vectores de prueba**

**Vector de prueba 1: Transición automática por tiempo (sin botón)**

- Condición inicial: El sistema se encuentra en STATE_HAPPY.
- Evento generado: Pasan 1500 ms sin presionar ningún botón.
- Resultado esperado: El sistema cambia a STATE_SMILE y muestra la imagen Image.SMILE.
- Resultado obtenido: El sistema cambió correctamente al estado Smile correctamente luego de los 1,5 segundos sin presionar ningún botón.
- Código:
```python
from microbit import *
import utime

# Mostrar cara feliz
display.show(Image.HAPPY)
utime.sleep_ms(1500)

# Transición automática a sonrisa
display.show(Image.SMILE)
```
**Vector de prueba 2: Interrupción desde estado feliz con botón A**

- Condición inicial: El sistema se encuentra en STATE_HAPPY.
- Evento generado: Se presiona el botón A antes de que pasen los 1500 ms.
- Resultado esperado: El sistema cambia a STATE_SAD inmediatamente después de presionar el botón A, si no se presiona antes de 1,5 segundos pasa a STATE_SMILE.
- Resultado obtenido: El sistema mostró la imagen de la cara triste al presionar el botón A. En la segundo prueba se dejó pasar el tiempo y funcionó igual al vector 1, cambiando automáticamente a SMILE después de haber pasado el tiempo.
- Código:
```python
from microbit import *
import utime

display.show(Image.HAPPY)
start_time = utime.ticks_ms()

while True:
    if button_a.was_pressed():
        # Interrupción esperada
        display.show(Image.SAD)
        break
    if utime.ticks_diff(utime.ticks_ms(), start_time) > 1500:
        # Si no se presionó el botón a tiempo
        display.show(Image.SMILE)
        break
```

**Vector de prueba 3: Interrupción desde estado triste con botón A**

- Condición inicial: El sistema se encuentra en STATE_SAD.
- Evento generado: Se presiona el botón A.
- Resultado esperado: El sistema cambia a STATE_SMILE y muestra Image.SMILE.
- Resultado obtenido: El micro:bit mostró la cara SAD, luego de presionar el botón A pasó a mostrar la cara Smile.
- Código:
```python
from microbit import *
import utime

# Iniciar directamente en estado triste
display.show(Image.SAD)

while True:
    if button_a.was_pressed():
        # Si se presiona A desde el estado SAD
        display.show(Image.SMILE)
        break
```







