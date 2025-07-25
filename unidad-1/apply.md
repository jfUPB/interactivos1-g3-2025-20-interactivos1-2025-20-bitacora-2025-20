# Unidad 1

## 🛠 Fase: Apply

### Actividad 5  
Este sistema físico interactivo conecta un microcontrolador (micro:bit) con un programa visual hecho en p5.js. Cuando la persona presiona el botón A en el micro:bit, este envía una señal por puerto serial a la computadora. El programa en p5.js recibe esa señal y cambia el color de un cuadrado en la pantalla dependiendo de si el botón está presionado o no.

Gracias a que el micro:bit envía constantemente el estado del botón (presionado o no), la comunicación es continua. Esto permite que la interfaz visual responda en tiempo real al accionar físico del usuario.  


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
