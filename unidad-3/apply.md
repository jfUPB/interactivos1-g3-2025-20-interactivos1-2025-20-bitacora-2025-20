# Unidad 3

## 🛠 Fase: Apply

### Actividad 06 - 20/08/2025

**Versión 1 código:**

```javascript
let estado = "inactiva";
let tiempo = 30;
let inicioTiempo;

let secuencia = ["A", "B", "A"];
let indice = 0;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(28);
}

function draw() {
  if (estado === "inactiva") {
    background(200);
    fill(50);
    ellipse(200, 200, 120, 120);
    fill(0);
    text("Tiempo: " + tiempo, 200, 100);
    text("Presiona ESPACIO para activar", 200, 310);
  } else if (estado === "armando") {
    background(0);
    fill(100);
    ellipse(200, 200, 120, 120);

    let transcurrido = int((millis() - inicioTiempo) / 1000);
    let restante = tiempo - transcurrido;

    fill(255, 0, 0);
    text(restante, 200, 200);

    if (restante <= 0) {
      estado = "explotada";
    }
  } else if (estado === "explotada") {
    background(255, 0, 0);
    fill(255);
    text("💥BOOM💥", 200, 200);
    textSize(20);
    text("Presiona R para reiniciar", 200, 350);
  } else if (estado === "desactivada") {
    background(0, 200, 0);
    fill(255);
    ellipse(200, 200, 120, 120);
    text("Bomba desactivada", 200, 200);
    textSize(20);
    text("Presiona R para reiniciar", 200, 350);
  }
}

function keyPressed() {
  if (estado === "inactiva") {
    if (key === " ") {
      estado = "armando";
      inicioTiempo = millis();
      indice = 0;
    } else if (key === "A" || key === "a") {
      tiempo = min(60, tiempo + 1);
    } else if (key === "B" || key === "b") {
      tiempo = max(10, tiempo - 1);
    }
  } else if (estado === "armando") {
    if (key === secuencia[indice]) {
      indice++;
      if (indice === secuencia.lenght) {
        estado = "desactivada";
      }
    } else {
      indice = 0;
    }
  } else if (
    (estado === "explotada" || estado === "desactivada") &&
    (key === "r" || key === "R")
  ) {
    estado = "inactiva";
  }
}
```

