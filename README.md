# Simulador Dräger Evita 4 — MEDISHORT360

Simulador **educativo** e instalable (PWA) del ventilador mecánico Dräger Evita 4.
Los estudiantes programan el ventilador sobre un paciente virtual y **ven lo que
pasaría de verdad**: si cometen un error, el paciente se deteriora y una
notificación les explica qué hicieron mal, por qué ocurre y cómo se corrige.

> ⚠️ Herramienta educativa. No es un dispositivo médico, no sustituye el manual
> del fabricante ni el criterio clínico profesional.

---

## Archivos

| Archivo | Contenido |
|---|---|
| `index.html` | Estructura de las 7 pantallas |
| `style.css` | Estilos, temas claro/oscuro y color de fondo configurable |
| `app.js` | Motor fisiológico, reglas de retroalimentación, curvas e interfaz |
| `manifest.json` | Manifiesto PWA (instalable en Android / iOS / escritorio) |
| `sw.js` | Service worker: la app funciona **100 % sin conexión** |
| `icons/` | Iconos MEDISHORT360 (192, 512 y maskable) |

No hay dependencias externas: ni librerías, ni fuentes remotas, ni peticiones de red.

---

## Cómo se usa

1. **Nuevo paciente** — se introducen talla, sexo, peso, temperatura, diagnóstico,
   gravedad, estado hemodinámico y sedación. La app calcula el **peso predicho (PBW)**
   y el volumen protector objetivo.
2. **Laboratorio** — gasometría arterial (pH, PaCO₂, PaO₂, HCO₃⁻, EB, SaO₂, lactato),
   electrolitos, albúmina, creatinina y glucosa. El botón *Generar según patología*
   carga una gasometría coherente con el diagnóstico, y la app **interpreta el
   trastorno ácido-base en vivo** (compensación esperada, fórmula de Winter, anión gap
   corregido por albúmina, agudo frente a crónico).
3. **Ventilador** — pantalla estilo Evita 4 con curvas de presión, flujo y volumen en
   tiempo real, bucles P-V y F-V, tendencias, valores medidos, monitor del paciente,
   modos, parámetros, límites de alarma y maniobras.
4. **Finalizar y evaluar** — informe con puntuación, errores graves, aspectos a
   mejorar, aciertos y bitácora completa de la sesión.

También hay **8 casos clínicos** listos, una **guía rápida** del Evita 4 y una
pantalla de **progreso** que acumula los errores más frecuentes del estudiante.

---

## Modos y parámetros

Modos: **IPPV/CMV, SIMV, BIPAP, CPAP/ASB, APRV y MMV**, con **AutoFlow®** y **ASB**
conmutables. Cada modo muestra únicamente sus propios parámetros, como el equipo real.

Rangos tomados del Evita 4: VT 0.1–2.0 L · FR 2–80/min · Tinsp 0.1–10 s ·
Flujo 6–120 L/min · Pinsp 0–95 mbar · PEEP 0–35 mbar · FiO₂ 21–100 % ·
ASB 0–95 mbar · Trigger de flujo 0.3–15 L/min · Rampa 0–2 s.
Alarmas: Paw↑ 10–99 mbar · VM↑/VM↓ · VTi↑ · f esp↑.

---

## Qué simula el motor fisiológico

**Mecánica:** distensibilidad y resistencia propias de cada patología, constante de
tiempo τ = R × C, presión pico y meseta, presión de distensión (ΔP), presión media,
relación I:E y **auto-PEEP en equilibrio** (`Vatrapado = VT · r/(1−r)`), limitación
por Pmax como ventilación limitada por presión.

**Intercambio gaseoso:** ventilación alveolar y PaCO₂ (`PaCO₂ = 0.863 · VCO₂ / VA`),
espacio muerto que aumenta con la hiperinsuflación y la sobredistensión, ecuación del
gas alveolar, **shunt** modulado por el reclutamiento que aporta la PEEP hasta el punto
útil de cada pulmón, curva de disociación de la hemoglobina (Severinghaus) con
desplazamiento por pH y temperatura.

**Ácido-base:** Henderson-Hasselbalch, amortiguación tisular aguda calculada **sobre el
cambio provocado por el estudiante** (no sobre 40 mmHg fijo, para no falsear a los
pacientes que llegan ya compensados), compensación renal lenta, exceso de bases y
anión gap corregido.

**Hemodinámica:** caída del retorno venoso proporcional a la presión media en la vía
aérea y a la volemia, gasto cardiaco, presión arterial media, frecuencia cardiaca
(incluida la bradicardia hipóxica preterminal) y lactato por hipoperfusión.

**Complicaciones:** neumotórax por presión meseta sostenida, volutrauma progresivo,
toxicidad por oxígeno, fatiga muscular, autodisparo y esfuerzos inefectivos.

---

## Retroalimentación de errores

Más de 30 reglas vigilan la sesión de forma continua. Cada una entrega:
**qué ha pasado** (con los números del paciente), **por qué ocurre** (la fisiología
detrás, desplegable) y **cómo se corrige** (la maniobra concreta).

Cubren protección pulmonar (VT por kg de PBW, meseta > 30, ΔP > 15), PEEP y
oxigenación, atrapamiento aéreo, trastornos ácido-base —incluido el error clásico de
**quitarle la compensación respiratoria a una cetoacidosis**—, neuroprotección en el
TCE, hemodinámica, sincronía paciente-ventilador, seguridad de las alarmas y
criterios de destete. También hay reglas de **refuerzo positivo** cuando el manejo es
correcto.

---

## Apariencia

Tema **oscuro**, **diurno** o **automático**; **color de fondo** elegible entre
10 presets o cualquier color personalizado; 7 colores de acento; sonido y vibración
de alarmas conmutables; velocidad de simulación ×1 / ×5 / ×15.
La paleta por defecto (negro profundo, oro y rojo cardiaco) está tomada del logo de
MEDISHORT360.

---

## Instalación

Se sirve como sitio estático (GitHub Pages, Netlify, o cualquier servidor).
En el móvil: *Añadir a pantalla de inicio* en Chrome o Safari, o el botón
**Instalar aplicación** de la pantalla de inicio.

Para probarlo en local:

```bash
python3 -m http.server 8080
# abrir http://localhost:8080
```
