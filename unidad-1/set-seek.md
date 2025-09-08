# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01  

**¿Qué es un sistema físico interactivo?**

Es una experiencia interactiva que cuenta con un código y un usuario, y su desarrollo depende directamente de las acciones que este realice. Por ejemplo, permite generar una canción a partir de dibujos, crear figuras utilizando ritmos musicales, o incluso controlar un personaje con el movimiento de la mano.  

**¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**

Como estudiante de Ingeniería en Diseño de Entretenimiento Digital, este tipo de herramientas me resulta especialmente útil para crear experiencias interactivas aplicables en eventos, museos y muchos otros espacios. Las experiencias que involucran activamente al usuario no solo captan mejor su atención, sino que también ofrecen una forma innovadora y atractiva de aprender o entretenerse.  


### Actividad 02  

**¿Qué es el diseño/arte generativo?**

Es una práctica utilizada por ejemplo por artistas y diseñadores que consiste en emplear un sistema autónomo para la creación de una obra. En este enfoque, se utiliza un programa que ejecuta la pieza deseada de forma independiente. El creador diseña el proceso (código), mientras que el sistema autónomo genera el resultado.En este está presenta la aleatoriedad, algoritmos y reglas y este puede ser usado en marcketing.  

**¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**

En la ingeniería en diseño de entretenimiento digital, considero que el diseño generativo permite crear contenido dinámico e interactivo mediante algoritmos y reglas definidas. Se puede aplicar en videojuegos para generar niveles o personajes de forma procedural (un ejemplo es que algunos juegos utilizan una "semilla" para generar el mundo), en animaciones para automatizar movimientos, y en narrativas interactivas que responden al usuario o algún sonido (tal como se mostró en clase con el logo que variaba según la canción). Este enfoque combina creatividad y programación locual es bastante usado en la carrera y será necesario a futuro, ademas permite desarrollar experiencias personalizadas y escalables sin diseñar cada elemento manualmente.


### Actividad 03  

**Inputs:**  
Microbit: A y B (Botones), Aceleron, Puerto USB y el Display
Computador: Puerto USB

**Outputs:**  
MicroBit: Puerto USB (Serial)
Computador: Cable, Boton "Send love" y Pantalla

**Proceso:**    
MicroBit: lee los inputs  
Computador: Datos enviados por el puerto USB y la pantalla  


### Actividad 04  

* Escribe el enlace a tu programa en el editor de p5.js.  
[Código p5.js](https://editor.p5js.org/Ayepes2402/sketches/VXM4SUZSL)  
* Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).
  
``` js
function setup() {
  createCanvas(600, 400);
  background(255);
  noStroke();
}

function draw() {
  let x = random(width);
  let y = random(height);

 
  let r = sin(frameCount * 0.01) * 127 + 128;
  let g = cos(frameCount * 0.01) * 127 + 128;
  let b = random(100, 255);
  fill(r, g, b, 100);

  
  let size = random(10, 30);
  ellipse(x, y, size, size);
}
```

* Muestra una captura de pantalla del resultado de tu programa.  
  <img width="448" height="298" alt="image" src="https://github.com/user-attachments/assets/83a57549-72de-455d-b7fb-e70fd63dea1c" />


