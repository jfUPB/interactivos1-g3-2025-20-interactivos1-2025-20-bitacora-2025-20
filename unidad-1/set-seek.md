# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1  
*¿Qué es un sistema físico interactivo?*  
Un sistema físico interactivo es el que tiene inputs que se envían a un programa, sistema o proceso en tiempo real; el respectivo programa y outputs, los cuales segun los inputs generaran una respuesta específica  

*¿Cómo podrías aplicar lo que has visto en tu perfil profesional?*  
Teniendo en cuenta que la carrera se basa en el entretenimiento digital, sería util en multiples sistemas, los cuales consistan en generar experiencias interactivas, esto ya que sería completamente en vivo  


### Actividad 2  

*¿Qué es el diseño/arte generativo?*  
Son piezas gráficas que se generan mediante sistemas físicos interactivos, hay un programa que recibe un tipo específico de inputs en vivo y los traduce o plasma en piezas gráficas que varían según la entrada de información

*¿Cómo podrías aplicar lo que has visto en tu perfil profesional?*  
Especfíficamente yo que tengo interes en el área de experiencias interactivas tengo demasiadas poisibilidades para usar esto, ya que todos los sistemas que funcionen de esta forma pueden ser usados para cualquier tipo de experiencia, sea para usarla por un tercero o por mi persona en la generación de piezas gráficas

### Actividad 3  
*En este sistemas físico interactivo identifica los inputs, outputs y el proceso.*  
| Elemento    | Descripción                                                             |
| ----------- | ----------------------------------------------------------------------- |
| **Inputs**  | Botones A y B, gesto "shake", clic en "Send Love"                       |
| **Proceso** | Enviar/recibir datos por comunicacion serial y cambiar estados          |
| **Outputs** | Círculo de colores (p5.js), íconos en micro\:bit display                |


### Actividad 4  
1) [Link al programa](https://editor.p5js.org/n4ndeZzz/sketches/SF-1L0ZGs)
2) 
     ```javascript  
     function setup() {
     createCanvas(600, 400);
     background(255);
     noStroke();
   }

   function draw() {
     background(255);
     fill(100, 150, 255);

     for (let x = 0; x < width; x += 20) {
    let y = height / 2 + sin(x * 0.05 + frameCount * 0.05) * 50;
    ellipse(x, y, 15, 15);
     }
   }
     ```

3)      

<img width="1853" height="524" alt="image" src="https://github.com/user-attachments/assets/8bc8bbeb-6eff-4b41-ba5b-16fbf7cf7fb8" />


