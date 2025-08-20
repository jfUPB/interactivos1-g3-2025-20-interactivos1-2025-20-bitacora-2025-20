# Unidad 3


## 🛠 Fase: Apply

### Actividad 6.  
```javascript
// ------------------------------
// Estados
const STATE_CONFIG = 0;
const STATE_ARMED = 1;
const STATE_EXPLODED = 2;

// ------------------------------
// Variables globales
let current_state = STATE_CONFIG;
let timer_value = 20;   // tiempo inicial en segundos
let min_time = 10;
let max_time = 60;
let countdown_time = 0;
let last_time = 0;

// Secuencia de desactivación
const PASSWORD = ['A','B','A'];
let keySequence = [];  // <-- arreglo separado para secuencia

// ------------------------------
// Funciones auxiliares
function showTime(t) {
  textSize(32);
  fill(0);
  textAlign(CENTER, CENTER);
  text(t, width / 2, height / 2);
}

function explode() {
  background(255, 0, 0);
  textSize(40);
  fill(255);
  textAlign(CENTER, CENTER);
  text("💥 EXPLOSIÓN 💥", width / 2, height / 2);
}

// ------------------------------
// p5.js
function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);
  last_time = millis();
}

function draw() {
  background(220);

  if (current_state === STATE_CONFIG) {
    showTime(timer_value);
    // Mostrar instrucciones
    textSize(16);
    text("Presiona A/B para ajustar tiempo, S para iniciar", width/2, height/2 + 50);
  } 
  else if (current_state === STATE_ARMED) {
    let current_time = millis();
    if (current_time - last_time >= 1000) {   // cada segundo
      countdown_time -= 1;
      last_time = current_time;
    }
    if (countdown_time > 0) {
      showTime(countdown_time);
      // Mostrar instrucciones
      textSize(16);
      text("Ingresa la contraseña (A,B,A) para desactivar", width/2, height/2 + 50);
    } else {
      current_state = STATE_EXPLODED;  // evento: tiempo agotado
    }
  } 
  else if (current_state === STATE_EXPLODED) {
    explode();
    // Mostrar instrucciones
    textSize(16);
    text("Presiona T para reiniciar", width/2, height/2 + 50);
  }
}

// ------------------------------
// Eventos de teclado
function keyPressed() {
  const k = String(key).toUpperCase(); // Asegurar mayúsculas y convertir a string

  if (current_state === STATE_CONFIG) {
    if (k === 'A') {
      timer_value = min(timer_value + 1, max_time);
    }
    else if (k === 'B') {
      timer_value = max(timer_value - 1, min_time);
    }
    else if (k === 'S') {
      current_state = STATE_ARMED;
      countdown_time = timer_value;
      last_time = millis();
      keySequence = [];
    }
  }
  else if (current_state === STATE_ARMED) {
    if (k === 'A' || k === 'B') {
      keySequence.push(k);
      // Mantener solo los últimos N caracteres (longitud de la contraseña)
      if (keySequence.length > PASSWORD.length) {
        keySequence.shift();
      }
      
      // Verificar si coincide con la contraseña
      if (arraysEqual(keySequence, PASSWORD)) {
        current_state = STATE_CONFIG;
        timer_value = 20;
        keySequence = [];
      }
    }
  }
  else if (current_state === STATE_EXPLODED) {
    if (k === 'T') {
      current_state = STATE_CONFIG;
      timer_value = 20;
      keySequence = [];
    }
  }
  
  return false; // Prevenir comportamiento por defecto
}

// Función para comparar arrays
function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) {
    if (a[i] !== b[i]) return false;
  }
  return true;
}
```

### Actividad 7.

