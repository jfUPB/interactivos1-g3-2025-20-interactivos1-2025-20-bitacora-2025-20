# Unidad 3

## 🔎 Fase: Set + Seek

# Actividad 5

🔹 Vector 1 – Configuración inicial

Estado inicial: CONFIG, contador = 20.

Entrada: A (botón o serial).

Salida esperada: contador incrementa en +1 (máx. 60), display muestra nuevo valor.

🔹 Vector 2 – Configuración restando tiempo

Estado inicial: CONFIG, contador = 20.

Entrada: B.

Salida esperada: contador decrementa en -1 (mín. 10), display muestra nuevo valor.

🔹 Vector 3 – Armar la bomba

Estado inicial: CONFIG.

Entrada: S (shake / serial).

Salida esperada:

Pasa a estado ARMED.

Empieza a decrementar contador cada segundo.

🔹 Vector 4 – Introducción de contraseña correcta

Estado inicial: ARMED, contador en curso.

Secuencia entrada: A, B, A.

Salida esperada:

Contraseña validada como correcta.

Estado vuelve a CONFIG.

Contador reiniciado a 20.

🔹 Vector 5 – Introducción de contraseña incorrecta

Estado inicial: ARMED.

Secuencia entrada: A, A, B.

Salida esperada:

Contraseña invalidada.

Estado permanece en ARMED.

keyindex se reinicia a 0.

🔹 Vector 6 – Explosión por tiempo agotado

Estado inicial: ARMED, contador = 1.

Entrada: esperar > 1 segundo sin introducir contraseña correcta.

Salida esperada:

Contador llega a 0.

Estado pasa a EXPLODED.

Display muestra Image.SKULL.

🔹 Vector 7 – Reinicio tras explosión

Estado inicial: EXPLODED.

Entrada: T (logo touch / serial).

Salida esperada:

Estado pasa a CONFIG.

Contador reiniciado a 20.

Display muestra 20.

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


