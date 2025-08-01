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
- "RED": Se enciende el LED superior (rojo).
- "GREEN": Se enciende el LED del medio (verde).
- "YELLOW": Se enciende el LED inferior (amarillo).

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






