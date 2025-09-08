# Unidad 3

## 🔎 Fase: Set + Seek

Codigo semadoro
mi pequeño intento
```python
from microbit import *
import utime

class Semaforo():
    def __init__(self,timeR,timeY,timeG,pixelX,pixelY):
        self.state = "Init"
        self.startTime =0
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.timeR = timeR
        self.timeY = timeY
        self.timeG = timeG
    
Semaforo1 = Semaforo(5000,2000,3000,)
```
codigo que realizamos en clase
```python
from microbit import*
import utime

class Semaforo:
    def __init__(self,columna,timeR,timeY,timeG):
        self.columna = columna
        self.timeR = timeR
        self.timeY = timeY
        self.timeG = timeG
        display.set_pixel(self.columna,0,9)
        self.state = "waitR"
        self.startTime = utime.ticks_ms()
        self.interval = self.timeR

    def update(self):
        if self.state == "waitR":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) >= self.interval:
                display.set_pixel(self.columna,0,0) #Apago rojo
                display.set_pixel(self.columna,2,9) #Prendo Amarillo
                self.startTime = utime.ticks_ms()
                self.interval = self.timeY
                self.state == "waitY"
                
        if self.state == "waitY":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) >= self.interval:
                display.set_pixel(self.columna,2,0) #Apago amarillo
                display.set_pixel(self.columna,4,9) #Prendo verde
                self.startTime = utime.ticks_ms()
                self.interval = self.timeG
                self.state == "waitG"

        if self.state == "waitG":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) >= self.interval:
                display.set_pixel(self.columna,2,0) #Apago verde
                display.set_pixel(self.columna,4,9) #Prendo rojo
                self.startTime = utime.ticks_ms()
                self.interval = self.timeR
                self.state == "waitR"
                                   
semaforo1 = Semaforo(0,5000,2000,3000)
semaforo2 = Semaforo(2,3000,1000,2000)
semaforo3 = Semaforo(4,4000,3000,2000)

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
```
inicio codigo bomba con objeto 
```python
from microbit import*
import utime

class Bomba:
    def __init__(self,interval):
        self.state= "Init"
        self.interval = 20000
        self.startTime = utime.ticks_ms()
        pass

    def update(self):
        if self.state == "Init":
            pass

        if self.state == "Settings":
            pass
        if self.state == "Armed":
            pass
        if self.state == "Exploted":
            pass




while True:
    Bomba
```

