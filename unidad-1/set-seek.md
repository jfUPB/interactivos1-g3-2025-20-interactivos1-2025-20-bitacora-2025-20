# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

**¿Qué es un sistema físico interactivo?**

Un sistema fisico interactivo es el que tiene tanto una parte fisica como una digital que por medio de computación, electrónica y mecanismos físicos en tiempo real consigue interactuar con las personas por medio de sensores o procesos computacionales.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

A nivel profesional los sistemas fisicos interactivos se vuelven fundamentales, esto porque mas allá de programar, crear video juegos o hacer animaciones, lo que buscamos es crear un producto o una experiencia para las personas, cosa que se nos facilita mucho más atraves de estos sistemas.

### Actividad 2

¿Qué es el diseño/arte generativo?

Algoritmo, reglas, sistema autonomo, variabilidad. En cuanto al diseño, se aplica mucho para marketing o branding, se parece mas a un estudiante de IDED que a un artista grafico porque no entrega algo rigido, si no algo con variabilidad e interactivo. Es una practica artistica o de diseño en el que el artista utiliza un sistemas con cierto grado de autonomía, las reglas pueden ser software o programas que le ayudaran al sistema a ejecutar la pieza que deseo hacer.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Se podría aplicar de una infinidad de formas, sabiendo ya que puede ser un concepto utilizado tanto para generar obras de arte o para el diseño de un logo ligado a lo interactivo, se abren las posibilidades para aplicarlo de manera profesional, sea para una empresa o para un evento, el diseño o arte generativo nos permite llevar más allá lo que podría ser un simple apartado visual para las personas.

### Actividad 3

Inputs

Depende de como se mire, del Microbit los inputs son los botones, el puerto y el acelerometro.  
Mientras que para el computador es el puerto USB.

Outputs

Del Microbit son el puerto y el display.  
Del computador es el Cable, el boton de "Send love" y la pantalla.

Proceso

El Microbit lee los inputs.  
Del computador son los datos enviados por el puerto USB y la pantalla

### Actividad 4

Escribe el enlace a tu programa en el editor de p5.js.

[Código p5.js](https://editor.p5js.org/alejogonzdav41/sketches/lhBSbsggE)

Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).

``` js
let t = 0;

function setup() {
  createCanvas(600, 400);
  background(240);
  frameRate(10);
  noStroke();
}

function draw() {
  dibujarFiguraAleatoria();
  t += 0.1;
}

function dibujarFiguraAleatoria() {
  let tipo = int(random(3));

  let col = color(
    127 + 127 * sin(t + random(PI)),
    127 + 127 * cos(t + random(PI)),
    127 + 127 * sin(t + random(TWO_PI)),
    random(100, 255)
  );
  fill(col);

  let x = width / 2 + cos(t + random(-PI, PI)) * random(50, width / 2);
  let y = height / 2 + sin(t + random(-PI, PI)) * random(50, height / 2);

  let size = 20 + 30 * abs(sin(t + random(PI)));
  
  switch (tipo) {
    case 0:
      ellipse(x, y, size, size);
      break;
    case 1:
      rect(x, y, size, size);
      break;
    case 2:
      triangle(
        x, y,
        x + size, y,
        x + size / 2, y - size
      );
      break;
  }
}
```

Muestra una captura de pantalla del resultado de tu programa.

<img width="746" height="493" alt="image" src="https://github.com/user-attachments/assets/e5270c4c-00ed-4a67-8385-e71e3454403e" />


