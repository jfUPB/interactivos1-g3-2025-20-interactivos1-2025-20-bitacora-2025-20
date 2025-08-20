# Unidad 3

## 🔎 Fase: Set + Seek


<img width="1024" height="768" alt="Simple Flowchart Infographic Graph" src="https://github.com/user-attachments/assets/53c4845f-6876-4813-88c5-32bb1040a894" />

**Vectores de prueba:**
| Estado inicial | Evento disparador | Acciones esperadas | Estado final |
|:--------------:|:-----------------:|:------------------:|:------------:|
| CONFIG (`count=20`) | — | Ninguna acción visible | CONFIG |
| CONFIG (`count=20`) | A | `count=min(21,60)`; `display.show(21)` | CONFIG |
| CONFIG (`count=59`) | A | `count=min(60,60)=60`; `display.show(60)` | CONFIG |
| CONFIG (`count=60`) | A | `count` permanece 60; `display.show(60)` | CONFIG |
| CONFIG (`count=20`) | B | `count=max(10,19)=19`; `display.show(19)` | CONFIG |
| CONFIG (`count=10`) | B | `count` permanece 10; `display.show(10)` | CONFIG |
| CONFIG (`count=k`) | S | `startTime=now`; `display.show(k)` | ARMED |
| ARMED (`count=20`, `keyindex=0`) | — (<1 s) | Ninguna | ARMED |
| ARMED (`count=20`) | tick (≥1 s) | `count=19`; `display.show(19)`; `startTime=now` | ARMED |
| ARMED (`count=1`) | tick (≥1 s) | `count=0`; `display.show(Image.SKULL)`; pasar a EXPLODED | EXPLODED |
| ARMED (`keyindex=0`) | A | `key[0]='A'`; `keyindex=1` | ARMED |
| ARMED (`keyindex=1`) | B | `key[1]='B'`; `keyindex=2` | ARMED |
| ARMED (`keyindex=2`) | A | `key[2]='A'`; `keyindex=3` → comparar con `PASSWORD` → correcto | CONFIG (reinicio) |
| ARMED (`keyindex=2`) | B (clave incorrecta) | `key[2]='B'`; `keyindex=3` → comparación falla → `keyindex=0` | ARMED |
| ARMED (cualquier `count`) | Entrada no A/B (ej. S o T) | Sin acción sobre clave ni `count` | ARMED |
| EXPLODED | T | `count=20`; `display.show(20)`; `startTime=now` | CONFIG |
| EXPLODED | — | Ninguna (permanece calavera) | EXPLODED |
| CONFIG | Entrada serial ‘A’ | Igual que botón A: `count++` con tope 60 | CONFIG |
| CONFIG | Entrada serial ‘B’ | Igual que botón B: `count--` con piso 10 | CONFIG |
| CONFIG | Entrada serial ‘S’ | Igual que botón S: armar bomba | ARMED |
