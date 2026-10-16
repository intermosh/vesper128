# VÉSPER 128 — Investigación: psicología aplicada a UI para terror liminal

> Objetivo: qué mecanismos perceptuales y cognitivos permiten que una interfaz de
> sistema operativo provoque *eerie* / liminalidad / extrañamiento sin sobresaltos
> ni contenido explícito, y cómo traducirlos a parámetros de diseño que el motor de
> VÉSPER 128 pueda sostener de verdad — no solo nombrar.
>
> `[ref]` remite a la bibliografía del final. Cada cita tiene una fuente rastreable.
> Donde la evidencia es débil, mixta o no replicó, se dice explícitamente, y la
> aplicación de diseño se apoya en lo que sobrevive de esa evidencia, no en la
> versión más fuerte del titular.

---

## 0. Tesis de diseño

El *jump-scare* asusta por atribución correcta: el jugador sabe exactamente qué lo
asustó. Lo que buscamos es distinto: activación fisiológica sin una causa clara a
la que asignarla. Schachter y Singer propusieron que la emoción es arousal
inespecífico más una etiqueta cognitiva que el sujeto le pone [19]; Dutton y Aron
mostraron un caso — arousal de un puente colgante reinterpretado como atracción —
que se volvió el ejemplo de cabecera del fenómeno [20], aunque replicaciones
posteriores no siempre reproducen el efecto con esa fuerza (Reisenzein hace el
balance dos décadas después: el núcleo de la idea resiste, la versión fuerte no
siempre) [21]. Tomamos esto como una heurística de diseño razonable, no como una
ley demostrada: activación fisiológica por canales que el jugador no puede
inspeccionar (periferia, timing, audio de baja frecuencia), y ninguna causa obvia
en el foco de atención.