micro:bit:   
```javascript
from microbit import *

while True:
    if button_a.is_pressed() and not button_b.is_pressed():
        uart.write("A\n")
        sleep(300)
    elif button_b.is_pressed() and not button_a.is_pressed():
        uart.write("B\n")
        sleep(300)
    elif button_a.is_pressed() and button_b.is_pressed():
        uart.write("S\n")  # iniciar
        sleep(300)

    # reinicio (agítalo para simular "T")
    if accelerometer.was_gesture("shake"):
        uart.write("T\n")
        sleep(500)
```
p5:  
```javascript
// ------------------------------
// Estados
const STATE_CONFIG = 0;
const STATE_ARMED = 1;
const STATE_EXPLODED = 2;

// ------------------------------
// Variables globales
let current_state = STATE_CONFIG;
let timer_value = 20;   // tiempo inicial en segundos
let min_time = 10;
let max_time = 60;
let countdown_time = 0;
let last_time = 0;

// Secuencia de desactivación
const PASSWORD = ['A','B','A'];
let keySequence = [];  

// ------------------------------
// Serial
let serial; 

// ------------------------------
// Funciones auxiliares
function showTime(t) {
  textSize(32);
  fill(0);
  textAlign(CENTER, CENTER);
  text(t, width / 2, height / 2);
}

function explode() {
  background(255, 0, 0);
  textSize(40);
  fill(255);
  textAlign(CENTER, CENTER);
  text("💥 EXPLOSIÓN 💥", width / 2, height / 2);
}

// ------------------------------
// p5.js
function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);
  last_time = millis();

  // --- Serial setup ---
  serial = createSerial();

  serial.on("list", (ports) => print("Puertos disponibles:", ports));
  serial.on("connected", () => serial.requestPort());
  serial.on("open", () => print("Puerto abierto"));
  serial.on("data", gotData);
  serial.on("close", () => print("Puerto cerrado"));
  serial.on("error", (err) => print("Error de serial:", err));
}

function draw() {
  background(220);

  if (current_state === STATE_CONFIG) {
    showTime(timer_value);
    textSize(16);
    text("Presiona A/B (o botones micro:bit) para ajustar\nS para iniciar", width/2, height/2 + 50);
  } 
  else if (current_state === STATE_ARMED) {
    let current_time = millis();
    if (current_time - last_time >= 1000) {
      countdown_time -= 1;
      last_time = current_time;
    }
    if (countdown_time > 0) {
      showTime(countdown_time);
      textSize(16);
      text("Ingresa la contraseña (A,B,A) para desactivar", width/2, height/2 + 50);
    } else {
      current_state = STATE_EXPLODED;
    }
  } 
  else if (current_state === STATE_EXPLODED) {
    explode();
    textSize(16);
    text("Presiona T para reiniciar", width/2, height/2 + 50);
  }
}

// ------------------------------
// Eventos de teclado
function keyPressed() {
  processInput(String(key).toUpperCase());
  return false;
}

// ------------------------------
// Evento serial (Micro:bit)
function gotData() {
  let data = serial.readLine().trim();
  if (data.length > 0) {
    processInput(data);
  }
}

// ------------------------------
// Procesar entrada (teclado o Micro:bit)
function processInput(k) {
  if (current_state === STATE_CONFIG) {
    if (k === 'A') timer_value = min(timer_value + 1, max_time);
    else if (k === 'B') timer_value = max(timer_value - 1, min_time);
    else if (k === 'S') {
      current_state = STATE_ARMED;
      countdown_time = timer_value;
      last_time = millis();
      keySequence = [];
    }
  }
  else if (current_state === STATE_ARMED) {
    if (k === 'A' || k === 'B') {
      keySequence.push(k);
      if (keySequence.length > PASSWORD.length) keySequence.shift();
      if (arraysEqual(keySequence, PASSWORD)) {
        current_state = STATE_CONFIG;
        timer_value = 20;
        keySequence = [];
      }
    }
  }
  else if (current_state === STATE_EXPLODED) {
    if (k === 'T') {
      current_state = STATE_CONFIG;
      timer_value = 20;
      keySequence = [];
    }
  }
}

// ------------------------------
// Comparar arrays
function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) if (a[i] !== b[i]) return false;
  return true;
}
```
Mi codigo:
[https://editor.p5js.org/ ](https://editor.p5js.org/mafora12/sketches/IMcuTpksYo)
