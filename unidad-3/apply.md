# Unidad 3


## 🛠 Fase: Apply

# Actividad 6 
```
let port;
let connectBtn;
let connectionInitialized = false;

let validChars = "ABST";

let bombaArmada = false;     
let secuencia = "";           
const secuenciaCorrecta = "ABA";

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

  textAlign(CENTER);
  text("Press A,B,S,T to simulate micro:bit keys", width / 2, height / 2);

  if (bombaArmada) {
    text("Bomba ARMADA! Ingresa secuencia ABA para desactivar", width / 2, height / 2 + 30);
  }

  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function keyPressed() {
  let keyValue = key.toUpperCase();
  if (validChars.includes(keyValue)) {
    console.log(keyValue);
    port.write(keyValue);

    if (bombaArmada) {
      secuencia += keyValue;
      if (secuencia.length > 3) {
        secuencia = secuencia.slice(-3); 
      }

      if (secuencia === secuenciaCorrecta) {
        bombaArmada = false;
        secuencia = "";
        console.log("Bomba Desactivada we");
      }
 
    } else {
      if (keyValue === 'S') {
        bombaArmada = true;
        secuencia = "";
        console.log("Bomba Activada we");
      }
    }
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
