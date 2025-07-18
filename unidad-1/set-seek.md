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



