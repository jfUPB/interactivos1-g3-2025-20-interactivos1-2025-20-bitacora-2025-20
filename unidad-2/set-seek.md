# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

#### ¿Cuáles son los estados en el programa?
  1. Init
  2. "WaitTimeout"

#### ¿Cuáles son los eventos/inputs en el programa?
```python
if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
```
Se espera a que pase el tiempo o sea la hora de prender/apagar.

#### ¿Cuáles son las acciones en el programa?
1. ```python
   self.startTime = utime.ticks_ms()
   ```
2. ```python
   display.set_pixel(self.pixelX,self.pixelY,self.pixelState)
   ```
3. Condición de guarda
   ```python
   if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
   ```

##### Notas:
- Al crear un objeto primero lo creo en el espacio de memoria y luego inicializo el objeto, y guardamos la direccion del objeto en una variable.  
- __init__  para crear un constructor.  
- this. en c# es self. en python.  
- Para funciones en python utilizamos def.  
- El evento es cuando debo esperar algo para que pase algo.
