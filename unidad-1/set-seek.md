# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

¿Qué es un sistema físico interactivo?

Se trata de un sistema que desde inputs y outputs por medio de la interacción del usuario, este responderá en tiempo real a las acciones que realice el mismo basado en la programación y configuración que este tenga de ambas partes.

Algunos ejemplos son Mario Kart Home Circuit, siendo un sistema que permite usar juguetes de personajes como Mario o Luigi (se pueden usar más de uno al mismo tiempo), cabe destacar que dichos juguetes vienen con una cámara integrada (input) la cual envía la información que reciba al ver las líneas de meta a la nintendo switch (input por la detección de sensores y botones del joycon y output por la cámara y como muestra a los oponentes).

Otro ejemplo similar es el de ProCreate in Ipad, aquí el input, la tablet desde la app de ProCreate, mientras que el usuario dibuja líneas o círculos (las acciones del usuario también cuentan como input), el output, es decir, la pantalla del computador, mostrará lo que haya hecho el usuario de una forma diferente, pasando de ver solo líneas curvas y círculos, tallos de una flor junto a los mismos pétalos, moviéndose de forma animada.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Dentro de mi perfil profesional, los sistemas físicos interactivos pueden ser de utilidad para aplicar mejoras o de plano tener una mejor capacidad de hacer prototipos de un proyecto, aunque a primera vista da la impresión de que solo serviria en la línea de Experiencias interactivas, en casos como los de Mario Kart y ProCreate estos sistemas también son de mucha utilidad en Videojuegos o Animación digital, incluso son bastante útiles yo en un futuro darle al usuario una experiencia más dinámica y especial.

### Actividad 2

¿Qué es el diseño/arte generativo?

El diseño, o también llamado arte generativo, es la práctica artística en la que un artista y/o diseñador por medio de un sistema, algoritmo, reglas, variabilidad, aleatoriedad y un sistema autónomo, puede ayudar a la creación o complementación de obras de arte de diferente tipo. Entre algunos ejemplos, se encuentra el Plotter, una máquina que gracias a la ayuda de indicaciones que le da un código (programa), además de la introducción de IA o algoritmo, puede generar pinturas que se diferencien entre sí.

Otro caso es el del logotipo de Philharmonie Luxembourg, el cual no hace uso de piezas estáticas como uno pensaría, sino que por medio de un sistema, logra que haya volumen en el logotipo al ritmo de la musica o cancion que esté tocando el artista, teniendo variaciones, como si de los LEDS de un DJ se tratara, siendo algo que generalmente o en muchos casos, un diseñador gráfico no llegaría a ese nivel.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

En un perfil profesional como el de un Ingeniero en Diseño de Entretenimiento Digital (IDED), el cual es mi caso por ejemplo, gracias a la ayuda de algoritmos y sistemas como fueron mostrados en los ejemplos de clase como el Plotter, permite crear y experimentar con las obras que yo pueda hacer, ya sea para volverlas más dinámicas o acelerar un proceso que puede llegar a ser tardío, pasando de estar demasiadas horas probando combinaciones de por ejemplo replicar un cuadro pero con diferencias notables, a tenerlo listo en cuestión de minutos o menos horas.

### Actividad 3

En este sistemas físico interactivo identifica los inputs, outputs y el proceso

INPUTS:

- Microchip: Los botones y el acelerómetro del microchip, así como el mismo puerto de comunicación USB, además del botón de "Send love".
- Computador: El serial/USB.

OUTPUTS:

- Microchip: El puerto de comunicación USB, el display (en este caso, reacciona al input con el botón de send love para mandar información).
- Computador: Los datos que el usuario envíe por el serial y lo que muestra el computador.

PROCESO:

El microchip lee los inputs y la página responde a esos inputs. También es la parte en la que el input genera los outputs.

### Actividad 4

Escribe el enlace a tu programa en el editor de p5.js.

[Mi código de p5.js](https://editor.p5js.org/pinwinasio480/sketches/pIeeSax4q)

Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(120, 10);
  square(random(100, 250), random(150,350), random(25,50));
  ellipse(random(50, 50), random(200,250), random(50,75));
  point(random(350, 350), random(200,250), random(50,75));
  let x = 100 * cos(frameCount * 0.05) + 180;
  let y = 50;
  line(75, y, x, y);
  circle(x, y, 20);
}

Muestra una captura de pantalla del resultado de tu programa

<img width="497" height="497" alt="image" src="https://github.com/user-attachments/assets/8abb448f-636b-45a7-94eb-cbe8015655f4" />



