# Unidad 3


## 🛠 Fase: Apply

## Actividad 6

### Crear la bomba en p5.js

````

/**
 * Bomba 2.0 en p5.js — Máquina de Estados (CONFIG, ARMED, EXPLODED)
 * Controles con teclado:
 * - A: ++count (máx 60) en CONFIG; parte de clave en ARMED
 * - B: --count (mín 10) en CONFIG; parte de clave en ARMED
 * - S: armar (pasar a ARMED)
 * - T: reset desde EXPLODED
 * Temporizador: 1s en ARMED (no bloqueante, usando millis())
 */

let eventQueue = [];

class BombFSM {
  constructor() {
    this.STATE = { CONFIG: 'CONFIG', ARMED: 'ARMED', EXPLODED: 'EXPLODED' };
    this.state = this.STATE.CONFIG;

    this.count = 20;            // [10..60]
    this.lastTick = millis();   // para TIMER de 1s

    this.PASSWORD = ['A','B','A'];
    this.key = new Array(this.PASSWORD.length).fill('');
    this.keyindex = 0;
  }

  // Procesa eventos genéricos
  processEvent(ev) {
    if (!ev) return;

    switch (this.state) {
      case this.STATE.CONFIG:
        if (ev === 'A') {
          this.count = Math.min(this.count + 1, 60);
        } else if (ev === 'B') {
          this.count = Math.max(this.count - 1, 10);
        } else if (ev === 'S') {
          this.lastTick = millis();
          this.state = this.STATE.ARMED;
        }
        break;

      case this.STATE.ARMED:
        if (ev === 'TIMER') {
          this.count -= 1;
          if (this.count <= 0) {
            this.count = 0;
            this.state = this.STATE.EXPLODED;
          }
        } else if (ev === 'A' || ev === 'B') {
          this.key[this.keyindex] = ev;
          this.keyindex++;
          if (this.keyindex === this.PASSWORD.length) {
            // Validar clave
            let ok = true;
            for (let i = 0; i < this.PASSWORD.length; i++) {
              if (this.key[i] !== this.PASSWORD[i]) { ok = false; break; }
            }
            if (ok) {
              this.count = 20;
              this.keyindex = 0;
              this.state = this.STATE.CONFIG;
            } else {
              this.keyindex = 0;
            }
          }
        }
        break;

      case this.STATE.EXPLODED:
        if (ev === 'T') {
          this.count = 20;
          this.lastTick = millis();
          this.state = this.STATE.CONFIG;
        }
        break;
    }
  }

  // Genera el evento TIMER cada 1 segundo
  update() {
    if (this.state === this.STATE.ARMED) {
      const now = millis();
      if (now - this.lastTick >= 1000) {
        this.lastTick = now;
        eventQueue.push('TIMER');
      }
    }
  }

  // Dibujo en canvas (acciones fuera de eventos)
  render() {
    background(20);
    textAlign(CENTER, CENTER);
    noStroke();

    fill(255);
    textSize(20);
    text('BOMBA 2.0 — FSM en p5.js', width/2, 30);

    textSize(16);
    text(`Estado: ${this.state}`, width/2, 70);

    textSize(80);
    if (this.state === this.STATE.EXPLODED) {
      fill(255, 80, 80);
      text('💀', width/2, height/2);
    } else {
      fill(200);
      text(this.count.toString(), width/2, height/2);
    }

    fill(180);
    textSize(14);
    text('Teclado: A(+), B(-), S(armar), T(reset)', width/2, height - 30);
  }
}

let bomb;

function setup() {
  createCanvas(420, 320);
  bomb = new BombFSM();
}

function draw() {
  // 1) Actualización no bloqueante (TIMER)
  bomb.update();

  // 2) Consumir eventos en cola
  while (eventQueue.length > 0) {
    const ev = eventQueue.shift();
    bomb.processEvent(ev);
  }

  // 3) Dibujar
  bomb.render();
}

// Capturar eventos de teclado
function keyPressed() {
  const k = key.toUpperCase();
  if ('ABST'.includes(k)) eventQueue.push(k);
}


````

