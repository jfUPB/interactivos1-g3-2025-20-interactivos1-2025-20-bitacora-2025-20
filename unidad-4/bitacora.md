
# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](URL)

Código a modificar:

``` js

```

[Enlace a la aplicación modificada](URL)

Código modificado:

``` js

```

## Video

[Video demostratativo](URL)


Esta entrega se realizó fuera de plazo

# Unidad 4


## 🛠 Fase: Apply

### Código original [aquí](https://editor.p5js.org/generative-design/sketches/P_2_1_2_01):

```js
'use strict';

var tileCount = 20;
var actRandomSeed = 0;

var circleAlpha = 130;
var circleColor;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);

  background(255);

  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(mouseY / 60);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {

      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var shiftX = random(-mouseX, mouseX) / 20;
      var shiftY = random(-mouseX, mouseX) / 20;

      ellipse(posX + shiftX, posY + shiftY, mouseY / 15, mouseY / 15);
    }
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}
```

### Código modificado [aquí](https://editor.p5js.org/n4ndeZzz/sketches/jkiQ-vJ2y):
```js
'use strict';

// --- Variables del sketch generativo ---
let tileCount = 20;
let actRandomSeed = 0;
let circleAlpha = 130;
let circleColor;

// --- Variables para la comunicación con Micro:bit ---
let port;
let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevAState = false;
let prevBState = false;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);

  // Botón y configuración para conectar al Micro:bit
  port = createSerial();
  let connectButton = createButton("Conectar micro:bit");
  connectButton.mousePressed(connectToMicroBit);
}

function connectToMicroBit() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}

function draw() {
  // --- Lectura de datos del Micro:bit ---
  if (port.opened() && port.availableBytes() > 0) {
    let datosSeriales = port.readUntil("\n");
    if (datosSeriales) {
      let values = datosSeriales.trim().split(",");
      if (values.length === 4) {
        // Asignamos los valores del acelerómetro
        microBitX = int(values[0]);
        microBitY = int(values[1]);
        // Asignamos el estado de los botones
        microBitAState = (values[2] === "True");
        microBitBState = (values[3] === "True");
      }
    }
  }

  // --- Lógica de dibujo del sketch generativo ---
  translate(width / tileCount / 2, height / tileCount / 2);
  background(255);
  
  // Usamos una semilla para que el patrón sea estático hasta presionar el Botón A
  randomSeed(actRandomSeed);

  stroke(circleColor);
  // Reemplazamos mouseY por el valor absoluto de microBitY
  strokeWeight(max(1, abs(microBitY) / 60));

  for (let gridY = 0; gridY < tileCount; gridY++) {
    for (let gridX = 0; gridX < tileCount; gridX++) {
      let posX = width / tileCount * gridX;
      let posY = height / tileCount * gridY;

      // Reemplazamos mouseX por microBitX para el desplazamiento
      let shiftX = random(-microBitX, microBitX) / 20;
      let shiftY = random(-microBitX, microBitX) / 20;

      // Reemplazamos mouseY por el valor absoluto de microBitY para el tamaño
      let circleSize = max(5, abs(microBitY) / 15);
      ellipse(posX + shiftX, posY + shiftY, circleSize, circleSize);
    }
  }

  // --- Lógica de los botones del Micro:bit ---
  // Si se presiona el Botón A, genera una nueva semilla (nuevo patrón)
  if (microBitAState && !prevAState) {
    actRandomSeed = random(100000);
  }

  // Si se presiona el Botón B, guarda el canvas como PNG
  if (microBitBState && !prevBState) {
    saveCanvas('microbit_pattern', 'png');
  }

  // Actualizamos el estado previo de los botones
  prevAState = microBitAState;
  prevBState = microBitBState;
}

// También puedes conservar la opción de guardar con el teclado
function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas('keyboard_pattern', 'png');
}
```



https://github.com/user-attachments/assets/fb72bf6d-1f6b-4e75-bd14-4e6121438d2f
