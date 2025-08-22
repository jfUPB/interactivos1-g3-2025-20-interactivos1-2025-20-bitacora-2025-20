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

# Actividad 7
Codigo microbit
```
from microbit import *
import utime
import radio

display.clear()

class Event:
    def __init__(self):
        self.value = 0

    def write(self,value):
        self.value = value

    def read(self):
        return self.value

    def clear(self):
        self.value = 0

class MicroBitSensors():
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")
        if button_b.was_pressed():
            event.write("B")
        if accelerometer.was_gesture("shake"):
            event.write("S")
        if pin_logo.is_touched():
            event.write("T")

class RemoteTask:
    def __init__(self):
        uart.init(baudrate=115200)

    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.write("A")
                if data[0] == ord('B'):
                    event.write("B")
                if data[0]== ord('S'):
                    event.write("S")
                if data[0] == ord('T'):
                    event.write("T")


class RadioRemote:
    def __init__(self):
        radio.config(group=69)
        radio.on

    def update(self):
        message = radio.receive()
        if message:
            if message == "A":
                event.write("A")
            elif message == "B":
                event.write("B")
            elif message == "S":
                event.write("S")
            elif message == "T":
                event.write("T")


class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            if event.read()== "A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read()== "B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read()== "S":
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if event.read()== "A":
                event.clear()
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if event.read()== "B":
                event.clear()
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if event.read()== "T":
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
sensors = MicroBitSensors()
remoteTask = RemoteTask()
radioRemote = RadioRemote()

while True:
    radioRemote.update()
    remoteTask.update()
    sensors.update()
    bombTask.update()
```
Codigo p5js
```
let bomb
let btnA, btnB, btnS, btnT

function setup() {
  createCanvas(400, 200)
  textAlign(CENTER, CENTER)
  textSize(32)
  bomb = new BombTask()

  btnA = createButton('A')
  btnA.position(50, height + 50)
  btnA.mousePressed(() => bomb.handleEvent('A'))

  btnB = createButton('B')
  btnB.position(120, height + 50)
  btnB.mousePressed(() => bomb.handleEvent('B'))

  btnS = createButton('S')
  btnS.position(190, height + 50)
  btnS.mousePressed(() => bomb.handleEvent('S'))

  btnT = createButton('T')
  btnT.position(260, height + 50)
  btnT.mousePressed(() => bomb.handleEvent('T'))
}

function draw() {
  background(0)
  bomb.update()
  fill(255)
  if (bomb.state === 'EXPLODED') {
    text("💀", width / 2, height / 2)
  } else {
    text(bomb.count, width / 2, height / 2)
  }
}

class BombTask {
  constructor() {
    this.PASSWORD = ['A', 'B', 'A']
    this.key = Array(this.PASSWORD.length).fill('')
    this.keyindex = 0
    this.count = 20
    this.startTime = millis()
    this.state = 'CONFIG'
  }

  _reset_key() {
    this.keyindex = 0
    for (let i = 0; i < this.key.length; i++) {
      this.key[i] = ''
    }
  }

  _append_key(k) {
    if (this.keyindex < this.key.length) {
      this.key[this.keyindex] = k
      this.keyindex += 1
    }
  }

  _check_password() {
    for (let i = 0; i < this.key.length; i++) {
      if (this.key[i] !== this.PASSWORD[i]) return false
    }
    return true
  }

  handleEvent(now_event) {
    if (this.state === 'CONFIG') {
      if (now_event === 'A') {
        this.count = min(this.count + 1, 60)
      } else if (now_event === 'B') {
        this.count = max(10, this.count - 1)
      } else if (now_event === 'S') {
        this._reset_key()
        this.startTime = millis()
        this.state = 'ARMED'
      }
    } else if (this.state === 'ARMED') {
      if (now_event === 'A') this._append_key('A')
      else if (now_event === 'B') this._append_key('B')

      if (this.keyindex === this.key.length) {
        if (this._check_password()) {
          this.count = 20
          this._reset_key()
          this.state = 'CONFIG'
        } else {
          this._reset_key()
        }
      }
    } else if (this.state === 'EXPLODED') {
      if (now_event === 'T') {
        this.count = 20
        this.startTime = millis()
        this._reset_key()
        this.state = 'CONFIG'
      }
    }
  }

  update() {
    if (this.state === 'ARMED') {
      if (millis() - this.startTime >= 1000) {
        this.startTime = millis()
        this.count -= 1
        if (this.count <= 0) {
          this.state = 'EXPLODED'
        }
      }
    }
  }
}
```
link 
https://editor.p5js.org/Changua666/sketches/NUW155hsM
