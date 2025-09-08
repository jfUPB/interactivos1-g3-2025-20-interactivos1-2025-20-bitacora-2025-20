# Unidad 3


## 🛠 Fase: Apply

### Actividad 06
### Crear la bomba en p5.js

#### En esta actividad vas a transferir la técnica de programación con máquinas de estado a p5.js.

#### Crea la bomba versión 2.0 en p5.js. No olvides que al aplicar la técnica de máquinas de estado en micro:bit se evitaba colocar acciones por fuera de los eventos. En el caso de p5.js será necesario que tengas acciones por fuera de eventos porque es necesario dibujar el canvas en cada frame.

```Javascript

let state = "CONFIG";
let count = 20;
let password = ['A', 'B', 'A'];
let inputKeys = [];
let keyIndex = 0;
let startTime;

function setup() {
    createCanvas(400, 400);
    textAlign(CENTER, CENTER);
    textSize(32);
    startTime = millis();
}

function draw() {
    background(220);


    if (state == "CONFIG") {
        text("Countdown: " + count, width / 2, height / 2);

    } else if (state == "ARMED") {
        if (millis() - startTime > 1000) {
            startTime = millis();
            count--;
            if (count == 0) {
                state = "EXPLODED";
            }
        }
        text("Countdown: " + count, width / 2, height / 2);

    } else if (state == "EXPLODED") {
        text("💀", width / 2, height / 2);
    }
}

function keyPressed() {
    if (state == "CONFIG") {
        if (key == 'A' || key == 'a') {
            count = min(count + 1, 60);
        } else if (key == 'B' || key == 'b') {
            count = max(count - 1, 10);
        } else if (key == 'S' || key == 's') {
            startTime = millis();
            state = "ARMED";
        }

    } else if (state == "ARMED") {
        if (key == 'A' || key == 'B') {
            inputKeys[keyIndex++] = key;
        }
        if (keyIndex == password.length) {
            let passOk = inputKeys.every((k, i) => k == password[i]);
            if (passOk) {
                count = 20;
                state = "CONFIG";
            }

            keyIndex = 0;
            inputKeys = [];
        }

    } else if (state == "EXPLODED") {
        if (key == 'T' || key == 't') {
            count = 20;
            startTime = millis();
            state = "CONFIG";
        }
    }
}

```

*1. El código fuente de la bomba en p5.js. No olvides el propósito de esta evaluación: utilizar la técnica de máquina de estados que estamos trabajando en el curso.*

### Actividad 07
### Bomba en p5.js + controles del micro:bit

#### Vas a practicar de nuevo la técnica de máquina de estados y eventos genéricos, pero esta vez vas a controlar la bomba desde el micro:bit y desde p5.js. TEN PRESENTE que la bomba estará corriendo en p5.js, pero deberás controlarla también desde los botones del micro:bit mediante el puerto serial.

*NOTA: De antemano voy a ser honesto con esta actividad. Debido a que el tiempo de la clase del Apply se me fue haciendo la actividad 6, no tuve tiempo de llevar a cabo esta actividad cuando se debia (se que es un error mio y no voy a ganar nada sermoneando como en este momento jejeje, pero igual prefiero aclararlo de antemano) por lo que lo que se vera de codigo tanto de Python como p5.js en esta actividad es como he estdado pensando en como podria funcionar usando los botones del serial (aunque no tenga el serial a la mano).*

1. Código p5.js

```Javascript
let state = "CONFIG";
let count = 20;
let password = ['A', 'B', 'A'];
let inputKeys = [];
let keyIndex = 0;
let startTime;
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    textAlign(CENTER, CENTER);
    textSize(32);
    startTime = millis();
    port = createSerial();
    connectBtn = createButton('Conectar micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(() => port.open());;
}

function draw() {
    background(220);

    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        if (dataRx) {
            handleInput(dataRx);
        }
    }

    if (state == "CONFIG") {
        text("Countdown: " + count, width / 2, height / 2);

    } else if (state == "ARMED") {
        if (millis() - startTime > 1000) {
            startTime = millis();
            count--;
            if (count == 0) {
                state = "EXPLODED";
            }
        }
        text("Countdown: " + count, width / 2, height / 2);

    } else if (state == "EXPLODED") {
        text("💀", width / 2, height / 2);
    }
}

function keyPressed() {
    handleInput(key);
}

function handleInput(input) {
    if (state == "CONFIG") {
        if (input == 'A' || input == 'a') {
            count = min(count + 1, 60);
        } else if (input == 'B' || input == 'b') {
            count = max(count - 1, 10);
        } else if (input == 'S' || input == 's') {
            startTime = millis();
            state = "ARMED";
        }

    } else if (state == "ARMED") {
        if (input == 'A' || input == 'B') {
            inputKeys[keyIndex++] = input;
        }
        if (keyIndex == password.length) {
            let passOk = inputKeys.every((k, i) => k == password[i]);
            if (passOk) {
                count = 20;
                state = "CONFIG";
            }
            keyIndex = 0;
            inputKeys = [];
        }

    } else if (state == "EXPLODED") {
        if (input == 'T' || input == 't') {
            count = 20;
            startTime = millis();
            state = "CONFIG";
        }
    }
}
```
   
2. Enlace al editor de p5.js con tu código.

[ENLANCE AL P5.JS DE ESTA IMPROVISACIÓN](https://editor.p5js.org/pinwinasio480/sketches/OxuP-D5fl)
   
3. Código del micro:bit.

```Python

from microbit import *

uart.init(baudrate=115200)
display.show(Image.SILLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(300)
    if button_b.is_pressed():
        uart.write('B')
        sleep(300)
    if accelerometer.was_gesture('shake'):
        uart.write('S')
        sleep(300)
    if pin_logo.is_touched():
        uart.write('T')
        sleep(300)
``` 







