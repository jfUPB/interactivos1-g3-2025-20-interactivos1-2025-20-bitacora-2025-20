# Unidad 3

## 🔎 Fase: Set + Seek

# Unidad 3

## 🔎 Fase: Set + Seek

### Modelado de la bomba 3.0

Modelo — Bomba 3.0 - Máquina de Estados Finitos (FSM)

##Estados
CONFIG: Ajuste del tiempo (10–60 s).
ARMED: Cuenta regresiva activa.
BOOM: Explosión (estado terminal).
RESET: transición técnica para reinicio desde BOOM.

## Eventos genéricos (normalizados)

A, B, ARM, TICK, NO_EVENT, RESET

Botones, serial y p5.js producen los mismos eventos.
Regla: después de manejar un evento ⇒ event = NO_EVENT.

## Variables

time_set ∈ [10,60] (por defecto 20).
remaining (segundos restantes en ARMED).
seq ∈ {"", "A", "AB"} (progreso de la secuencia de desarme A→B→A).

## Transiciones y acciones
CONFIG
A: time_set = min(60, time_set+1); show_time(time_set).
B: time_set = max(10, time_set-1); show_time(time_set).
ARM: remaining = time_set; reset_seq(); start_timer(); show_countdown(remaining) ⇒ ARMED.

## ARMED

TICK: remaining -= 1; show_countdown(remaining);
si remaining == 0 ⇒ explode(); stop_timer() ⇒ BOOM.
si remaining <= 5 ⇒ beep('panic'); blink_fast().

Secuencia ABA (desarme)

  A con seq=="" ⇒ seq="A".
  B con seq=="A" ⇒ seq="AB".
  A con seq=="AB" ⇒ éxito: stop_timer(); reset_seq(); remaining=time_set; show_time(time_set) ⇒ CONFIG.
  Cualquier desvío (p. ej., A con seq=="A" o B con seq=="AB") ⇒ reset_seq(); flash('err') (permanece en ARMED).

Otros eventos válidos (A/B sueltos): feedback visual/sonoro opcional, sin cambiar estado si no forman parte válida de ABA.
NO_EVENT: no cambios.

## BOOM

RESET (si lo implementas) ⇒ time_set=20; remaining=time_set; reset_seq(); show_time(time_set) ⇒ CONFIG.
De lo contrario, reinicio por energía/sistema.

## Invariantes / Reglas

Siempre consumir el evento tras procesarlo (event = NO_EVENT).
Los TICK no borran la secuencia seq (permanece hasta completar o fallar).
Límite estricto: 10 ≤ time_set ≤ 60.

### Tabla con los vectores de prueba

| Estado inicial | Evento disparador | Acciones | Estado final |
|----------------|------------------|----------|--------------|
| CONFIG | A | time_set = min(60, time_set+1); show_time(time_set) | CONFIG |
| CONFIG | B | time_set = max(10, time_set-1); show_time(time_set) | CONFIG |
| CONFIG | ARM | remaining=time_set; reset_seq(); start_timer(); show_countdown(remaining) | ARMED |
| ARMED | TICK | remaining -= 1; show_countdown(remaining); if remaining==0 → explode() | ARMED/BOOM |
| ARMED | A (seq=="") | seq="A" | ARMED |
| ARMED | B (seq=="A") | seq="AB" | ARMED |
| ARMED | A (seq=="AB") | stop_timer(); reset_seq(); remaining=time_set; show_time(time_set) | CONFIG |
| ARMED | Secuencia errónea | reset_seq(); flash("err") | ARMED |
| ARMED | TICK (remaining<=5) | beep("panic"); blink_fast(); show_countdown(remaining) | ARMED |
| BOOM | RESET | time_set=20; remaining=time_set; reset_seq(); show_time(time_set) | CONFIG |



