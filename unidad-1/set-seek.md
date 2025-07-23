# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1  
*¿Qué es un sistema físico interactivo?*  
Un sistema físico interactivo es el que tiene inputs que se envían a un programa, sistema o proceso en tiempo real; el respectivo programa y outputs, los cuales segun los inputs generaran una respuesta específica  

*¿Cómo podrías aplicar lo que has visto en tu perfil profesional?*  
Teniendo en cuenta que la carrera se basa en el entretenimiento digital, sería util en multiples sistemas, los cuales consistan en generar experiencias interactivas, esto ya que sería completamente en vivo  


### Actividad 2  

*¿Qué es el diseño/arte generativo?*  
Son piezas gráficas que se generan mediante sistemas físicos interactivos, hay un programa que recibe un tipo específico de inputs en vivo y los traduce o plasma en piezas gráficas que varían según la entrada de información

*¿Cómo podrías aplicar lo que has visto en tu perfil profesional?*  
Especfíficamente yo que tengo interes en el área de experiencias interactivas tengo demasiadas poisibilidades para usar esto, ya que todos los sistemas que funcionen de esta forma pueden ser usados para cualquier tipo de experiencia, sea para usarla por un tercero o por mi persona en la generación de piezas gráficas

### Actividad 3  


### Actividad 4  


### Actividad 5  



### Actividad 6
Enlace a mi proyecto en p5.js [aquí](https://editor.p5js.org/n4ndeZzz/sketches/Cz1YTku9Q)  

```javascript
let port;
let connectBtn;
let connectionInitialized = false;
let circleX = 200;

function setup() {
  createCanvas(400, 400);
  background(220);
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  if (port.availableBytes() > 0) {
    let dataRx = port.read(1);
    if (dataRx == "A") {
      circleX -= 10; 
    } else if (dataRx == "B") {
      circleX += 10; 
    }
  }

  ellipse(circleX, height / 2, 50, 50);

  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}
```

```py
from microbit import *

uart.init(baudrate=115200)

while True:

    if button_a.is_pressed():
        uart.write('A')
        
    if button_b.is_pressed():
        uart.write('B')


    sleep(100)
```
