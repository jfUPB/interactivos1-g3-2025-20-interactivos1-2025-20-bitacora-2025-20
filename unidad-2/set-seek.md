# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

#### ¿Cuáles son los estados en el programa?
  1. Init
  2. "WaitTimeout"

#### ¿Cuáles son los eventos/inputs en el programa?
```python
if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
```
Se espera a que pase el tiempo o sea la hora de prender/apagar.

#### ¿Cuáles son las acciones en el programa?
1. ```python
   self.startTime = utime.ticks_ms()
   ```
2. ```python
   display.set_pixel(self.pixelX,self.pixelY,self.pixelState)
   ```
3. Condición de guarda
   ```python
   if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
   ```

##### Notas:
- Al crear un objeto primero lo creo en el espacio de memoria y luego inicializo el objeto, y guardamos la direccion del objeto en una variable.  
- __init__  para crear un constructor.  
- this. en c# es self. en python.  
- Para funciones en python utilizamos def.  
- El evento es cuando debo esperar algo para que pase algo.

### Actividad 2
```python
from microbit import*
import utime

State_init = 0
State_red = 1
State_yellow = 2
State_green = 3

currentState = State_init

startTime = 0
interval = 0

Time_in_red = 3000
Time_in_yellow = 1000
Time_in_green = 3000

def tarea1():
    global currentState
    global startTime
    global interval

    if currentState == State_init:
        display.set_pixel(2,0,9)
        startTime = utime.ticks_ms()
        interval = Time_in_red
        currentState = State_red
    elif currentState == State_red:
        if utime.ticks_diff(utime.ticks_ms(),startTime)> interval:
            display.set_pixel(2,2,9)
            startTime = utime.ticks_ms()
            interval = Time_in_yellow
            currentState = State_yellow
            display.set_pixel(2,0,0)
    elif currentState == State_yellow:
        if utime.ticks_diff(utime.ticks_ms(),startTime)> interval:
            display.set_pixel(2,4,9)
            startTime = utime.ticks_ms()
            interval = Time_in_green
            currentState = State_green
            display.set_pixel(2,2,0)
    elif currentState == State_green:
        if utime.ticks_diff(utime.ticks_ms(),startTime)> interval:
            display.set_pixel(2,0,9)
            startTime = utime.ticks_ms()
            interval = Time_in_red
            currentState = State_red
            display.set_pixel(2,4,0)
    else:
        pass

while True:
    tarea1()
```

### Actividad 3

####  Identifica los estados, eventos y acciones en el programa.
- pseudoestado init (no espera nada), estado happy, estado smile y estado sad.
- Eventos ciclo "normal":
1. De happy a smile: 
```python 
if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
```
2. De smile a sad: 
```python 
if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
```
3. De sad a happy: 
```python 
if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
``` 
- Condiciones que cambian el ciclo normal:
1. De happy a sad: 
```python 
if button_a.was_pressed(): 
``` 
2. De smile a happy:
```python 
 if button_a.was_pressed(): 
```
3. De sad a smile: 
```python 
if button_a.was_pressed(): 
``` 
- Acciones para los eventos:
1. 
```python 
 display.show(Image.SAD)
```
2. 
```python 
 display.show(Image.SMILE)
```
3. 
```python 
display.show(Image.HAPPY)
``` 
- Aciiones de entrada para los siguientes eventos:
1. 
```python 
            start_time = utime.ticks_ms()
            interval = SAD_INTERVAL
            current_state = STATE_SAD
```
2. 
```python 
            start_time = utime.ticks_ms()
            interval = SMILE_INTERVAL
            current_state = STATE_SMILE
```
3. 
```python 
            start_time = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            current_state = STATE_HAPPY
```
#### Codigo hecho en clase
```python
from microbit import *
import utime

State_init = 0
State_happy = 1
State_smile = 2
State_sad = 3

currentState = State_init

startTime = 0
interval = 0

TIME_IN_HAPPY = 1500
TIME_IN_SMILE = 1000
TIME_IN_SAD = 2000

def tarea1():
    global currentState
    global startTime
    global interval
    
    if currentState == State_init:
        display.show(Image.HAPPY)
        startTime = utime.ticks_ms()
        interval = TIME_IN_HAPPY
        currentState = State_happy
    
    elif currentState == State_happy:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > interval:
            display.show(Image.SMILE)
            startTime = utime.ticks_ms()
            interval = TIME_IN_SMILE
            currentState = State_smile

        if button_a.was_pressed():
            display.show(Image.SAD)
            currentState = State_sad
                   
            
    elif currentState == State_smile:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > interval:
            display.show(Image.SAD)
            startTime = utime.ticks_ms()
            interval = TIME_IN_SAD
            currentState = State_sad

        if button_a.was_pressed():
            display.show(Image.HAPPY)
            currentState = State_happy
        
    elif currentState == State_sad:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > interval:
            display.show(Image.HAPPY)
            startTime = utime.ticks_ms()
            interval = TIME_IN_HAPPY
            currentState = State_happy

        if button_a.was_pressed():
            display.show(Image.SMILE)
            currentState = State_smile
        
    else:
        pass

while True:
    tarea1()
```
*Notas: Una tarea puede hacer muchas cosas dependiendo el estado en el que este. error: 'currentState' is unbound, quiere decir que estamos utilizando una variable local sin definirla, pero la variable ya la habiamos definido por fuera de la condición, en python hay que poner global currentState, para que el error se quite, así le decimos que no es una variable local sino global.*
