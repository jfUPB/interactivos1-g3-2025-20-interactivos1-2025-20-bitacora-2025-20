# Unidad 1

## 🛠 Fase: Apply

### Actividad 05: 23/07/2025

El sistema físico interactivo que creamos une las aplicaciones **p5.js** y **micro:bit editor** para lograr que el computador (micro:bit) siga las intrucciones dadas en el software p5.js para hacer que al mantener presionado el botón A el software muestre en la pantalla un cuadrado rojo, de lo contrario muestra un cuadrado de color verde. 

Usando condicionales y ciclos, en el lado del software de dibujo, se logra que la duración del color rojo en la pantalla coincida con el tiempo que está presionado, en lugar de solo aparecer por menos de un segundo; por otro lado, en el micro:bit editor se usa el comando sleep(100) para lograr que se mantenga la imagen durante mas tiempo o se "duerma" el computador durante 100 milisegundos. De igual forma, se sabe que es necesario, conectar el micro:bit al p5.js para que pueda funcionar, ya que de lo contrario no detectaria el computador. Además, se hace uso de bibliotecas en el software para lograr que funcione correctamente.

Por otro lado, es posible verificar si el micro:bit esta conectado al equipo mirando el color del cuadrado proyectadfo en la pantalla, ya que mientras esté desconectado este será de color blanco.

#### Código micro:bit.editor

```python
from microbit import *

uart.init(baudrate=115200)
display.show(Image.SILLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
    if button_b.is_pressed():
        uart.write('B')
```


