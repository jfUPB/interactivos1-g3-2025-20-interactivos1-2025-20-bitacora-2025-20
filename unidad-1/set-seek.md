# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01 - 16/07/2025

#### ¿Qué es un sistema físico interactivo?

Es un sistema que percibe información del entorno físico mediante **sensores** (inputs), procesa esa información, y responde a ella mediante **actuadores** (outputs), generando una **interacción dinámica** con el usuario o el entorno.

#### ¿Cómo se relacionan los sistemas físicos interactivos con mi perfil profesional?

Los sistemas físicos interactivos se relacionan con **mi perfil profesional** porque me permiten crear productos donde el usuario interactúa con el mundo **físico y digital al mismo tiempo**.  Esto es fundamental para desarrollar **videojuegos, instalaciones, interfaces** o **experiencias educativas** que respondan a acciones reales mediante sensores y actuadores.

### Actividad 02 - 18/07/2025

Un **artista generativo** utiliza **herramientas autónomas** para la elaboración de usas obras, **programando el código** para que herramientas como los plotters pinten con base a la introducción codificada por el usuario. Es conocido como una forma contemporánea del arte, en donde este se encuentra en el código detrás de la obra que en la pintura en si.

El **diseño generativo** es comunmente usado por las marcas actualmente. Se enfoca en la entrega de una aplicación que puede ser usada por los diseñadores para ser aplicados en diferentes tipos de piezas, permitiendo elementos de variabilidad en el diseño según el contexto y la necesidad.

#### ¿Qué es el arte/diseño generativo entonces?

Se refiere a cualquier práctica del ámbito artistico en el que el artista hace uso de un sistema el cual tiene funcionalidad con cierta autonomía.

### Actividad 03

#### Inputs (Entradas)

**micro:bit:**
- Botón A
- Botón B 
- Sensor de acelerómetro (cuando se detecta el gesto de sacudir - shake)
- Puerto de comunicación (USB)
- Botón "Send Love" en la interfaz de p5.js

**Computador**
- Serial (USB)

#### Proceso

- El programa en la micro:bit detecta si se presionan los botones A o B, o si se sacude el dispositivo.
- Según el evento detectado, la micro:bit envía un carácter por el USB al computador:  
  - `'A'` si se presiona el botón A  
  - `'B'` si se presiona el botón B  
  - `'C'` si se sacude
- En el navegador, p5.js recibe este dato a través del puerto serial y cambia el color de un círculo en pantalla dependiendo del valor recibido.


#### Outputs (Salidas):

**micro:bit:**
  - Puerto de comunicación (USB)

**p5.js:**
- Cambio del color del círculo central
- Texto con el carácter recibido en el centro del círculo

**Computador:**
- Display
- Datos enviados por el serial

### Actividad 04

[Enlace al editor](https://editor.p5js.org/Valengp2006/sketches/OSpyB6vzc)

#### Animación con figuras que cambian de forma, color y dirección

Este sketch en **p5.js** implementa un sistema visual interactivo que simula el movimiento de dos figuras que rebotan dentro del lienzo, cambian de color de forma progresiva y modifican su forma cada vez que colisionan con los bordes del lienzo.

#### Código

```javascript
// Figura 1
let x1 = 100, y1 = 100, dx1 = 2, dy1 = 1.5, tipo1 = 0, size1 = 50;
let r1 = 100, g1 = 150, b1 = 255;

// Figura 2
let x2 = 300, y2 = 200, dx2 = -2, dy2 = 2.2, tipo2 = 1, size2 = 40;
let r2 = 255, g2 = 100, b2 = 180;

function setup() {
  createCanvas(400, 400);
  background(220); 
}

function draw() {

  // Dibujar figura 1
  fill(r1, g1, b1);
  dibujarFigura(x1, y1, tipo1, size1);

  // Dibujar figura 2
  fill(r2, g2, b2);
  dibujarFigura(x2, y2, tipo2, size2);

  // Rebote y cambio de forma (figura 1)
  let reboteX1 = (x1 <= 0 || x1 >= width) * 1;
  let reboteY1 = (y1 <= 0 || y1 >= height) * 1;
  dx1 *= 1 - 2 * reboteX1;
  dy1 *= 1 - 2 * reboteY1;
  tipo1 = (reboteX1 + reboteY1) * int(random(3)) + tipo1 * (1 - (reboteX1 + reboteY1));

  // Rebote y cambio de forma (figura 2)
  let reboteX2 = (x2 <= 0 || x2 >= width) * 1;
  let reboteY2 = (y2 <= 0 || y2 >= height) * 1;
  dx2 *= 1 - 2 * reboteX2;
  dy2 *= 1 - 2 * reboteY2;
  tipo2 = (reboteX2 + reboteY2) * int(random(3)) + tipo2 * (1 - (reboteX2 + reboteY2));

  // Movimiento
  x1 += dx1;
  y1 += dy1;
  x2 += dx2;
  y2 += dy2;

  // Cambio de color constante
  r1 = (r1 + 1) % 256;
  g1 = (g1 + 2) % 256;
  b1 = (b1 + 3) % 256;

  r2 = (r2 + 3) % 256;
  g2 = (g2 + 2) % 256;
  b2 = (b2 + 1) % 256;
}

// Función para dibujar una figura según su tipo
function dibujarFigura(x, y, tipo, size) {
  let formas = [
    () => ellipse(x, y, size),
    () => rectMode(CENTER) || rect(x, y, size, size),
    () => triangle(
      x, y - size / 2,
      x - size / 2, y + size / 2,
      x + size / 2, y + size / 2
    )
  ];
  noStroke();
  formas[tipo % 3]();
}
```

#### GIF del resultado final

![Resultado final]([https://github.com/user-attachments/assets/e2aeffd9-b771-4bbe-9a54-b979cbe66ca](https://github.com/user-attachments/assets/b6bb9845-f138-417d-887f-2ad1426caf81)

