# Unidad 1

## 🛠 Fase: Apply

### Actividad 05
* Input microbit: boton A
* Output microbit: Mensaje
* Proceso: Al undir el boton A se envia el mensaje
* Input p5.js: boton, informacion del serial
* Output p5.js: boton, el cuadro verde y rojo
* Proceso: si recibe una "A" se pinta de rojo 

Se podria decir que este codigo sirve para ir variando de colores dependiendo de si se unde un boton o no.

### Actividad 6

https://editor.p5js.org/Ayepes2402/sketches/X8WaNgrqv

```
from microbit import *

uart.init(baudrate=115200)

while True:
      if button_a.was_pressed():
          uart.write('A')
      elif  button_a.was_pressed():
              uart.write('B')
```
```
 let port;
  let connectBtn;
  let connectionInitialized = false;
x=200;
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
circle(x, 200, 50);
    if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }

    if (port.availableBytes() > 0) {
      let dataRx = port.read(1);
      if (dataRx == "A") {
       x-=5; 
      } else if (dataRx == "B") {
     x+=5;
      }
    }

    
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

