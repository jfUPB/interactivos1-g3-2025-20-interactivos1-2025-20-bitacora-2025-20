# Unidad 2


## 🛠 Fase: Apply

### Actividad 4:  
<img width="540" height="272" alt="image" src="https://github.com/user-attachments/assets/8711c931-4007-4848-98fc-b79a98c75d73" />

_____________________________________________________________________

### Actividad 5:  

__1) El código que implementa la bomba temporizada.__  

```py

from microbit import *
import utime

STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

time_set = 20   # Tiempo inicial en segundos
min_time = 10
max_time = 60

current_state = STATE_CONFIG
start_time = 0
remaining_time = time_set

while True:
    if current_state == STATE_CONFIG:
        display.show(str(time_set))
        
        if button_a.was_pressed():
            if time_set < max_time:
                time_set += 1
        
        if button_b.was_pressed():
            if time_set > min_time:
                time_set -= 1
        
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            remaining_time = time_set
            start_time = utime.ticks_ms()
    
    elif current_state == STATE_ARMED:
        display.show(str(remaining_time))
        
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= 1000:
            remaining_time -= 1
            start_time = utime.ticks_ms()
        
        if remaining_time <= 0:
            current_state = STATE_EXPLOSION
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
    
    elif current_state == STATE_EXPLOSION:
        for i in range(5):
            display.show(Image.SKULL)
            utime.sleep_ms(200)
            display.clear()
            utime.sleep_ms(200)
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
```

__2) La definición de los vectores de prueba básicos que permiten verificar el correcto funcionamiento del programa.__  

__Vector de prueba 1 — Configuración del tiempo (UP/DOWN)__  
*Condiciones iniciales*  
- Estado: CONFIGURACIÓN  
- Tiempo configurado: 20 segundos.  

*Eventos*  
- Presionar botón A (UP) tres veces.  
- Presionar botón B (DOWN) dos veces.  

*Resultado esperado*  
- Después de las 3 presiones en UP: tiempo = 23 segundos.  
- Después de las 2 presiones en DOWN: tiempo = 21 segundos.  
- El valor se mantiene en pantalla.  


__Vector de prueba 2 — Límite mínimo y máximo del tiempo__   
*Condiciones iniciales*  
- Estado: CONFIGURACIÓN.  

*Eventos*  
- Colocar el tiempo en 60 segundos y presionar UP una vez más.  
- Colocar el tiempo en 10 segundos y presionar DOWN una vez más.  

*Resultado esperado*  
- En el caso 1: el tiempo sigue siendo 60 (no aumenta).  
- En el caso 2: el tiempo sigue siendo 10 (no disminuye).  


__Vector de prueba 3 — Armado de la bomba__   
*Condiciones iniciales*  
- Estado: CONFIGURACIÓN.  
- Tiempo configurado: 15 segundos.  

*Eventos*  
- Agitar el micro:bit (gesto shake).  

*Resultado esperado*  
- Estado cambia a ARMADA.  
- Pantalla muestra 15 y empieza a decrementar cada segundo.  


__Vector de prueba 4 — Cuenta regresiva completa__  
*Condiciones iniciales*  
- Estado: ARMADA.  
- Tiempo restante: 5 segundos.  

*Eventos*  
- Dejar correr el tiempo hasta llegar a 0.  

*Resultado esperado*  
- Cuando llega a 0, estado pasa a EXPLOSIÓN.  
- Pantalla parpadea con icono de calavera (Image.SKULL) durante 5 ciclos.  


__Vector de prueba 5 — Reinicio desde EXPLOSIÓN__  
*Condiciones iniciales*  
- Estado: EXPLOSIÓN.  

*Eventos*  
- Tocar el logo táctil (pin_logo.is_touched()).  

*Resultado esperado*  
- Estado vuelve a CONFIGURACIÓN.  
- Tiempo mostrado en pantalla es el configurado antes de armar la bomba.  


__Vector de prueba 6 — Cancelación en medio de la cuenta regresiva__  
*Condiciones iniciales*  
- Estado: ARMADA.  
- Tiempo restante: 12 segundos.  

*Eventos*  
- Tocar el logo táctil antes de que el tiempo llegue a 0.  

*Resultado esperado*  
- Estado vuelve a CONFIGURACIÓN.  


