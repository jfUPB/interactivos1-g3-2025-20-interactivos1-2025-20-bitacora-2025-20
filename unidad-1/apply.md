# Unidad 1

## 🛠 Fase: Apply

Actividad 5

En tu bitácora: explica cómo funciona el sistema físico interactivo que acabamos de crear.

inpuit: boton, informacion serial
output: botom , lo visual
proceso: si recibo a, el cuadrado pinta roja, pero al oprimir n, el cuadro se pone verde
microbit, indicar el boton oprimido y dar cool down para cada mensaje

procesamiento: ver si el bootn se esta oprimiendo y el output responde

cada que enviamos, lo detecta una vez
 CONSIDERANDO TIMING Y POSIBLES PROBLEMAS

Actividad 6

Escribe el enlace a tu programa en el editor de p5.js.

[Mi codigo P5.JS DE ACTIVIDAD 6](https://editor.p5js.org/pinwinasio480/sketches/K-A8Z8I7Q)

Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).

```Javascript

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

Copia el código del micro:bit en la bitácora (recuerda insertarlo usando markdown y el lenguaje python).



from microbit import *

uart.init(baudrate=115200)
display.show(Image.SILLY)

while True:

    if button_a.is_pressed():
        uart.write('A')
    if button_b.is_pressed():
        uart.write('B')

    sleep(100)
      
