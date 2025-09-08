# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5

### Es momento de modelar la bomba y definir vectores de prueba

#### Ahora es momento de modelar la bomba con una máquina de estados y definir una tabla con vectores de prueba.

*1. Construye el modelo de la bomba 3.0. Como ya tienes el código puedes tener un modelo muy preciso (o en cristiano, hacer el diagrama del codigo que ya esta hecho)*

<img width="1717" height="953" alt="image" src="https://github.com/user-attachments/assets/5c49f3f7-8d85-4fe3-b326-8b57f7eed598" />

*2. Crear una tabla con los vectores de prueba. La tabla debe tener 4 columnas por vector y puedes agrupar vectores en un gran vector. Las columnas son:*

*- Estado inicial*

*- Evento disparador*

*- Acciones*

*- Estado final*

| Estado Inicial | Evento disparador | Acciones | Estado Final |
| --- | --- | --- | --- |
| CONFIG ( self.count = 20) | Aumentar un segundo al contador | Si presiono 'A' (event.read() == 'A'), se limpia el evento y self.count = min(self.count+1,60), aumentandolo a 21 | El programa pasa el vector |
| CONFIG ( self.count = 20) | Disminuir un segundo al contador | Si presiono 'B' (event.read() == 'B'), se limpia el evento y self.count = max(10,self.count-1), haciendo que baje a 19 | El programa tambien pasa el vector |
| CONFIG | Activar la bomba y pasar al estado ARMED | Si agito el micro:bit, "shake" (event.read() == 'S':), la bomba dara cuenta atras y pasara al estado ARMED (self.state = 'ARMED') | El programa pasa por el vector sin problemas |
| ARMED | Reducir un segundo cada que pasa el intervalo de tiempo | Si el intervalo pasa los 1000 milisegundos, la bomba ira reduciendo -1 segundo (utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000)  | El programa funciona con lo que solicita el vector |
| ARMED | Oprimir 'A' acumulara al contador de la contraseña | Si presiono el botón 'A' (event.read() == 'A') durante la cuenta atras, mostrara la cara "SILLY" (display.show(Image.SILLY)) | El vector cumple con la prueba |
| ARMED | Oprimir 'B' acumulara al contador de la contraseña | Si presiono el botón 'B' (event.read() == 'B') durante la cuenta atras, mostrara la cara "SURPRISED" (display.show(Image.SURPRISED) | El vector nuevamente cumple|
| ARMED (A, B, A acumulado) | Desactivar la bomba y devolverse al estado CONFIG| Si tras oprimir la combinación, A, B, A, (passIsOK == True), regresara a CONFIG con el contador de 20 segundos predeterminado (self.state = 'CONFIG') | El programa cumple con el vector |
| ARMED | Detonar la bomba y pasar al estado EXPLODED | Si el contador es igual a 0 (self.count == 0), mostrara en el micro:bit una calavera en señal de game over (display.show(Image.SKULL)) y el programa pasara al estado EXPLODED (self.state = 'EXPLODED') | El vector nuevamente vuelve a cumplir |
| EXPLODED | Volver a la configuración (al estado CONFIG) tras explotar la bomba | Si se oprime el touch del micro:bit (event.read() == 'T'), además de limpiar el evento, este el estado actual ahora sera CONFIG (self.state = 'CONFIG') | El programa nuevamente cumple con el vector |

##### *Nota: En algunos vectores (dos en total) hice ligeras modificaciones al codigo para verificar que si funcionaran, cabe aclarar que estos no se veran reflejados en el diagrama ni aqui en el codigo de GitHub, son solo como digo, para verificación.*














