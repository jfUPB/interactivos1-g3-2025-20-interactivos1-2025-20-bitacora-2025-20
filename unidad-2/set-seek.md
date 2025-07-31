# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1.  

Al revisar el código trabajado en clase sobre el parpadeo de píxeles con micro:bit, me concentré en entender el funcionamiento interno del programa a partir de tres aspectos clave: los estados, los eventos/entradas y las acciones. 

#### 1. Estados del programa.  

El objeto "Pixel", creado dentro del programa, funciona como una pequeña máquina de estados. Cada objeto tiene dos estados principales:  

- "Init": Es el estado inicial, donde se configura el píxel por primera vez. Aquí se enciende el píxel y se guarda el tiempo actual.
  
- "WaitTimeout": Es el estado en el que el programa espera que pase un intervalo de tiempo específico. Si ese tiempo ya pasó, el píxel cambia de estado (se apaga si estaba encendido, o se enciende si estaba apagado).

Cada pixel tiene sus propios estados de forma independiente.  

#### 2.





