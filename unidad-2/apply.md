# Unidad 2
## 🛠 Fase: Apply
### Actividad 4
#### Diseño de la lógica de una bomba temporizada
![Diagrama de la maquina de estado](https://www.canva.com/design/DAGvU67JuO8/_dCyHHNlJYEWAHGjNKvB0w/edit?utm_content=DAGvU67JuO8&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

### Actividad 5
#### Implementando la Bomba Temporizada
##### Codigo
```python
from microbit import *
import utime

State_Init = 0
State_Settings = 1
State_Armed = 2
State_Exploted = 3

interval = 0
current_State = State_Init
cero = 0

def bomba():
    global interval
    global current_State
    global cero
    if current_State==State_Init:
        interval=20
        display.show(interval)
        display.clear()
        current_State=State_Settings
        
    elif current_State == State_Settings:
        
        if button_a.was_pressed():
            if interval<60:
                interval = interval + 1
                display.show(interval)
                display.clear()
                current_State=State_Settings
                
        if button_b.was_pressed():
            if interval >10:
                interval = interval - 1
                display.show(interval)
                display.clear()
                current_State=State_Settings

        if accelerometer.was_gesture('shake'):
            current_State=State_Armed
            
    elif current_State == State_Armed:
    
        for i in range (interval,cero -1,-1):
            display.show(str(i))
            utime.sleep(1)
        
        display.scroll('BUM')
        current_State = State_Exploted

    elif current_State == State_Exploted:
        display.show(Image.SKULL)
        if pin_logo.is_touched():
            current_State=State_Init

    else:
        pass

while True:
    bomba()
```
##### Vectores de prueba
1. Si estoy en el estado Settings, presiono el boton A, deberia sumarle un segundo al tiempo incial y volver a estado Settings para esperar otra "indicacion". (Funciona)
2. Si estoy en el estado Settings, presiono el boton B, deberia restarle un segundo al tiempo incial y volver a estado Settings para esperar otra "indicacion". (Funciona)
3. Si estoy en el estado Settings, mando la indicacion de 'shake', deberia cambiar de estado. (Funciona)
4. Si estoy en el estado Armed, el primer numero que deberia ver es el tiempo que modifique o no en el estado anterior, y ver la cuenta regresiva hasta 0. (Funciona)
5. Si estoy en el estado Armed, una vez la cuenta regresiva termine, es decir, llega a cero, debe mostrar un texto que diga 'BUM' y debe cambiar de estado. (Funciona)
6. Si estoy en el estado Exploted, debe mostrar una calavera, y una vez se presione el 'touch' debe inicializar el tiempo al original, y cambiar de estado. (Funciona)
7. Si cambie de estado Exploted a estado Settings, al presionar A o B, debe cambiar el tiempo inicial. (Funciona)


