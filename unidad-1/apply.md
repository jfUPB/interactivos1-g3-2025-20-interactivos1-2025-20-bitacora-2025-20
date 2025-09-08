# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

En tu bitácora: explica cómo funciona el sistema físico interactivo que acabamos de crear.

Antes de explicar el funcionamiento del sistema interactivo visto en clase, es importante destacar cuales son los inputzs y outputs del mismo:

Inputs: El boton que oprime el usuario ("A"), y la información serial.
Outputs: El boton de "Connect to micro:bit/Disconnect" y lo visual (color del cuadrado).

Ya con esto, el funcionamiento es simple, cuando se activa el sistema, aparecera un cuadrado blanco, al conectar el micro:bit, si el usuario oprime el boton a
el cuadrado se volvera rojo e indicara que se hizo con una 'A', al hacer algo opuesto a esto, se señalara con una 'N' mientras el cuadrado se vuelve verde,
en si, se encarga de indicar el boton oprimido, evidenciarlo con el color del cuadrado (el output da respuesta) y dar cool down para cada mensaje, debido a que, cada que enviamos 
una acción con el botón o contrario, el sistema lo detecta una vez, considerando timing y posibles problemas.

### Actividad 6

Escribe el enlace a tu programa en el editor de p5.js.

[Mi codigo P5.JS DE ACTIVIDAD 6](https://editor.p5js.org/pinwinasio480/sketches/K-A8Z8I7Q)

Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).

``` js

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

  ellipseMode(CENTER);
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

Copia el código del micro:bit en la bitácora (recuerda insertarlo usando markdown y el lenguaje python).

```Python

from microbit import *

uart.init(baudrate=115200)
display.show(Image.SILLY)

while True:

    if button_a.is_pressed():
        uart.write('A')
    if button_b.is_pressed():
        uart.write('B')

    sleep(100)
```
      

