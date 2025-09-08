# Unidad 3


## 🛠 Fase: Apply

**Actividad 6 y 7**

* Codigo p5.js
```js
let bombTask;
let serialTask;
let event;
let port;
let connectBtn;

function setup(){
  createCanvas(700,700);
  textAlign(CENTER,CENTER);
  textSize(28);

  port = createSerial();
  connectBtn = createButton('CONECTAR AL MICROBIT');
  connectBtn.position(150,250);
  connectBtn.mousePressed(connectBtnClick);

  event = new BombEvent();
  bombTask = new BombTask();
  serialTask = new SerialTask();
}

function draw(){
  background(0);

  serialTask.update();
  bombTask.update();

  bombTask.display();
}


class BombEvent {
  constructor(){
    this.value = null;
  }
  set(val){ this.value = val; }
  clear(){ this.value = null; }
  read(){ return this.value; }
}


class SerialTask {
  update(){
    if(port.availableBytes() > 0){
      let dataRx = port.read(1);
      if(dataRx === 'A') event.set('A');
      else if(dataRx === 'B') event.set('B');
      else if(dataRx === 'S') event.set('S');
      else if(dataRx === 'T') event.set('T');
    }
    if (!port.opened()) connectBtn.html('Connect to micro:bit');
    else connectBtn.html('Disconnect');
  }
}

class BombTask {
  constructor(){
    this.PASSWORD = ['A','B','A'];
    this.key = [];
    this.count = 20;
    this.startTime = millis();
    this.state = 'CONFIG';
  }
  update(){
    if(this.state === 'CONFIG'){
      if(event.read() === 'A'){
        event.clear();
        this.count = min(this.count + 1, 60);
      }
      else if(event.read() === 'B'){
        event.clear();
        this.count = max(this.count - 1, 10);
      }
      else if(event.read() === 'S'){
        event.clear();
        this.startTime = millis();
        this.state = 'ARMED';
      }
    }
    else if(this.state === 'ARMED'){
      if(millis() - this.startTime > 1000){
        this.count--;
        this.startTime = millis();
        if(this.count <= 0){
          this.state = 'EXPLODED';
        }
      }
      if(event.read() === 'A' || event.read() === 'B'){
        this.key.push(event.read());
        event.clear();
        if(this.key.length === this.PASSWORD.length){
          if(this.key.join('') === this.PASSWORD.join('')){
            this.state = 'CONFIG';
            this.count = 20;
          }
          this.key = [];
        }
      }
    }
    else if(this.state === 'EXPLODED'){
      if(event.read() === 'T'){
        event.clear();
        this.state = 'CONFIG';
        this.count = 20;
        this.startTime = millis();
      }
    }
  }
  display(){
    fill(255);
    if(this.state === 'CONFIG'){
      text(`CONFIG\n${this.count}`, width/2, height/2);
    }
    else if(this.state === 'ARMED'){
      text(`ARMED\n${this.count}`, width/2, height/2);
    }
    else if(this.state === 'EXPLODED'){
      fill(255,0,0);
      text("EXPLODED", width/2, height/2);
    }
  }
}
function connectBtnClick(){
  if(!port.opened()){
    port.open('MicroPython',115200);
  } else {
    port.close();
  }
}

function keyPressed() {
  if (key === 'A') event.set('A');
  else if (key === 'B') event.set('B');
  else if (key === 'S') event.set('S');
  else if (key === 'T') event.set('T');
}
```
* Codigo Micro.bit
```js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.SILLY)

while True:
    display.show(Image.SILLY)
    if button_a.was_pressed():
        uart.write('A')
        display.show(Image.TRIANGLE)
        sleep(200)
    if button_b.was_pressed():
        uart.write('B')
        display.show(Image.DIAMOND)
        sleep(200)
    if accelerometer.was_gesture('shake'):
        uart.write('S')
        display.show(Image.SKULL)
        sleep(200)
    if pin_logo.is_touched():
        uart.write('T')
        display.show(Image.SILLY)
        sleep(200)
```
* Link editor p5.js

[Link para el Editor de p5.js](https://editor.p5js.org/alejogonzdav41/sketches/0CdhgIbQO)


