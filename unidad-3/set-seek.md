# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5

#### Es momento de modelar la bomba y definir vectores de prueba

##### Ahora es momento de modelar la bomba con una máquina de estados y definir una tabla con vectores de prueba.

*1. Construye el modelo de la bomba 3.0. Como ya tienes el código puedes tener un modelo muy preciso (o en cristiano, hacer el diagrama del codigo que ya esta hecho)*

<img width="1717" height="953" alt="image" src="https://github.com/user-attachments/assets/5c49f3f7-8d85-4fe3-b326-8b57f7eed598" />

*2. Crear una tabla con los vectores de prueba. La tabla debe tener 4 columnas por vector y puedes agrupar vectores en un gran vector. Las columnas son:*

*- Estado inicial*

*- Evento disparador*

*- Acciones*

*- Estado final*

| Estado Inicial | Evento disparador | Acciones | Estado Final |

| CONFIG | 2 | 3 | 4 |
| CONFIG | 2 | 3 | 4 |
| CONFIG | 2 | 3 | 4 |
| CONFIG | 2 | 3 | 4 |








//Este código tiene un error raro. Para buscar (dejo el codigo fallido por si algo, en caso de que antes de que el profesor evalue, no lo haya borrado):

``` py
from microbit import *
import utime

display.clear()

class Event():
    
    def __init__(self):
        self.eventValue = 0

    def write(self,eventValue):
        self.eventValue = eventValue

    def read(self):
        return self.eventValue

    def clear(self):
        self.eventValue = 0

class MicroBitSensors():
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")
        if button_b.was_pressed():
            event.write("B")
        if accelerometer.was_gesture('shake'):
            event.write("S")
        if pin_logo.is_touched():
            event.write("T")

class SerialTask:
    def __init__(self):
        uart.init(baudrate=115200)

    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.write("A")
                if data[0] == ord('B'):
                    event.write("B")
                if data[0] == ord('T'):
                    event.write("T")
                if data[0] == ord('S'):
                    event.write("S")


class BombTask:
    def __init__(self):
        
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)
        

    def update(self):
        if self.state == 'CONFIG':
            if event.read() == "A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read() == "B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read() == "S":
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if event.read() == "A":
                #display.show(Image.BUTTERFLY)
                #sleep(2)
                event.clear()
                self.keyindex = self.keyindex + 1

            if event.read() == "B":
                #display.show(Image.SMILE)
                #sleep(2)
                event.clear()
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):
                #display.show(Image.SMILE)
                #sleep(2)
                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        display.show(Image.SMILE)
                        sleep(2)
                        passIsOK = False
                        break
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0
        
        elif self.state == 'EXPLODED':
            if event.read() == "T":
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
microBitSensors = MicroBitSensors()
serialTask = SerialTask()

while True:
    microBitSensors.update()
    serialTask.update()
    bombTask.update()

```