Hay un límite real a esto que vale nombrar de entrada: el jugador ya sabe que está
jugando un juego de terror. El género pre-etiqueta el arousal general ("esto da
miedo porque es un juego de terror"). Lo que la ambigüedad puede lograr no es
impedir esa etiqueta general — es negar una etiqueta *específica*: qué icono, qué
sonido, qué ventana fue. El objetivo realista es que el jugador nunca pueda señalar
el evento concreto, no que dude de estar jugando algo diseñado para asustar.

> **Principio rector:** el foco de atención permanece inocente; la amenaza vive en
> lo que el sistema visual descarta — periferia, adaptación, memoria de escena,
> ruido.

---

## 1. Lo *eerie*, lo liminal y lo *unheimlich*

Freud definió *das Unheimliche* como lo familiar que se vuelve inquietante por
retorno de algo reprimido: lo *heimlich* (doméstico, conocido) da vuelta a su
propio contrario [1]. Fisher tradujo esto en términos más simples — *un-homely*,
el hogar dejado de ser hogar — y lo separó de lo *weird* (lo que directamente no
pertenece) [2]. Vidler llevó la misma idea a la arquitectura: el espacio doméstico
que revela una extrañeza que siempre estuvo latente [3].

Un escritorio de sistema operativo es, para cualquier usuario alfabetizado
digitalmente, de los espacios más *heimlich* que existen: barra de menú, papelera,
disquete, reloj. Por eso el *desktop horror* tiene algo que el cine de terror no
tiene tan fácil: el jugador ya sabe usar ese lugar, así que cualquier desviación
mínima — un ítem de menú nuevo, un icono corrido ocho píxeles — es una violación
del hogar, no un monstruo importado de afuera.

La aportación específica de Fisher es la pregunta por la agencia: lo eerie aparece
cuando hay más o menos agencia de la que debería haber — ¿hay algo haciendo esto,
o no hay nada y esto se hace solo? [2]. De ahí una regla de diseño concreta: el
sistema nunca debe comportarse de forma caótica (eso es avería, *weird*), sino con
un orden ligeramente excesivo — una ventana que se abre sola pero "prolijamente",
una papelera que devuelve lo borrado. El desorden asusta menos que el orden que no
debería estar ahí.

La liminalidad completa el cuadro: los espacios de tránsito sin ocupante previsto
—pasillos, salas de espera— inquietan porque su función presupone ausencia [2]. Un
sistema recién arrancado *es* liminal por definición: un espacio de espera entre el
apagado y el trabajo, diseñado para atravesarse, no para habitarse. Mantener al
jugador detenido en ese umbral — un POST que tarda, un escritorio sin tarea
asignada, un reloj congelado — convierte la liminalidad estructural del arranque en
atmósfera sostenida.

Carroll agrega la pieza de las categorías: el monstruo de ficción funciona porque
viola el esquema conceptual vigente, y ese descubrimiento de lo imposible produce
asco y fascinación a la vez [4]. En un sistema operativo las categorías en juego son
las del sistema de archivos y del modelo de interacción: un documento que crece
mientras se lee, una carpeta cuya clave es una fecha, un disco que aparece sin
haberse conectado. La violación tiene que ser lógica, no gráfica — el monstruo de
VÉSPER 128 es una relación de pertenencia imposible, nunca una criatura dibujada.

Mori describió la caída de familiaridad cuando un artefacto se aproxima a lo humano
sin llegar [5]; MacDorman e Ishiguro lo formalizaron después [6]. Vale la pena ser
preciso con lo que esto sí y no dice para UI: no dice "poné caras". Dice que la
máxima inquietud vive en la casi-identidad — algo que el sistema visual clasifica
primero como correcto, y que una etapa posterior marca como erróneo. Un icono
idéntico al esperado con un detalle fuera de lugar perturba más que uno
directamente monstruoso, precisamente porque hay dos juicios en conflicto en vez de
uno. Es la lógica detrás de la cara feliz del arranque con el ojo corrido, o del
menú "OÍR" con tipografía perfectamente correcta: no se trata de acuidad visual —
de hecho un jugador puede ver perfectamente el desvío si lo busca — se trata de que
el desvío no aparece donde el jugador está mirando activamente, así que primero
pasa como correcto.

---

## 2. Percepción sin conciencia y sin atención

### 2.1 Priming: qué está bien establecido y qué no

Hay dos afirmaciones distintas que suelen mezclarse bajo "priming subliminal", y
conviene separarlas.

La primera — priming de identificación por enmascaramiento — está bien establecida:
un estímulo presentado brevemente y flanqueado por máscaras acelera el
procesamiento de un objetivo relacionado, sin que el sujeto pueda reportar haberlo
visto [7]. Dehaene y colegas mostraron activación cortical medible ante primas
enmascaradas de ~29 ms [8]; Greenwald, Draine y Abrams demostraron algo similar con
primas incluso más breves [9]. La condición operativa es el enmascaramiento: sin
una máscara antes y después, un estímulo de dos fotogramas (~33 ms a 60 Hz) suele
ser leíble, sobre todo si aparece siempre en la misma zona de la pantalla y el
jugador aprende dónde mirar. Esto importa para el diseño: si el destello se
presenta sin máscara circundante, hay que asumir que en algún punto de la sesión
el jugador lo va a leer conscientemente, y diseñar el contenido para que eso esté
bien — nunca para que dependa de permanecer invisible.

La segunda afirmación — priming conductual duradero, el tipo popularizado por el
estudio de caminar más lento tras primar la vejez [10] — está en una situación muy
distinta: las replicaciones con medición automatizada no lo reprodujeron, y en las
que sí encontraron algo, el efecto dependía de que el experimentador supiera qué
esperar [10]. No hay base para diseñar asumiendo que un estímulo breve puede
instalar una conducta o una creencia duradera en el jugador. Y por si hiciera
falta un tercer punto de referencia: las cintas de autoayuda "subliminales" de
los 90 se probaron a doble ciego y no produjeron ningún efecto específico
atribuible a su contenido oculto [11].

**Aplicación honesta.** VÉSPER 128 usa destellos de dos fotogramas con palabras del
léxico diegético ("ATRÁS", "SOPORTE", "18,97") asumiendo que en algún momento se
leen. Eso está bien: lo que hacen es sembrar vocabulario que después reaparece en
los documentos, de modo que el jugador siente que "ya lo sabía" sin poder decir
cuándo lo vio — un efecto de familiaridad más que de programación subliminal. Dos
consecuencias de tomar esto en serio: primero, ninguna de esas palabras puede ser
una instrucción dirigida al jugador ("APAGÁ", "NO MIRES") porque si se lee — y hay
que asumir que se lee — deja de ser vocabulario ambiental y pasa a ser el juego
hablándole directamente, que rompe el principio de "sin cuarta pared". Segundo, la
duración real del efecto no alcanza para "precargar" un diálogo que aparece
minutos después; alcanza para una palabra que resuena unos segundos. El destello
debe estar cerca, en el tiempo, de donde se paga.

### 2.2 Ceguera al cambio y ceguera por desatención

Rensink, O'Regan y Clark mostraron que cambios grandes en una escena pasan
desapercibidos si ocurren durante una interrupción transitoria — un parpadeo, un
corte, una mancha que cruza la imagen [12][13]. Simons y Chabris probaron lo mismo
con un estímulo saliente y sostenido: el gorila no se ve si la atención está puesta
en otra tarea [14]. Drew, Võ y Wolfe extendieron el resultado a expertos —
radiólogos que miran fijo una anomalía insertada y no la reportan [15].

**Aplicación.** Es la herramienta central para mover cosas sin que se sepa qué
cambió. Toda mutación de escena — un icono que se corre, un ítem que desaparece —
tiene que ejecutarse durante una perturbación que ocupe la atención: un parpadeo
negro breve, el zoom de apertura de una ventana, un scroll. El jugador percibe la
perturbación, no el cambio, y lo que le queda es una sensación de error propio
("juraría que estaba en otro lado") que no puede confirmar porque no hay
historial. Esa duda dirigida contra la propia percepción, sin dato que la
resuelva, es el núcleo de lo eerie tal como lo define este documento.

### 2.3 Atención periférica: lo que Troxler explica y lo que no

Con fijación *sostenida* en un punto, los estímulos periféricos estables se
desvanecen y el sistema los rellena con el fondo — el fading de Troxler, descripto
en 1804 y estudiado desde entonces. Es importante no estirar esto más de lo que
da: el efecto depende de que la mirada se mantenga clavada en un punto fijo
durante varios segundos, no de que el cursor esté quieto. Un jugador con el
cursor inmóvil puede estar mirando cualquier parte de la pantalla. Usar "Troxler"
como nombre para un icono que pierde contraste cuando el mouse lleva un rato lejos
es, en rigor, otra cosa: es una regla de atención sostenida general, más cercana a
"lo que no se toca se desvanece" que a la fisiología específica de la fijación
retiniana. Vale la pena diseñarlo con ese nombre más modesto, porque cambia cómo
se ajusta: el desvanecimiento debe ser gradual y reversible al primer movimiento
del mouse, nunca binario — la versión binaria no es un fundido, es que el icono
desaparece, y un icono que desaparece de golpe se nota mucho más de lo que
ayuda.

Un mecanismo relacionado y mejor documentado es la ceguera inducida por
movimiento: objetivos periféricos estables desaparecen cuando hay un patrón
dinámico superpuesto cerca de ellos, no simplemente en otra parte de la pantalla
[16]. Para usar esto de verdad hace falta que el elemento dinámico se superponga
visualmente al área del objetivo — una animación central no alcanza si el icono
que se quiere afectar está en la otra punta de la pantalla; el diseño tiene que
poner ambos elementos cerca.

### 2.4 Homogeneidad de campo: lo que hay y lo que no hay

La exposición a un campo sensorial homogéneo y sostenido produce, en minutos,
percepción espontánea de patrones y formas (Ganzfeld, descripto por Metzger en
1930) [17]. La condición es exigente: campo sin bordes que cubra todo el campo
visual, sostenido varios minutos. Un escritorio con iconos, menú y marco de
ventana no cumple ninguna de las dos condiciones, así que llamarlo "casi-Ganzfeld"
sería exagerar lo que puede producir. Lo que sí puede aprovecharse del mismo
principio, a escala mucho más modesta, es lo opuesto de la simulación
Ganzfeld completa: un campo vacío y uniforme, breve, sin interfaz encima. Eso es lo
que ya existe en el catálogo de efectos como campo en blanco de uno o dos
segundos — es la aplicación honesta de esta idea, no la trama del escritorio.

### 2.5 Pareidolia

El sistema visual detecta configuraciones que sugieren una cara con umbrales muy
bajos, incluso en ruido puro; hay evidencia de correlatos neuronales y
conductuales específicos para esto [18]. La configuración que dispara el efecto
con más consistencia en la literatura es simple: dos elementos arriba, uno abajo
— la estructura mínima de "dos ojos, una boca" — más que una proporción geométrica
particular. No hay evidencia de que la proporción áurea tenga un rol especial acá;
es una asociación popular sin respaldo específico, y conviene no usarla como
justificación técnica.

**Aplicación.** Los agrupamientos de densidad en la trama del escritorio deben
respetar la configuración dos-arriba-uno-abajo, no una proporción calculada, y
mantenerse por debajo del umbral en que el jugador podría señalar "eso es una
cara" — la gracia es que sienta que "hay algo" sin poder nombrarlo. Nunca se
dibuja una cara: se dibuja la estadística mínima de una.

---

## 3. Arousal, atribución y curso temporal

### 3.2 Berlyne y la curva de preferencia

Las variables colativas — novedad, complejidad, incertidumbre, conflicto — generan
potencial de activación, y la preferencia por ellas sigue una curva en U invertida:
gusta hasta cierto punto, después empieza a incomodar [22]. Una revisión de 57
estudios sobre preferencia musical confirmó el patrón cuando se controlan bien las
variables [23] — vale la aclaración de que esa revisión es sobre música, no sobre
interfaces; se usa acá por analogía, no como evidencia directa del dominio.

**Aplicación.** La curva de tensión del juego es una caminata deliberada hacia el
lado derecho de esa U: arranca en el vértice cómodo — nostalgia, orden, simetría —
y desplaza la incertidumbre poco a poco por encima del óptimo, hasta que el mismo
tipo de estímulo que resultaba agradable se vuelve aversivo. Por eso la
degradación tiene que ser lenta y continua: si hay un salto notorio, el jugador
tiene la oportunidad de re-etiquetar el evento concreto que lo produjo, y eso es
justo lo que se busca evitar.

### 3.3 Refuerzo de razón variable

Los programas de razón e intervalo variable producen las tasas de respuesta más
altas y las más resistentes a la extinción [24] — es el mecanismo detrás de
máquinas tragamonedas y de buena parte del diseño de scroll infinito. La
incertidumbre sobre el *cuándo* mantiene la atención en vigilancia sostenida.

**Aplicación.** Las anomalías ambientales no ocurren a intervalo fijo: se eligen
con probabilidad ponderada y el intervalo hasta la próxima combina una media que
baja con la tensión y variación aleatoria encima. El jugador no puede predecir ni
habituarse del todo, y la espera misma se vuelve parte de la tensión. Para que
esto funcione la varianza tiene que ser alta — un intervalo casi constante con
poco ruido encima se aprende rápido y deja de sorprender.

### 3.4 Indefensión aprendida

La exposición a eventos incontrolables produce pasividad y dificulta aprender
control después; el hallazgo original viene de perros expuestos a shock
inescapable [25], y Seligman lo extendió como marco general más tarde [26]. En una
interfaz, la forma directa de esto es retirar la contingencia entre lo que el
jugador hace y lo que el sistema responde.

**Aplicación, con el límite explícito.** En los actos tardíos: botones cuya área
sensible se corre unos píxeles respecto de lo que se ve, ventanas que no cierran
al primer intento, ítems de menú que se inhabilitan sin explicación. El límite no
es opcional: el control nunca se retira del todo ni de forma permanente, el
jugador siempre puede seguir avanzando. Lo que se dosifica es la fricción, no el
acceso.

### 3.5 Tareas interrumpidas

Zeigarnik describió una ventaja de memoria para tareas interrumpidas frente a
completadas [27] — un hallazgo que no siempre replica cuando se lo pone a prueba
con rigor moderno, así que conviene no apoyarse en la versión de "se recuerda
mejor". Lo que sí está mejor sostenido, del mismo programa de investigación, es
la tendencia a *retomar* una acción interrumpida — la presión motivacional hacia
cerrarla, más que el recuerdo de ella [28].

**Aplicación.** Dejar siempre una tarea abierta e incerrable — un documento sin
guardar cuyo botón de guardar está inhabilitado, una copia que no termina, un
archivo que sigue escribiéndose — no se apoya en que el jugador la vaya a
recordar mejor. Se apoya en que la deja tironeando: quiere volver a intentar
cerrarla, y esa presión lo mantiene en el escritorio más tiempo del que
planeaba.

---

## 4. Color, contraste y percepción visual

### 4.1 Color y afecto: lo que sostiene el meta-análisis

Elliot y colegas mostraron que el rojo, presentado brevemente antes de una tarea
de logro (un examen, un test de anagramas), induce motivación de evitación y
empeora el desempeño sin que el sujeto lo note [29][30]. El hallazgo generó una
teoría más amplia — que el significado del color depende del contexto en el que
aparece, no es fijo [31] — pero trabajos posteriores que agregaron los estudios
disponibles no encuentran un efecto robusto y general del rojo sobre el
rendimiento cognitivo fuera de ese contexto de logro muy específico. Dicho de
otra forma: el rojo no es una tecla de miedo universal, es un efecto acotado a una
situación experimental particular, y conviene no construir el diseño sobre la
versión fuerte del titular.

Hay además una razón práctica, aparte de la evidencia: en cualquier interfaz el
rojo ya es convención de error o alto — el jugador lo *lee* como "esto salió mal",
no lo siente como amenaza difusa. Un único rojo en un escritorio monocromo es un
faro consciente, exactamente lo contrario de lo que busca este proyecto.

Lo que sí está mejor sostenido es que placer y activación dependen más de
saturación y luminosidad que del tono específico [32]. Esto favorece directamente
al monocromo: se puede modular activación por luminancia sin necesitar ningún
color. Saturación cero no es ausencia de afecto — es ausencia de vocabulario
afectivo nombrable, que es justo lo que este proyecto necesita.

**Aplicación.** En una paleta monocroma no hay ningún tono que pueda dar miedo;
los vectores disponibles son luminancia, contraste, temperatura del "fósforo" y
trama. Eso es una ventaja, no una limitación: opera por debajo del vocabulario
consciente ("no es rojo, no es nada, es... gris"). El único acento cromático del
juego — el ámbar del arranque — funciona precisamente porque retirarlo es un
evento emocional en un juego que, de ahí en más, no tiene ningún otro color con
el que competir.

### 4.2 Incomodidad visual: frecuencias espaciales y temporales

Hay un hallazgo consistente en la literatura de estrés visual: las imágenes cuya
energía de contraste se aparta de la estadística natural 1/f — en particular con
exceso en frecuencias espaciales medias, alrededor de 3 ciclos por grado — se
juzgan incómodas de mirar y pueden asociarse a cefalea en sujetos sensibles
[33][34]. En el dominio temporal aparece un patrón análogo: el flicker con exceso
de energía en frecuencias medias temporales resulta incómodo, y el pico de riesgo
fotoparoxístico está documentado entre 12 y 30 Hz [35].

**Consecuencia de diseño, con la aritmética hecha.** Un damero a 3 ciclos por
grado no es cualquier patrón de un píxel: depende de cuántos píxeles ocupa cada
celda del damero, del tamaño de pantalla y de la distancia de lectura. Con una
celda de un solo píxel en un monitor de escritorio típico, visto a la distancia
habitual, la frecuencia real ronda los 10 a 20 ciclos por grado — muy por encima
de la banda que la literatura marca como incómoda, y por lo tanto un patrón que
no produce el efecto que se le atribuye, más allá de que a simple vista *se vea*
denso. Para acercarse a la banda de 2 a 3 ciclos por grado hace falta una celda
bastante más grande, del orden de 8 a 12 píxeles según la distancia asumida — un
damero visiblemente más grueso que un dither de un píxel. Esto es un dato de
diseño real, no una corrección cosmética: si la intención es usar esta banda como
"textura ligeramente aversiva pero técnicamente inobjetable", el tamaño de celda
tiene que estar calculado para la resolución y distancia reales del juego, no
asumido.

**Sobre seguridad, sin margen para interpretación.** Evitar por completo trenes
periódicos de luminancia en la banda de 12 a 30 Hz para cualquier modulación de
área grande. Los destellos narrativos son eventos únicos de uno o dos fotogramas,
nunca una secuencia repetida a esa frecuencia. La advertencia de fotosensibilidad
al inicio no es opcional ni decorativa.

### 4.3 Sobre "visión nocturna" en pantalla

Vale la pena ser preciso acá porque es fácil pedirle a la metáfora más de lo que
da. La visión mesópica y escotópica real — donde baja la sensibilidad cromática y
el pico de sensibilidad se corre hacia el azul-verde, el efecto Purkinje — ocurre
a niveles de luminancia mucho más bajos que los que cualquier pantalla
retroiluminada alcanza incluso con el brillo al mínimo. Bajar el "papel" de la
interfaz y subir el nivel de negro en los actos tardíos no reproduce visión
mesópica: es un cambio de contraste y temperatura de color, y punto. Es una
metáfora visual legítima — la interfaz se ve más fría, más apagada — pero no una
simulación de fisiología ocular, y no hay que prometer que "cuesta más leer"
salvo que el contraste efectivo realmente haya bajado lo suficiente como para que
eso sea cierto en la práctica, no solo en la intención.

### 4.4 Tonos de negro

| Uso | Valor | Razón |
|---|---|---|
| Negro de tinta | `#0A0A0B` | evitar negro puro: menos halación, menos "crushing" en pantallas OLED |
| Negro de trama/sombra | `#101012` | sombra por trama, no gris plano |
| Negro elevado (actos tardíos) | `#1A1B1E` | los negros "se lavan" con la tensión |
| Papel de arranque | `#D6C8A8` | fósforo ámbar — el único color del juego |
| Papel acto I | `#CBCAC7` | neutro, frío |
| Papel actos II–III | `#B0BECF` | deriva fría, sin volverse ilegible |

La regla de fondo: el negro nunca es absoluto y el blanco nunca es estable. La
deriva lenta de ambos extremos es el vector principal de la paleta — y conviene
verificar, cada vez que se ajusta, que el contraste resultante siga siendo legible:
con estos valores el contraste papel-tinta va de aproximadamente 12:1 al arranque
a 6-7:1 en tensión máxima, que sigue siendo perfectamente legible. Si en algún
ajuste futuro esa cifra bajara mucho más, dejaría de ser "interfaz que envejece" y
pasaría a ser interfaz rota — y esa es una frontera que vale la pena vigilar
numéricamente, no a ojo.

---

## 5. Forma, disposición y ritmo de la interfaz

### 5.1 Estética-usabilidad y su inversión

La belleza aparente de una interfaz predice mejor la usabilidad *percibida* que la
usabilidad real medida por separado [37], resultado que se replicó
transculturalmente [38]. La inversión útil para este proyecto: una interfaz que
arranca prolija, simétrica, con tipografía nítida, se gana la confianza del
jugador — y degradar esos mismos ejes (simetría, nitidez, armonía) más adelante
degrada esa confianza junto con la estética. La atribución de esa caída de
confianza no está garantizada por ningún estudio — es un salto que el diseño da,
no la investigación — pero es un salto razonable: no hay ningún otro elemento en
foco al que el jugador pueda culpar, así que lo más probable es que la caída se
sienta como "la máquina" en general, difusa, más que como un error de diseño
puntual.

### 5.2 Desvíos de grilla: por qué funcionan sin apelar a límites de acuidad

Un desvío de uno o dos píxeles en la alineación de un icono o un título es
perfectamente visible si alguien lo busca — la agudeza humana para detectar
desalineaciones (agudeza vernier) es del orden de segundos de arco, muchísimo más
fina que un píxel a distancia de lectura. Así que el efecto de estos desvíos no
viene de estar cerca de ningún umbral de detección: viene de que el sistema de
percepción usa expectativas de arriba hacia abajo para completar patrones
familiares, y una grilla que "ya conoce" tiende a leerse como correcta a menos que
alguien la mire con atención puntual. El jugador no deja de ver el desvío porque
no pueda verlo — lo pasa por alto porque no tiene motivo para revisar algo que ya
clasificó como "la grilla de siempre". Es el mismo mecanismo que el near-miss del
§1: el sistema perceptivo confía primero y corrige después, si corrige.

**Consecuencia práctica.** El desvío tiene que estar en una zona periférica del
campo de atención del momento — no en lo que el jugador está mirando activamente
— y no hace falta que sea sutil en términos de píxeles; alcanza con que no esté
en el centro de atención cuando ocurre.

### 5.3 Latencia de entrada: a favor de quién trabaja la ambigüedad

La ley de Fitts describe el tiempo de adquisición de un objetivo en función de su
distancia y tamaño [39]; el estado de flujo depende de retroalimentación inmediata
y contingente a la acción [40]. Romper esa contingencia en grados pequeños —
retardo de cursor variable, un clic que se registra tarde — produce
micro-frustración que cada evento individual no alcanza a justificar.

Hace falta ser preciso sobre a favor de quién juega esto. Un retardo de cien
milisegundos o más se siente, casi siempre, exactamente como lo que es: la
sensación familiar de "esta máquina anda lenta". Eso no es indeseable — es
*útil*, siempre que el diseño lo sepa y lo use: le da al jugador una etiqueta
disponible ("es la máquina") que no es la etiqueta que en el fondo se está
buscando instalar (algo más cercano a "acá hay algo, y no sé qué es"). La
latencia funciona mejor como una fuente más de evidencia ambiental — junto con el
resto — que como el vector principal de ambigüedad; sola, tiende a resolverse
demasiado fácil.

### 5.4 Patrones oscuros como gramática

Los patrones oscuros documentados por Gray y colegas muestran que una interfaz
puede mentir con affordances formalmente válidos [41]; Norman y Gibson dieron el
marco teórico de qué son las affordances y cómo se perciben [42]. VÉSPER usa
affordances veraces en forma y falsos en consecuencia: un botón "Detener" que
existe, responde, y cuyo efecto es un diálogo que explica que no se puede detener
lo que nunca empezó. La traición vive en la semántica, no en la forma — el sistema
sigue siendo legible en todo momento, lo que se corroe es la confianza en que
hacer lo correcto produce lo esperado.

---

## 6. Audio: frecuencias y texturas

### 6.1 Infrasonido: lo que hay, con la incertidumbre puesta donde va

Tandy y Lawrence documentaron un caso — una onda estacionaria de aproximadamente
19 Hz producida por un extractor en un laboratorio, asociada a sensación de
presencia y una "figura gris" periférica que desapareció al corregir el
ventilador [43]. Es un caso único, bien documentado, pero un caso; no es una
serie ni una réplica sistemática. Wiseman y colegas armaron después un
experimento con público real: un concierto donde algunas piezas llevaban
infrasonido a 17 Hz mezclado con la música, y el público reportó en promedio un
22% más de sensaciones inusuales durante esas piezas, sin poder detectar
conscientemente el estímulo [44]. Vale aclarar el estatus real de ese dato: fue
un experimento artístico-científico presentado como informe a un congreso de
divulgación, no un paper con revisión de pares, y los propios organizadores
describieron sus resultados como "tentativos". Un intento posterior, más
controlado, de construir una "sala embrujada" combinando infrasonido y campos
electromagnéticos no encontró el mismo efecto atribuible específicamente al
infrasonido [45]. El cuadro completo es: un mecanismo psicoacústico plausible,
sugerido por un caso y un experimento de divulgación con resultados moderados y
no replicados de forma controlada. Suficiente para uso artístico consentido y a
niveles seguros — no suficiente para prometer que el infrasonido va a producir
una sensación física específica en el jugador.

Y hay una limitación práctica que conviene no pasar por alto: 19 Hz y 38 Hz están,
en la práctica, fuera del rango que un parlante de notebook o unos auriculares de
consumo pueden reproducir a un nivel que importe. Sin auriculares decentes o un
sistema con graves reales, esa capa del audio no hace lo que el diseño le pide
que haga — queda, en el mejor de los casos, como un zumbido apenas perceptible.
El diseño tiene que asumir esto en vez de prometer un efecto físico: recomendar
auriculares explícitamente (ya está en la pantalla de advertencia) y tratar la
capa de infrasonido como una pieza narrativa y de ambiente — el sonido *es* el
macguffin de la historia — más que como una palanca psicoacústica garantizada.

### 6.2 Ruido de baja frecuencia

El ruido de baja frecuencia ambiental produce molestia y fatiga, y el efecto crece
con el nivel del ruido, no aparece "por debajo del umbral de audición consciente"
— el ruido en cuestión es audible, solo que grave y persistente [46]. El zumbido
de red (50 Hz en Argentina) y el silbido de flyback de un CRT (~15,6 kHz) son
firmas acústicas reconocibles de "máquina encendida": funcionan bien como cama
diegética que baja cuando la máquina "se enferma" — el silencio de lo que debería
zumbar resulta más inquietante que un ruido nuevo, y esa inversión no necesita
ningún respaldo especial de psicoacústica para funcionar; es simplemente romper
una expectativa que el jugador construyó él solo, escuchando.

### 6.3 Dos tonos próximos: lo que producen y cómo se llama

Dos tonos muy cercanos en frecuencia (diferencias de pocos hertz) generan un
batido lento y perceptible — una modulación de amplitud, un "latido" — no lo que
la literatura llama técnicamente rugosidad, que corresponde a diferencias más
rápidas, del orden de 15 a 35 Hz, y que se percibe como aspereza más que como
pulso [47]. Un cluster con un batido de 6 o 7 Hz es una disonancia lenta que no
resuelve, y es perfectamente útil para el diseño — solo que conviene llamarla por
su nombre correcto y no atribuirle el mecanismo de rugosidad rápida, porque eso
lleva a ajustarla mal si en algún momento se la quiere hacer más intensa (subir
la separación de frecuencia la acercaría a rugosidad real; mantenerla como está
la deja como pulso lento, que es lo que efectivamente se escucha hoy).

### 6.4 Estadística 1/f y apertura espectral

Las escenas y sonidos naturales siguen aproximadamente una caída 1/f; las
desviaciones con exceso de energía en bandas medias tienden a juzgarse como menos
naturales y más incómodas — el mismo principio del §4.2, ahora en el dominio
sonoro [33][35]. Una cama de ruido rosa que abre su filtro hacia agudos con la
tensión se aleja gradualmente de esa estadística cómoda: el mismo ruido se
vuelve "áspero" sin cambiar de fuente ni de volumen general, solo de color
espectral.

---

## 7. Ética y límites del diseño subliminal

1. **No se manipula conducta duradera.** Todo priming en este juego es semántico,
   breve, y su efecto no sobrevive al cierre de la sesión. No se usa priming
   conductual — además de tener respaldo débil, cruzaría una línea de
   consentimiento que este proyecto no está dispuesto a cruzar.
2. **Consentimiento informado mínimo.** Pantalla de advertencia — fotosensibilidad,
   infrasonido, duración de la sesión — que además sirve como gesto de usuario
   necesario para habilitar audio. Modo reducido siempre disponible.
3. **Sin cuarta pared coercitiva.** El juego no pide ningún dato real del jugador
   más allá de lo que el navegador ya expone por su cuenta (zona horaria), lo usa
   solo como material diegético, y no lo envía a ningún lado — es un archivo
   local, sin red.
4. **Salida siempre visible.** Escape y cerrar funcionan siempre. El control se
   degrada — más fricción, más latencia — pero nunca desaparece.
5. **Seguridad fotosensible.** Sin trenes de flicker entre 12 y 30 Hz bajo ninguna
   circunstancia; destellos únicos de uno o dos fotogramas a contraste moderado;
   advertencia explícita antes de empezar.

---

## 8. Hipótesis de diseño sin respaldo empírico directo

Vale la pena decir con todas las letras cuáles de las ideas de este documento son
heurísticas de diseño razonables y cuáles están respaldadas por evidencia
publicada, porque tratarlas igual sería el error que este documento intenta
evitar en primer lugar. Lo que sigue no tiene una cita que lo sostenga — son
decisiones de diseño hechas *a partir* de los mecanismos de arriba, por analogía
o extensión, no hallazgos:

- Que el valle inquietante se pueda aplicar a un icono de interfaz y no solo a
  rostros o cuerpos — es una extensión razonable del principio de casi-identidad,
  no algo que la bibliografía de robótica social haya probado para UI.
- Que degradar la estética-usabilidad haga que el jugador le eche la culpa "a la
  máquina" y no al diseño — es plausible porque no hay otro candidato visible al
  que culpar, pero es una inferencia del equipo, no un resultado medido.
- Que la ceguera al cambio, aplicada repetidamente, erosione la confianza general
  del jugador en su propia percepción — es la lectura narrativa que se le quiere
  dar al mecanismo, más allá de lo que los estudios originales midieron (que era
  detección de un cambio puntual, no acumulación de duda a lo largo de una
  sesión).
- Que el refuerzo de razón variable, pensado originalmente para eventos
  *deseables* con recompensa, funcione igual para eventos ambientales aversivos
  y no contingentes con ninguna acción del jugador — la lógica de "no se puede
  predecir ni habituar" se sostiene, pero el mecanismo de Ferster y Skinner
  describe respuesta a recompensa, no vigilancia frente a una amenaza ambiental
  sin refuerzo positivo de por medio.

Ninguna de estas cuatro es mala idea de diseño. Son buenas ideas de diseño que no
deberían presentarse con el peso de una cita, porque no lo tienen.

---

## 9. Matriz técnica → mecanismo → parámetro

| # | Técnica | Mecanismo citado | Estado de la evidencia | Parámetro en VÉSPER 128 |
|---|---|---|---|---|
| 1 | Destello breve con vocabulario diegético | priming por enmascaramiento [7][8][9] | sólido si hay máscara; sin ella, leíble en algún punto de la sesión | 2 fotogramas, contraste 0.44, solo vocabulario neutro — nunca instrucciones |
| 2 | Mutación durante perturbación | ceguera al cambio [12][13][14] | sólido | iconos que se corren durante un parpadeo enmascarante de ~140 ms |
| 3 | Desvanecimiento por desatención sostenida | analogía con Troxler, no el mecanismo exacto | heurística razonable, no fisiología literal | icono que pierde contraste gradualmente, nunca de golpe, tras varios segundos sin mouse cerca |
| 4 | Campo vacío breve | Ganzfeld [17] | el escritorio con iconos no cumple las condiciones; el campo vacío sí, a escala menor | 1-2 s de gris uniforme sin interfaz, en momentos puntuales de tensión alta |
| 5 | Agrupamientos de densidad | pareidolia [18] | sólido para la configuración dos-arriba-uno-abajo; sin base la proporción áurea | *clusters* de trama con esa geometría, por debajo del umbral de reconocimiento consciente |
| 6 | Degradación lenta de paleta | análogo de mesópica/Purkinje, tratado como metáfora | metáfora visual válida, no simulación fisiológica | contraste 12:1 → 6-7:1, siempre verificado numéricamente |
| 7 | Trama de frecuencia media | incomodidad por desvío de 1/f [33][34] | sólido, pero exige el tamaño de celda correcto | celda de dither calculada para caer cerca de 2-3 c/grado a la distancia asumida, no un dither de 1 píxel |
| 8 | Latencia de entrada variable | ruptura de contingencia [39][40] | sólido como micro-frustración; la atribución más probable es "la máquina anda lenta" | retardo de cursor variable, usado como una fuente más de ambigüedad, no la única |
| 9 | Anomalías a intervalo variable | razón/intervalo variable [24] | sólido | media que baja con la tensión, variación aleatoria amplia encima |
| 10 | Affordance veraz / consecuencia falsa | patrones oscuros [41], affordances [42] | sólido | "Detener" que existe y explica que no hay nada que detener |
| 11 | Indefensión dosificada y reversible | indefensión aprendida [25][26] | sólido, con el límite ético explícito | deriva de área sensible, cierre que falla una vez, nunca pérdida total de control |
| 12 | Tarea incerrable | tendencia a retomar, no ventaja de memoria [28] | el mecanismo de retomada está mejor sostenido que el de memoria | documento que sigue escribiéndose, guardar inhabilitado |
| 13 | Near-miss icónico | valle inquietante [5][6], vía expectativa top-down | sólido como mecanismo perceptivo general | detalle fuera de lugar en zonas que no están bajo atención activa |
| 14 | Agencia sin autor | eerie / agencia [2] | conceptual, no medible en laboratorio | archivos reordenados "mejor", papelera que devuelve |
| 15 | Infrasonido a 18,97 Hz | caso único + experimento de divulgación [43][44][45] | sugestivo, no replicado bajo control; además limitado por hardware de audio | capa continua narrativa, pensada para auriculares, sin promesa de efecto físico garantizado |
| 16 | Batido lento sin resolución | modulación de amplitud, no rugosidad rápida [47] | correcto si se lo llama por su nombre | cluster con separación de ~6 Hz, entendido como pulso, no aspereza |
| 17 | Violación de categorías | monstruo por transgresión conceptual [4] | conceptual | archivo que crece al leerse, disco sin conexión, clave que es una fecha |
| 18 | Liminalidad sostenida | umbral de tránsito [2][3] | conceptual | POST largo, escritorio sin tarea, reloj congelado |
| 19 | Estética-usabilidad invertida | belleza percibida vs. real [37][38] | sólido el efecto base; la atribución "a la máquina" es hipótesis de diseño (§8) | confianza alta al inicio, degradada junto con la estética |

---

## Bibliografía

[1] Freud, S. (1919). *Das Unheimliche*. Imago, 5(5–6), 297–324.
[2] Fisher, M. (2016). *The Weird and the Eerie*. London: Repeater Books.
[3] Vidler, A. (1992). *The Architectural Uncanny: Essays in the Modern Unhomely*. MIT Press.
[4] Carroll, N. (1990). *The Philosophy of Horror, or: Paradoxes of the Heart*. Routledge.
[5] Mori, M. (1970/2012). "The Uncanny Valley". *IEEE Robotics & Automation Magazine*, 19(2), 98–100.
[6] MacDorman, K. F., & Ishiguro, H. (2006). "The uncanny advantage of using androids in social and cognitive science research". *Interaction Studies*, 7(3), 297–337.
[7] Marcel, A. J. (1983). "Conscious and unconscious perception: Experiments on visual masking and word recognition". *Cognitive Psychology*, 15(2), 197–237.
[8] Dehaene, S., et al. (1998). "Imaging unconscious semantic priming". *Nature*, 395, 597–600.
[9] Greenwald, A. G., Draine, S. C., & Abrams, R. L. (1996). "Three cognitive markers of unconscious semantic activation". *Science*, 273(5282), 1699–1702.
[10] Doyen, S., Klein, O., Pichon, C.-L., & Cleeremans, A. (2012). "Behavioral Priming: It's All in the Mind, but Whose Mind?". *PLoS ONE*, 7(1), e29081. — sobre Bargh, Chen & Burrows (1996), *JPSP*, 71, 230–244.
[11] Greenwald, A. G., Spangenberg, E. R., Pratkanis, A. R., & Eskenazi, J. (1991). "Double-blind tests of subliminal self-help audiotapes". *Psychological Science*, 2(2), 119–122.
[12] Rensink, R. A., O'Regan, J. K., & Clark, J. J. (1997). "To see or not to see: The need for attention to perceive changes in scenes". *Psychological Science*, 8(5), 368–373.
[13] O'Regan, J. K., Rensink, R. A., & Clark, J. J. (1999). "Change-blindness as a result of 'mudsplashes'". *Nature*, 398, 34.
[14] Simons, D. J., & Chabris, C. F. (1999). "Gorillas in our midst: Sustained inattentional blindness for dynamic events". *Perception*, 28(9), 1059–1074.
[15] Drew, T., Võ, M. L.-H., & Wolfe, J. M. (2013). "The invisible gorilla strikes again: Sustained inattentional blindness in expert observers". *Psychological Science*, 24(9), 1848–1853.
[16] Bonneh, Y. S., Cooperman, A., & Sagi, D. (2001). "Motion-induced blindness in normal observers". *Nature*, 411, 798–801.
[17] Metzger, W. (1930). "Optische Untersuchungen am Ganzfeld". *Psychologische Forschung*, 13, 6–29.
[18] Liu, J., Li, J., Feng, L., Li, L., Tian, J., & Lee, K. (2014). "Seeing Jesus in toast: Neural and behavioral correlates of face pareidolia". *Cortex*, 53, 60–77.
[19] Schachter, S., & Singer, J. (1962). "Cognitive, social, and physiological determinants of emotional state". *Psychological Review*, 69(5), 379–399.
[20] Dutton, D. G., & Aron, A. P. (1974). "Some evidence for heightened sexual attraction under conditions of high anxiety". *Journal of Personality and Social Psychology*, 30(4), 510–517.
[21] Reisenzein, R. (1983). "The Schachter theory of emotion: Two decades later". *Psychological Bulletin*, 94(2), 239–264.
[22] Berlyne, D. E. (1971). *Aesthetics and Psychobiology*. Appleton-Century-Crofts.
[23] Chmiel, A., & Schubert, E. (2017). "Back to the inverted-U for music preference: A review of the literature". *Psychology of Music*, 45(6), 886–909.
[24] Ferster, C. B., & Skinner, B. F. (1957). *Schedules of Reinforcement*. Appleton-Century-Crofts.
[25] Seligman, M. E. P., & Maier, S. F. (1967). "Failure to escape traumatic shock". *Journal of Experimental Psychology*, 74(1), 1–9.
[26] Seligman, M. E. P. (1972). "Learned helplessness". *Annual Review of Medicine*, 23, 407–412.
[27] Zeigarnik, B. (1927). "Über das Behalten von erledigten und unerledigten Handlungen". *Psychologische Forschung*, 9, 1–85.
[28] Ovsiankina, M. (1928). "Die Wiederaufnahme unterbrochener Handlungen". *Psychologische Forschung*, 11, 302–379.
[29] Elliot, A. J., Maier, M. A., Moller, A. C., Friedman, R., & Meinhardt, J. (2007). "Color and psychological functioning: The effect of red on performance attainment". *Journal of Experimental Psychology: General*, 136(1), 154–168.
[30] Elliot, A. J., Maier, M. A., Binser, M., Friedman, R., & Pekrun, R. (2009). "The effect of red on avoidance behavior in achievement contexts". *Personality and Social Psychology Bulletin*, 35(3), 365–375.
[31] Elliot, A. J., & Maier, M. A. (2012). "Color-in-context theory". En *Advances in Experimental Social Psychology*, vol. 45. Academic Press.
[32] Valdez, P., & Mehrabian, A. (1994). "Effects of color on emotions". *Journal of Experimental Psychology: General*, 123(4), 394–409.
[33] Fernandez, D., & Wilkins, A. J. (2008). "Uncomfortable images produce non-sparse responses". *Perception*, 37(7), 1098–1113.
[34] Juricevic, I., Land, L., Wilkins, A., & Webster, M. (2010). "Visual discomfort and natural image statistics". *Perception*, 39(7), 884–899.
[35] Yoshimoto, S., Garcia, J., Jiang, F., Wilkins, A. J., Takeuchi, T., & Webster, M. A. (2017). "Visual discomfort and flicker". *Vision Research*, 138, 18–28.
[36] Purkinje, J. E. (1825). Observaciones originales sobre el desplazamiento espectral en visión escotópica; ver también la curva de luminosidad escotópica CIE 1951 para la referencia moderna.
[37] Kurosu, M., & Kashimura, K. (1995). "Apparent usability vs. inherent usability". *CHI '95 Conference Companion*, 292–293.
[38] Tractinsky, N. (1997). "Aesthetics and apparent usability". *CHI '97*, 115–122; Tractinsky, N., Katz, A., & Ikar, D. (2000). "What is beautiful is usable". *International Journal of Human-Computer Studies*, 53, 127–145.
[39] Fitts, P. M. (1954). "The information capacity of the human motor system in controlling the amplitude of movement". *Journal of Experimental Psychology*, 47(6), 381–391.
[40] Csikszentmihalyi, M. (1990). *Flow: The Psychology of Optimal Experience*. Harper & Row.
[41] Gray, C. M., Kou, Y., Battles, B., Hoggatt, J., & Toombs, A. L. (2018). "The Dark (Patterns) Side of UX Design". *CHI '18: Proceedings of the 2018 CHI Conference on Human Factors in Computing Systems*, Paper 534.
[42] Norman, D. A. (1988). *The Psychology of Everyday Things*. Basic Books; Gibson, J. J. (1979). *The Ecological Approach to Visual Perception*. Houghton Mifflin.
[43] Tandy, V., & Lawrence, T. R. (1998). "The Ghost in the Machine". *Journal of the Society for Psychical Research*, 62.
[44] Wiseman, R., Watt, C., O'Keeffe, C., Angliss, S., Lord, R., & Simons, D. (2003). Proyecto "Infrasonic" / concierto "Soundless Music", Purcell Room, Londres — informe presentado a la British Association for the Advancement of Science. No publicado con revisión de pares.
[45] French, C. C., Haque, U., Bunton-Stasyshyn, R., & Davis, R. (2009). "The 'Haunt' project: An attempt to build a 'haunted' room by manipulating complex electromagnetic fields and infrasound". *Cortex*, 45(5), 619–629.
[46] Leventhall, G. (2004). "Low frequency noise and annoyance". *Noise & Health*, 6(23), 59–72.
[47] Helmholtz, H. von (1863). *Die Lehre von den Tonempfindungen*; Sethares, W. A. (1993). "Local consonance and the relationship between timbre and scale". *Journal of the Acoustical Society of America*, 94(3), 1218–1228.
