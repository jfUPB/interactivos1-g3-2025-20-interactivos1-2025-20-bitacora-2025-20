# Unidad 3

## 🔎 Fase: Set + Seek


<img width="1024" height="768" alt="Simple Flowchart Infographic Graph" src="https://github.com/user-attachments/assets/53c4845f-6876-4813-88c5-32bb1040a894" />


**Vectores de prueba:**

- A, B, S, T: evento recibido (vía botón o puerto serie).
- tick: ha pasado ≥1 s (utime.ticks_diff(...) > 1000).
- —: no hay evento.

| Estado inicial | Evento disparador | Acciones esperadas | Estado final |
|:--------------:|:-----------------:|:------------------:|:------------:|
| CONFIG  | — | Ninguna acción visible | CONFIG |
| CONFIG  | A | `count=min(21,60)`; `display.show(21)` | CONFIG |
| CONFIG  | A | `count=min(60,60)=60`; `display.show(60)` | CONFIG |
| CONFIG  | A | `count` permanece 60; `display.show(60)` | CONFIG |
| CONFIG  | B | `count=max(10,19)=19`; `display.show(19)` | CONFIG |
| CONFIG  | B | `count` permanece 10; `display.show(10)` | CONFIG |
| CONFIG  | S | `startTime=now`; `display.show(k)` | ARMED |
| ARMED  | — (<1 s) | Ninguna | ARMED |
| ARMED | tick (≥1 s) | `count=19`; `display.show(19)`; `startTime=now` | ARMED |
| ARMED  | tick (≥1 s) | `count=0`; `display.show(Image.SKULL)`; pasar a EXPLODED | EXPLODED |
| ARMED  | A | `key[0]='A'`; `keyindex=1` | ARMED |
| ARMED  | B | `key[1]='B'`; `keyindex=2` | ARMED |
| ARMED  | A | `key[2]='A'`; `keyindex=3` → comparar con `PASSWORD` → correcto | CONFIG (reinicio) |
| ARMED  | B (clave incorrecta) | `key[2]='B'`; `keyindex=3` → comparación falla → `keyindex=0` | ARMED |
| ARMED  | Entrada no A/B (ej. S o T) | Sin acción sobre clave ni `count` | ARMED |
| EXPLODED | T | `count=20`; `display.show(20)`; `startTime=now` | CONFIG |
| EXPLODED | — | Ninguna (permanece calavera) | EXPLODED |
| CONFIG | Entrada serial ‘A’ | Igual que botón A: `count++` con tope 60 | CONFIG |
| CONFIG | Entrada serial ‘B’ | Igual que botón B: `count--` con piso 10 | CONFIG |
| CONFIG | Entrada serial ‘S’ | Igual que botón S: armar bomba | ARMED |



