# VÉSPER 128 — Análisis: ¿SO simulado en blanco y negro o en color?

## 1. Planteo

La pregunta no es estética sino **perceptual y narrativa**: dado que el objetivo
es provocar *eerie / liminal / rechazo inconsciente* mediante la UI, ¿qué espacio
de color maximiza esos efectos con el mínimo de señales explícitas?

Se comparan tres candidatos:

- **A. Color pleno** (paleta tipo System 7 / Windows 3.1: grises + acentos).
- **B. Monocromo estricto 1-bit** (Macintosh 128K: negro #000 / blanco #FFF).
- **C. Monocromo modulado** ("monocromo+": 1 bit de *salida* con trama ordenada,
  pero **paleta interna de grises y deriva cromática lenta del fósforo**).

Veredicto anticipado: **C**. Lo que sigue es la justificación.

---

## 2. Argumentos a favor del monocromo (y por qué el color pleno pierde)

### 2.1 El color es un canal consciente; la luminancia es un canal automático
La información cromática se procesa por vías (konio/parvo) con baja resolución
espacial y temporal y alta dependencia de contexto; la luminancia domina la
forma, el movimiento y la atención [1][2]. Un cambio de color es **nombrable**
("se puso rojo"); un cambio de luminancia/contraste es **sentido antes que
nombrado**. Como el objetivo es arousal *sin etiqueta* (Schachter & Singer; ver
investigación §3.1), el vector debe viajar por luminancia. El color pleno
convierte cada decisión emocional en una decisión verbalizable y por tanto
discutible y atribuible — exactamente lo que queremos evitar.

### 2.2 El color pleno activa expectativas de interfaz moderna
La estética-usabilidad [3][4] predice que una paleta colorida y armónica eleva
la confianza percibida. Para *degradar* confianza necesitamos un punto de
partida austero cuya degradación sea medible en pocos grados de libertad. En
color, los grados de libertad (tono, saturación, luminancia) son tantos que la
degradación se percibe como "estilo", no como "falla". En monocromo, los únicos
grados son luminancia y textura: una deriva de 4 % en contraste **es** la falla.

### 2.3 Diegesis y liminalidad histórica
El monocromo 1-bit es el aspecto material de las máquinas que el relato evoca
(Macintosh 128K, 512×342, 1984; Apple II). La liminalidad del objeto
"encontrado" se apoya en su obsolescencia: una interfaz a color de los 90 es un
hogar *habitado*; una interfaz 1-bit es un hogar *abandonado* [5]. El color
pleno rompería la ficción del "LOST HD": nadie abandona una máquina colorida
sin borrarla; las máquinas en blanco y negro se heredan con sus fantasmas.

### 2.4 El monocromo como campo casi-Ganzfeld
Un campo 1-bit con trama uniforme es homogéneo y de baja estructura: condiciones
que favorecen percepts espontáneos y amplificación de ruido interno (efecto
Ganzfeld; Metzger 1930 [6]). El color pleno añade estructura cromática que
*ancla* la percepción y reduce la sugestionabilidad del campo.

### 2.5 Pareidolia y economía de rasgos
La detección de configuraciones faciales en ruido es máxima cuando la imagen
tiene **pocos rasgos y alto contraste** [7]. El 1-bit es el caso límite: cada
píxel es rasgo o no-rasgo. Los agrupamientos de trama que sugieren una cara
funcionan en 1-bit y se diluyen en color (el color compite como rasgo).

---

## 3. Argumentos a favor del color (y por qué no alcanzan)

### 3.1 El rojo como señal de amenaza
Elliot y cols. muestran que el rojo induce evitación sin conciencia [8][9].
Tentador. Pero: (a) el efecto está ligado a **contexto de logro** (color-in-
context [8]); fuera de ese contexto se debilita; (b) en una interfaz, el rojo
es ya una convención de error/parada: el usuario lo **lee** como "error", no lo
**siente** como amenaza; (c) un único rojo en un mar monocromo es un faro
consciente: atribución inmediata, muerte del efecto difuso. El rojo sirve para
*señalar*, y nosotros no queremos señalar.

### 3.2 Saturación y placer
Valdez y Mehrabian: placer/arousal dependen de saturación y luminosidad [10].
Cierto, pero eso favorece al monocromo *bien iluminado*: podemos modular
luminancia (placer) sin tocar tono. La saturación cero no es ausencia de
afecto: es ausencia de *vocabulario* afectivo, que es lo que buscamos.

### 3.3 Riesgo del monocromo: fatiga y monotonía
Un campo 1-bit prolongado puede volverse simplemente aburrido (Berlyne: baja
variable colativa → lado izquierdo de la U [11]). **Contra-argumento y solución:**
la monotonía es el *estado base deseado* (liminalidad), y la curva de tensión
introduce novedad/conflicto por encima del óptimo de forma gradual. Además, el
monocromo modulado (C) dispone de textura (tramas), deriva de fósforo y ruido,
es decir, complejidad *sin* color.

---

## 4. Por qué no el monocromo estricto (B) sino el modulado (C)

El 1-bit puro (#000/#FFF) tiene tres problemas técnicos y uno psicológico:

1. **Contraste infinito aversivo no dosificable.** Con sólo dos valores no hay
   manera de degradar *lentamente*: cualquier cambio es binario y por tanto
   notorio. Necesitamos un eje continuo de degradación.
2. **Halación y fatiga.** Blanco #FFF sobre negro #000 en CRT simulado produce
   deslumbramiento y halación que cansa por razones ópticas, no narrativas:
   ruido no atribuible *indeseado* (queremos el atribuido a la máquina).
3. **Sin espacio para lo subliminal.** Un destello de 2 frames en 1-bit puro es
   o visible o invisible; no hay "casi visible".
4. **Psicológico:** el 1-bit puro es *estilización reconocible* (el usuario lo
   lee como "retro bonito"). La estilización reconocible activa nostalgia =
   afecto positivo que compite con el eerie. Necesitamos que el monocromo
   **se comporte como materia** (fósforo que envejece, negros que se lavan), no
   como estilo.

**C** resuelve todo: la *salida* es 1 bit (tramada), pero el *buffer interno*
es de 8 bits de luminancia con una paleta de fósforo que deriva. Así tenemos:

- eje continuo de degradación (contraste, lift, temperatura);
- negros que no son #000 (evita halación y permite "negros lavados");
- espacio para lo subliminal (contrastes 0.3–0.5 que el damero traduce a
  texturas casi-invisibles);
- y, crucialmente, **un único evento cromático**: el ámbar del arranque. Su
  retirada al pasar al escritorio neutro es el primer gesto emocional del juego
  y funciona precisamente porque el resto del juego es monocromo.

---

## 5. Especificación de la paleta (C)

### 5.1 Escala de grises efectiva
Salida 1-bit con damero/Bayer de 4×4 → 17 niveles efectivos. Niveles de uso:

| Nivel | Uso | Trama |
|---|---|---|
| 0 (tinta) | texto, bordes, cursor | sólido |
| 0.25 | sombras suaves, ítems inhabilitados | Bayer 25 % |
| 0.5 | escritorio, cuerpo de carpetas, barra de título inactiva | damero |
| 0.75 | obturador de disquete, rellenos claros | Bayer 75 % |
| 1 (papel) | fondos de ventana, menús | sólido |

### 5.2 Extremos cromáticos por acto
| Acto | Papel (RGB) | Tinta (RGB) | Contraste | Lift | Lectura emocional |
|---|---|---|---|---|---|
| 0 · POST | 214,200,168 | 10,10,11 | 1.06 | 0 | ámbar = hogar, máquina viva |
| I · escritorio | 203,202,199 | 10,10,11 | 1.04 | 0.01 | neutro = confianza |
| II · descubrimiento | 190,196,203 | 16,16,18 | 0.98 | 0.035 | frío incipiente |
| III · agencia | 176,190,206 | 26,27,30 | 0.90 | 0.075 | mesópico, lavado, nocturno |

La interpolación es **continua en función de la tensión**, no por actos
discretos: el usuario nunca ve un "cambio de tema", ve una máquina que
envejece en tiempo real.

### 5.3 Regla del acento único
Ningún otro tono aparece en toda la sesión. Si en una futura versión se
introduce color, debe ser **uno solo, una sola vez, y debe ser un error**
(p. ej. un píxel cian en un mar monocromo durante 1 frame): el color como
síntoma, nunca como estilo.

---

## 6. Síntesis relacional (análisis propio)

Cruzando los mecanismos de la investigación con las restricciones del medio:

1. *Arousal sin etiqueta* exige canales no-inspectables → **luminancia y
   timing**, no tono. ⇒ monocromo.
2. *Degradación continua* exige un eje analógico → **buffer de 8 bits bajo
   salida 1-bit**. ⇒ modulado, no estricto.
3. *Liminalidad* exige homogeneidad de campo → **trama uniforme dominante**.
   ⇒ damero 50 % como suelo.
4. *Pareidolia* exige economía de rasgos → **1-bit de salida**. ⇒ modulado con
   cuantización dura.
5. *Hogar que se pierde* exige un punto de partida afectivo → **un solo color
   (ámbar) que se retira**. ⇒ evento cromático único.
6. *Seguridad y fatiga* prohíben negro/blanco absolutos y flicker de banda
   media → **negros ≥ #0A0A0B, blancos ≤ #D6C8A8, flicker <2 % y <10 Hz**.

Los seis vectores convergen en **C: monocromo modulado con un único acento
cromático retirado al inicio**. Ninguna otra opción satisface más de cuatro.

---

## Bibliografía

[1] Wandell, B. A. (1995). *Foundations of Vision*. Sinauer. (Vías de luminancia vs. crominancia.)
[2] Livingstone, M., & Hubel, D. (1988). "Segregation of form, color, movement, and depth". *Science*, 240, 740–749.
[3] Kurosu, M., & Kashimura, K. (1995). *CHI '95 Companion*, 292–293.
[4] Tractinsky, N., Katz, A., & Ikar, D. (2000). "What is beautiful is usable". *Int. J. Human-Computer Studies*, 53, 127–145.
[5] Fisher, M. (2016). *The Weird and the Eerie*. Repeater. (Liminalidad y espacios de tránsito.)
[6] Metzger, W. (1930). "Optische Untersuchungen am Ganzfeld". *Psychologische Forschung*, 13.
[7] Ullman, S., et al. (2002). *Nature Neuroscience*, 5, 452–457; Liu, J., et al. (2014). "Face perception in noise". *Cortex*.
[8] Elliot, A. J., et al. (2007). *JEP: General*, 136(1), 154–168; Elliot & Maier (2012), color-in-context.
[9] Elliot, A. J., et al. (2009). *PSPB*, 35(3), 365–375.
[10] Valdez, P., & Mehrabian, A. (1994). *JEP: General*, 123(4), 394–409.
[11] Berlyne, D. E. (1971). *Aesthetics and Psychobiology*; Vroegh, P. (2017). *Psychology of Music*, 45(6).
