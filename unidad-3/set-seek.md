# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 5:  


## Tabla de vectores de prueba

| Estado inicial | Evento disparador      | Acciones                                                   | Estado final |
|----------------|------------------------|------------------------------------------------------------|--------------|
| CONFIG         | A                      | `count = min(count+1,60)` → mostrar en display             | CONFIG       |
| CONFIG         | B                      | `count = max(10,count-1)` → mostrar en display             | CONFIG       |
| CONFIG         | S                      | `startTime = ticks_ms()` → pasar a cuenta regresiva        | ARMED        |
| CONFIG         | Ninguno                | Mostrar count en display                                   | CONFIG       |
| ARMED          | 1 seg transcurrido     | `count--`, mostrar en display, si `count==0` → SKULL       | ARMED/EXPLODED |
| ARMED          | A                      | Agregar 'A' al buffer de clave                            | ARMED        |
| ARMED          | B                      | Agregar 'B' al buffer de clave                            | ARMED        |
| ARMED          | Clave correcta (A,B,A) | Reset `count=20`, mostrar en display, volver a CONFIG      | CONFIG       |
| ARMED          | Clave incorrecta       | Reset `keyindex=0`                                         | ARMED        |
| EXPLODED       | T                      | Reset `count=20`, mostrar en display, `state=CONFIG`       | CONFIG       |
| EXPLODED       | A/B/S                  | Ignorado                                                   | EXPLODED     |

---
