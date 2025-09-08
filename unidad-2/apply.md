# Unidad 2


## 🛠 Fase: Apply

### Actividad 4
<img width="750" height="746" alt="image" src="https://github.com/user-attachments/assets/1b6cd2aa-d1be-422b-8cec-1682df134b37" />

### Actividad 5
```js
from microbit import *
import music
import utime

TIME_MIN = 10
TIME_MAX = 60
TIME_INIT = 20
INTERVAL = 1000  # 1 segundo en milisegundos

STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

state = STATE_CONFIG
time_set = TIME_INIT
time_left = TIME_INIT
start_time = utime.ticks_ms()

def show_time(t):
    display.show(str(t).rjust(2))  # siempre 2 dígitos


while True:
    # Estado CONFIG
    if state == STATE_CONFIG:
        if button_a.was_pressed() and time_set < TIME_MAX:
            time_set += 1
            show_time(time_set)
        elif button_b.was_pressed() and time_set > TIME_MIN:
            time_set -= 1
            show_time(time_set)
        elif accelerometer.was_gesture("shake"):
            time_left = time_set
            start_time = utime.ticks_ms()
            state = STATE_ARMED
            show_time(time_left)


    elif state == STATE_ARMED:
        current_time = utime.ticks_ms()
        if utime.ticks_diff(current_time, start_time) >= INTERVAL:
            time_left -= 1
            start_time = current_time
            show_time(time_left)
        

        if pin_logo.is_touched():
            state = STATE_CONFIG
            time_set = TIME_INIT
            show_time(time_set)


        if time_left <= 0:
            state = STATE_EXPLODED
            display.show(Image.SKULL)
            music.play(music.WAWAWAWAA)


    elif state == STATE_EXPLODED:
        # Reinicio por toque
        if pin_logo.is_touched():
            state = STATE_CONFIG
            time_set = TIME_INIT
            show_time(time_set)
```
**Vector de Prueba 1**  
Condiciones iniciales: state = STATE_CONFIG, time_set = 20 s. El botón A es presionado 5 veces antes de agitar el micro:bit.  
Evento: button_a.was_pressed() == True (5 veces) seguido de accelerometer.was_gesture('shake') == True.  

**Resultado esperado:**  
**Imagen:** Image.ARROW_N en cada incremento.  
**Estado nuevo:** STATE_ARMED.  
Tiempo configurado (time_set) = 25 s.  
Comienza la cuenta regresiva desde 25 s y, al llegar a 0, muestra Image.SKULL, reproduce music.WAWAWAWAA, vuelve a STATE_CONFIG con time_set = 20 s.    
**Resultado obtenido:** El sistema incrementa el tiempo, muestra flechas hacia arriba, cuenta desde 25 s, detona y reinicia correctamente.  

**Vector de Prueba 2**  
Condiciones iniciales: state = STATE_CONFIG, time_set = 20 s. El botón B es presionado 5 veces antes de agitar el micro:bit.  
Evento: button_b.was_pressed() == True (5 veces) seguido de accelerometer.was_gesture('shake') == True, luego pin_logo.is_touched() == True antes de llegar a 0.  

**Resultado esperado:**
**Imagen:** Image.ARROW_S en cada decremento.  
Estado nuevo después de agitar: STATE_ARMED.  
Tiempo configurado (time_set) = 15 s.  
Se desarma al tocar el logo, vuelve a STATE_CONFIG con time_set = 20 s, sin sonido ni imagen de explosión.  
**Resultado obtenido:** El sistema reduce el tiempo a 15 s, pasa a STATE_ARMED, se desarma al tocar el logo y vuelve a configuración con el tiempo reiniciado.  

**Vector de Prueba 3**  
**Condiciones iniciales:** state = STATE_CONFIG, time_set = 20 s.  
**Evento:** Se presiona button_a.was_pressed() == True repetidamente hasta intentar superar 60 s y luego button_b.was_pressed() == True repetidamente hasta intentar bajar de 10 s, después accelerometer.was_gesture('shake') == True.  
**Resultado esperado:**  
**Imagen:** Image.ARROW_N al incrementar y Image.ARROW_S al decrementar.  
Límite superior: 60 s.  
Límite inferior: 10 s.  
Estado nuevo después de agitar: STATE_ARMED con el último tiempo válido.  
Al llegar a 0: Image.SKULL, sonido music.WAWAWAWAA, vuelve a STATE_CONFIG con time_set = 20 s.  
**Resultado obtenido:** El sistema respeta los límites, arma la bomba y detona con el tiempo configurado, reiniciando correctamente.  

**Vector de Prueba 4**  
**Condiciones iniciales:** state = STATE_ARMED, time_left = 5 s.  
**Evento:** Esperar a que el contador llegue a 0 sin presionar nada, luego tocar el logo (pin_logo.is_touched() == True).  
**Resultado esperado:**  
Al llegar a 0: muestra Image.SKULL, reproduce music.WAWAWAWAA.  
No vuelve a STATE_CONFIG automáticamente, permanece en STATE_EXPLODED.  
Tras tocar el logo, vuelve a STATE_CONFIG y time_set = 20 s.  
**Resultado obtenido:** El sistema detona, muestra calavera y sonido, permanece en STATE_EXPLODED hasta que se toca el logo, luego vuelve a configuración con el tiempo reiniciado.  

**Vector de Prueba 5**
**Condiciones iniciales:** state = STATE_CONFIG, time_set = 20 s.  
**Evento:** Presionar button_a.was_pressed() hasta llegar exactamente a 60 s.  
Agitar (accelerometer.was_gesture('shake') == True).  
**Resultado esperado:** No permite pasar de 60 s, aunque se presione más veces el botón A.  
**Estado nuevo:** STATE_ARMED con time_left = 60 s.  
Cuenta regresiva correcta desde 60 s hasta 0.  
Al llegar a 0: calavera, sonido y reinicio con 20 s.  
**Resultado obtenido:** El sistema respeta el límite máximo, inicia con 60 s, detona y reinicia correctamente.  
 


