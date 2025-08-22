# Unidad 3


## 🛠 Fase: Apply

### Actividad #6:  

```js
const STATE_CONFIG   = "CONFIG";
const STATE_ARMED    = "ARMED";
const STATE_EXPLODED = "EXPLODED";

const MIN_TIME = 10;
const MAX_TIME = 60;
const TICK_MS  = 1000;
const PASSWORD = ["A", "B", "A"];

let state = STATE_CONFIG;
let timeSet = 20;
let remaining = timeSet;
let lastTick = 0;
let passBuffer = [];
let flashes = 0;
let flashTimer = 0;
let flashOn = false;

function setup() {
  createCanvas(480, 300);
  textFont("monospace");
  textAlign(CENTER, CENTER);
}

function draw() {
  background(20);
  drawStatePanel();

  switch (state) {
    case STATE_CONFIG:
      remaining = timeSet;
      break;
    case STATE_ARMED:
      if (millis() - lastTick >= TICK_MS) {
        lastTick = millis();
        remaining = max(0, remaining - 1);
        if (remaining === 0) transitionTo(STATE_EXPLODED);
      }
      break;
    case STATE_EXPLODED:
      const FLASH_MS = 200;
      if (millis() - flashTimer >= FLASH_MS) {
        flashTimer = millis();
        flashOn = !flashOn;
        if (flashOn) flashes++;
      }
      if (flashOn) fill(200, 0, 0, 120), rect(0, 0, width, height);
      break;
  }
}

function keyPressed() {
  const k = key.toUpperCase();

  if (k === "T") {
    transitionTo(STATE_CONFIG);
    return;
  }

  if (state === STATE_CONFIG) {
    if (k === "A") timeSet = min(MAX_TIME, timeSet + 1);
    if (k === "B") timeSet = max(MIN_TIME, timeSet - 1);
    if (k === "S") transitionTo(STATE_ARMED);
  }
  else if (state === STATE_ARMED) {
    if (k === "A" || k === "B") {
      passBuffer.push(k);
      if (passBuffer.length > PASSWORD.length) passBuffer.shift();
      if (arraysEqual(passBuffer, PASSWORD)) {
        transitionTo(STATE_CONFIG);
      }
    }
  }
}

function transitionTo(next) {
  if (next === STATE_CONFIG) {
    state = STATE_CONFIG;
    remaining = timeSet;
    passBuffer = [];
    flashes = 0;
    flashOn = false;
  }
  else if (next === STATE_ARMED) {
    state = STATE_ARMED;
    remaining = timeSet;
    lastTick = millis();
    passBuffer = [];
  }
  else if (next === STATE_EXPLODED) {
    state = STATE_EXPLODED;
    flashes = 0;
    flashOn = false;
    flashTimer = millis();
  }
}

function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) if (a[i] !== b[i]) return false;
  return true;
}

function drawStatePanel() {
  push();
  translate(width/2, height/2);
  noStroke();
  stroke(120); noFill(); rectMode(CENTER); rect(0, 0, 360, 160, 12); noStroke();

  textSize(28);
  fill(stateColor());
  text(state, 0, -20);

  textSize(36);
  fill(255);
  text((state === STATE_ARMED || state === STATE_CONFIG) ? `${state === STATE_CONFIG ? timeSet : remaining}s` : "0s", 0, 40);
  pop();
}

function stateColor() {
  if (state === STATE_CONFIG) return color(80, 180, 255);
  if (state === STATE_ARMED)  return color(255, 190, 60);
  return color(255, 80, 80);
}
```
___________________________________

### Actividad #7: 

__Código de micro:bit__  
```py
from microbit import *
import utime

# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

# Variables
time_set = 20   # Tiempo inicial en segundos
min_time = 10
max_time = 60

current_state = STATE_CONFIG
start_time = 0
remaining_time = time_set

# Inicializar comunicación serial (UART)
uart.init(baudrate=115200)

while True:
    # ------------------ LECTURA DE SERIAL ------------------
    if uart.any():
        data = uart.read(1)  # leemos un byte
        if data is not None:
            cmd = data.decode("utf-8").strip()
        
        # Simulamos teclas
            if cmd == "A":   # subir tiempo
                if time_set < max_time:
                    time_set += 1
        
            elif cmd == "B": # bajar tiempo
                if time_set > min_time:
                    time_set -= 1
        
            elif cmd == "S": # iniciar (simula shake)
                current_state = STATE_ARMED
            remaining_time = time_set
            start_time = utime.ticks_ms()
    # ------------------ ESTADOS ------------------
    if current_state == STATE_CONFIG:
        display.show(str(time_set))
        
        if button_a.was_pressed():
            if time_set < max_time:
                time_set += 1
        
        if button_b.was_pressed():
            if time_set > min_time:
                time_set -= 1
        
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            remaining_time = time_set
            start_time = utime.ticks_ms()
    
    elif current_state == STATE_ARMED:
        display.show(str(remaining_time))
        
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= 1000:
            remaining_time -= 1
            start_time = utime.ticks_ms()
        
        if remaining_time <= 0:
            current_state = STATE_EXPLOSION
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            time_set = 20
            display.clear()
    
    elif current_state == STATE_EXPLOSION:
        for i in range(5):
            display.show(Image.SKULL)
            utime.sleep_ms(200)
            display.clear()
            utime.sleep_ms(200)
        
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            time_set = 20
            display.clear()
```
_____________________________________________________________  
[LINK](https://editor.p5js.org/n4ndeZzz/sketches/21nCBdXyE)  

__Código de p5.js__    
```js
let port;
let writer;
let reader;
let state = "CONFIG";
let timeSet = 20;  // valor inicial, luego micro:bit puede actualizarlo

async function setup() {
  createCanvas(400, 300);
  textAlign(CENTER, CENTER);
  textSize(24);

  // Abrir puerto serial
  if ("serial" in navigator) {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    writer = port.writable.getWriter();
    reader = port.readable.getReader();
    readLoop();
  }
}

// Bucle de lectura desde el micro:bit
async function readLoop() {
  while (true) {
    const { value, done } = await reader.read();
    if (done) {
      console.log("Serial cerrado");
      break;
    }
    if (value) {
      let textIn = new TextDecoder().decode(value).trim();
      console.log("Microbit dice:", textIn);

      // ejemplo: si microbit envía "STATE:ARMED" o "TIME:19"
      if (textIn.startsWith("STATE:")) {
        state = textIn.split(":")[1];
      } else if (textIn.startsWith("TIME:")) {
        timeSet = int(textIn.split(":")[1]);
      }
    }
  }
}

// Enviar un carácter al micro:bit
async function sendCommand(cmd) {
  if (writer) {
    const data = new TextEncoder().encode(cmd);
    await writer.write(data);
  }
}

function draw() {
  background(30);
  fill(255);

  text("Estado: " + state, width/2, height/3);
  text("Tiempo: " + timeSet + "s", width/2, height/2);

  textSize(16);
  text("A: +Tiempo | B: -Tiempo | S: Start | T: Reset", width/2, height-40);
  textSize(24);
}

// Control por teclado
function keyPressed() {
  if (key === "A") {
    sendCommand("A");
  } else if (key === "B") {
    sendCommand("B");
  } else if (key === "S") {
    sendCommand("S");
  } else if (key === "T") {
    sendCommand("T");
  }
}
```
