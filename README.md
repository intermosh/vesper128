# VÉSPER 128 — v0.0.1

Sistema operativo ficticio de los años 80 como pieza de horror liminal: una máquina
que nunca fue de nadie y que, sin embargo, registra al quien la usa.
Todo el contenido (narrativa, grafo de descubrimientos, textos, sonidos, estética CRT)
vive en **datos**; el código sólo es un motor que los interpreta.

## Cómo correrlo

Necesita servirse por HTTP (módulos ES6 + `fetch` de JSON):

```bash
cd vesper128
python3 -m http.server 8100 --bind 0.0.0.0
# juego:   http://localhost:8100/index.html
# editor:  http://localhost:8100/editor.html
```

Sin narrativa (sólo el SO funcional): poner `runtime.narrative: false` en `data/game.json`.

## Arquitectura

```
index.html            arranque del juego (sin lógica embebida)
editor.html           editor web (grafo, catálogo, pantalla, audio, datos)
css/base.css          reset + layout común
css/game.css          escenario del juego (canvas, capas CRT)
css/editor.css        interfaz del editor
js/core/              util, framebuffer (512×342 1-bit), icons (sprites), input,
                      events (bus), audio (WebAudio sintetizado), crt (curvatura/
                      viñeta/bloom/rodadura), config (carga y acceso a game.json)
js/os/                ui (ventanas/menús/diálogos), desktop (iconos, anomalías de
                      escritorio), filesystem (carpetas, docs con envoltura
                      adaptativa, papelera, apps)
js/game/              story (motor narrativo: grafo, tensión, flags, anomalías),
                      effects (catálogo ejecutable), apps (OYENTE, VISITANTE,
                      ACERCA, CAJÓN), boot (warn→power→post→happy→desktop→ending),
                      main (bucle, input, contexto compartido)
js/editor/            store (fuente de verdad + guardar/cargar/validar), graph
                      (lienzo drag&drop), inspector (paneles reactivos), panels
                      (catálogo/pantalla/audio/datos), preview (mini-CRT en vivo),
                      dom (helpers), editor (ensamblaje)
data/game.json        TODO el contenido configurable (compartido con el editor)
data/effects.json     catálogo externo de efectos (ampliable sin tocar código)
```

### El JSON compartido (`data/game.json`)

- `meta`, `runtime`, `display` (resolución, escala, `crt`: curvatura, viñeta, bloom,
  rodadura, parpadeo, ruido, glitch), `audio`, `clock`, `ui`, `desktop`, `boot`.
- `fs`: árbol de nodos (`folder/doc/sound/app/trash`) con `locked`, `hidden`, `doc`.
- `docs`: textos con entradas progresivas (`grow`) que se revelan al hacer scroll.
- `sounds`: registros sintetizados (frecuencias, duraciones, capas).
- `apps`: configuración declarativa de OYENTE / VISITANTE (live) / ACERCA / CAJÓN.
- `menus`: barra y menús condicionales (`showIf`, `needsFlag`, `alwaysDisabled`,
  `effects` por ítem) + menús extra inyectados por la historia (`extra`).
- `anomalies`: pozo de eventos ambientales con `minT` (tensión mínima) y peso.
- `story`: grafo de habilitación.
  - **nodos**: `{ id, label, type: start|event|auto, trigger {type,event,match,ms},
    conditions[], effects[], repeat?, cooldown?, x, y }`.
    - `repeat: true` → el nodo puede re-dispararse (avisos de clave, bloqueos).
    - Un nodo queda *descubierto* al activarse; eso habilita sus aristas salientes.
  - **aristas**: `{ from, to, conditions[] }`. Varias aristas entrantes = OR;
    las condiciones de la arista = ramas (flag / tension / time / node / always…).
  - **condiciones**: `{ kind: 'flag'|'tension'|'time'|'node', name|value|op|node, negate }`.
  - **efectos**: `{ effect, params, delay?, enabled? }` — se ejecutan en orden.
- `ending`, `subliminal`, `dialogs`, `sprites` (overrides de iconos desde el editor).

### Catálogo de efectos (`data/effects.json`)

41 efectos en categorías (diálogos, archivos, escritorio, pantalla, audio, tensión,
flujo). Cada entrada declara `params` con tipo y default: el editor genera los
widgets automáticamente. Añadir un efecto nuevo = añadir su entrada al JSON y, si
procede, una implementación `define('id', fn)` en `js/game/effects.js`; sin
implementación, el motor lo avisa y lo ignora (nunca rompe).

## Editor (`editor.html`)

- **Grafo**: arrastrar nodos; arrastrar desde el puerto de salida de un nodo hasta
  otro para crear aristas; clic en arista para seleccionarla; `Supr` borra lo
  seleccionado; `N` nodo nuevo; `F` encuadrar; rueda = zoom; shift/rueda media = pan.
  El inspector edita disparador (evento + match JSON), condiciones (ramas), efectos
  (picker del catálogo con parámetros), repetibilidad y posición.
- **Catálogo**: lista externa de efectos con sus parámetros y descripción.
- **Pantalla**: resolución/escala/tipografía y CRT completo (curvatura sutil por
  defecto 0.14, viñeta, bloom, rodadura, parpadeo, ruido, glitch) con preview en vivo
  y tensión de prueba.
- **Audio / Datos**: capas de sonido y edición directa del JSON.
- **Validar**: consistencia del grafo (nodos huérfanos, banderas sin origen, rutas
  inexistentes, efectos sin implementación…).
- **Guardar**: escribe `game.json` (File System Access o descarga). El juego lee ese
  mismo archivo: editar → guardar → recargar el juego es todo el ciclo.

## Pruebas (`/home/user/testharness`)

- `prod.js` — walkthrough completo con puppeteer: encendido, cuaderno progresivo,
  envoltura de texto al maximizar, clave incorrecta/correcta, DISCO 2, OYENTE,
  registro de visitas, papelera (vaciar vacía + arrastre), menús condicionales y
  final completo. Imprime flags/nodos descubiertos y capturas en `shots2/`.
- `ed.js` — interacciones del editor: arrastre, aristas, inspector, efectos,
  condiciones, pestañas, validación, guardado, zoom.
- `smoke.js` — carga limpia de ambas páginas (0 errores de consola).

## Notas de diseño

Horror sin jumpscares: incomodidad atribuible (infrasenso 18,97 Hz, fading de
Troxler, ganzfeld, ceguera al cambio, uncanny arquitectónico). La estética amable
de los 80 baja la guardia; las anomalías son siempre supraliminares pero
desatendidas. Bibliografía y análisis relacional en los documentos de investigación
del proyecto (`/home/user/vesper-128/` contiene el prototipo autocontenido original).
