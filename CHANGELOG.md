# CHANGELOG — VÉSPER 128

Formato: [Keep a Changelog](https://keepachangelog.com/es/1.1.0/) + SemVer.
Este proyecto versiona esquema y contenido juntos: romper un campo de `game.json`
exige bump de MINOR y migración (ver CONTRATO_TECNICO §14).

## [Unreleased]

### Candidatos (deuda conocida, ver CONTRATO §13)
- Undo/redo y autosave en el editor.
- Tests unitarios del motor narrativo (hoy sólo E2E).
- Revisión manual de audio (no audible en headless).
- Cubrir `n_about`/nodos de menú con narrativa activa y tensión controlada.
- Perfil de rendimiento en hardware bajo.
- `happyWrong` (cara con el ojo corrido del arranque) es efectivamente
  inalcanzable: sólo se muestra en un segundo arranque con `Boot.bootCount>0` o
  con `Story.tension>0.9` durante el POST inicial, y ninguna de las dos
  condiciones ocurre en una partida normal (no hay flujo de reinicio in-fiction).
  Requiere una decisión de diseño (¿wire de un reinicio real?, ¿otro disparador?),
  no un fix de una línea — queda fuera de este parche.
- La trama del escritorio (dither de 1px) da ~10-20 c/grado, muy por encima de
  la banda de 2-3 c/grado que 01_investigacion_psicologia_ui.md §4.2 identifica
  como la aversiva. Cambiar el tamaño de celda es una decisión visual, no un bug;
  documentado y no tocado en este parche.

### Fixed — 0.0.2
Bugs de motor encontrados al verificar el pool de anomalías contra el código
real (no contra la especificación). Los cinco impedían que efectos del DGD
llegaran a ocurrir en una partida real:
- **Inacción nunca disparaba (Acto IV, DGD §6.3).** `Input.mouse.moved` se
  ponía en `true` en cada `mousemove` pero nunca se reseteaba, así que
  `UI.stillFor` quedaba en 0 para siempre y "¿Seguís ahí?" / "ACTIVIDAD EN
  REPOSO" eran inalcanzables. `js/game/main.js`: se consume el flag una vez
  por frame, igual que ya hacía `UI.mouse.moved`.
- **`unprompted` crasheaba (`Config is not defined`).** Los 12 diálogos sin
  autor de `dialogs.invitados` (Acto III, ~13% de las anomalías por peso)
  nunca se disparaban. `js/game/effects.js`: falta un `import { Config }`.
- **`ghost_window` crasheaba (`ctx.UI.winByTitle is not a function`).**
  `winByTitle`/`frontWin` son exports sueltos de `os/ui.js`, no métodos del
  objeto `UI`. `js/game/effects.js`: se usan las funciones importadas
  directamente.
- **`icon_shift` casi no movía nada.** El clamp topeaba en `FB.W-60`, por
  debajo de la posición de reposo de los iconos (`FB.W-54`), así que el primer
  desplazamiento hacia la derecha quedaba anulado por el clamp y, al marcar
  `keep=true`, el icono se congelaba ahí. `js/game/effects.js`: clamp
  ampliado a `[20, FB.W-40]`.
- **`ghost_cursor` casi no se movía.** `main.js` resincroniza
  `UI.mouse.rawX` con el mouse real al principio de cada frame, *antes* de
  llamar a `Story.tick()`; como el efecto escribía el avance directamente
  sobre `UI.mouse.rawX`, cada frame partía de cero y el desplazamiento nunca
  se acumulaba (quedaba en ~2% del recorrido total). `js/game/story.js` +
  `js/game/effects.js` + `js/game/main.js`: el cursor fantasma ahora lleva su
  propio estado (`Story.ghostCursor.x/y`) que persiste entre frames, y
  `main.js` lo aplica *después* de `Story.tick()`.

Verificado corriendo el pool completo de `anomalies.pool` con el motor real en
Node (headless) antes y después de cada fix, no sólo leyendo el código.

---

## [0.0.1] — 2026-09-18

Primera versión de producción: motor + editor + datos externos. Reemplaza al
prototipo autocontenido `[0.0.0]` como base de trabajo.

### Added
- **Arquitectura modular** vanilla ES6 sin dependencias: `js/core/*` (util,
  framebuffer 1-bit 512×342, icons, input, events, audio WebAudio, crt, config),
  `js/os/*` (ui, desktop, filesystem), `js/game/*` (story, effects, apps, boot,
  main), `js/editor/*` (store, graph, inspector, panels, preview, dom, editor).
- **`data/game.json`** como fuente única compartida juego/editor: meta, runtime,
  display+crt, audio, clock, ui, desktop, boot, fs, docs, sounds, apps, menus,
  dialogs, subliminal, anomalies, ending y story (30 nodos / 19 aristas).
- **`data/effects.json`** externo y ampliable: 41 efectos en 6 categorías con
  `params` tipados (widgets automáticos en el editor), `macro`, y catálogos de
  `triggers`, `events` y `conditions`.
- **Editor web** (`editor.html`): lienzo del grafo con drag&drop, puertos para crear
  aristas, pan/zoom/snap, atajos (N, F, Supr), inspector reactivo (disparador con
  match JSON, condiciones/ramas, efectos con picker y parámetros, repetible+
  cooldown, posición), pestañas Catálogo/Pantalla/Audio/Datos, preview CRT en vivo
  con tensión de prueba, validador de consistencia y guardado del mismo `game.json`.
- **Nodos repetibles**: campos `repeat`/`cooldown` en el motor + checkbox
  «repetible» en el inspector + badge `↻` en el grafo (avisos de clave y bloqueos
  que deben volver a salir).
- **SO funcional sin narrativa**: con `runtime.narrative:false`, soltar un icono en
  la papelera lo mueve a `Trash.items` (oculta nodo FS, quita icono) y vaciar limpia;
  sin flags, nodos ni menús narrativos.
- **Hooks de depuración**: `window.__VESPER__` (juego), `__STORE__`/`__GRAPH__`
  (editor), usados exclusivamente por el harness.
- **Harness de pruebas** (fuera del entregable): `prod.js` (walkthrough completo
  hasta el final), `ed.js` (interacciones del editor), `smoke.js` (carga limpia),
  `dbg10.js` (modo sin narrativa); capturas en `shots2/`.
- `<link rel="icon" href="data:,">` en ambas páginas (elimina 404 de favicon).
- Docs: `README.md`, `CONTRATO_TECNICO.md`, este `CHANGELOG.md`.

### Changed
- **Sin bisel de monitor**: la salida ocupa todo el viewport (se pidió explícitamente).
- **Envoltura de texto adaptativa**: los documentos recalculan columnas según el
  ancho actual de la ventana (maximizar incluidas); antes el texto desbordaba.
- **Curvatura CRT sutil** (0.14) por rebanadas de fila en `present()`, configurable
  desde el editor junto a scanlines/viñeta/bloom/roll/flicker/noise/glitch.
- `Especial › Vaciar papelera` emite `menu:emptyTrash` **también con la papelera
  vacía** (beat narrativo: ítems que vuelven); sin narrativa conserva el diálogo de
  SO «vacía».
- Texto del efecto `trash_return` para n=0: «La papelera estaba vacía. / Aun así,
  hay ítems nuevos.»
- `body.editor` con `align/justify-items: stretch` (el layout común centra por
  defecto y colapsaba el lienzo del editor).
- Sprites de iconos overridables vía clave `sprites` en `game.json`.

### Fixed
- **El final no se ejecutaba nunca**: `ctx` no incluía `Endgame`/`Boot`/
  `AudioOverride`, y el efecto `ending` moría silencioso en el `try/catch` del
  dispatcher. Añadidos al contexto compartido.
- Nodos `repeat` no marcaban `discovered`, rompiendo las aristas salientes
  (ANÁLISIS→REGISTRO quedaba inhabilitado tras un reintento de clave).
- Editor negro: `#main` con altura 0 por `align-items:center` heredado de
  `base.css` (no «fixear» dando altura explícita a `#main`).
- `preview.js`: el preview necesita `view`+CRT adjuntos **antes** de
  `apply()/sizeCanvas`.
- Inspector: no re-renderizar mientras hay un input enfocado (`renderSoft`).
- Harness: APIs correctas (`V.winByTitle` no está en `UI`), geometría de filas de
  carpeta (body y+20, paso 22), `bringFront` antes de clics solapados, `settle()`
  para el cursor con inercia, `clearAt()` para ventanas que tapan iconos,
  `dismissDialogs()` ante modales anómalos, y orden de pasos (papelera antes de
  DISCO 2; menús con narrativa pausada porque el modal COPIAR congela el escritorio).

### Verified (aceptación de la 0.0.1)
- `prod.js`: 21+ nodos descubiertos, todas las banderas del camino crítico
  (open_casa…read_last, played_172, unlock, read_registro, disk2, ran_oyente,
  visitor_folder, read_live, endgame, trash_fail), clave incorrecta+correcta,
  cuaderno 3→13, envoltura maximizada (68 cols), DISCO 2→OYENTE→VISITANTE,
  papelera (vaciar vacía + arrastre), menús condicionales (Canal B, Antonio, OÍR),
  final completo (`fase: ending`, `session: 1`). **0 errores / 0 avisos**.
- `ed.js`: arrastre de nodos, crear/borrar aristas por puertos, etiquetas reactivas,
  repetible on/off, añadir efecto desde catálogo con widgets, condiciones de arista
  serializadas (`{kind:'tension',op:'>=',value:0.55}`), N/Supr, pestañas, validación
  (0 errores), guardado (blob `game.json` parseable), zoom/fit. **0 errores**.
- `smoke.js`: juego y editor cargan sin errores ni avisos.
- `dbg10.js`: SO funcional puro con narrativa apagada.

---

## [0.0.0] — prototipo autocontenido (sesiones previas)

- Prototipo en un solo HTML (`/home/user/vesper-128/`) con toda la jugabilidad:
  boot, escritorio, cuaderno progresivo, clave 0210, DISCO 2, OYENTE, visitante,
  final; anomalías (Troxler, ganzfeld, infrasenso 18,97 Hz, ghost windows…).
- Investigación con bibliografía académica (Tandy & Lawrence 1998; Wiseman 2003;
  Elliot & Maier 2007/2008/2012; Troxler 1804; Metzger 1930; Fernandez & Wilkins
  2008; Simons & Chabris 1999; Fisher 2016; Freud 1919; Kurosu & Kashimura 1995;
  Schachter & Singer 1962; Berlyne; Carroll 1990) y análisis relacional B/N vs color
  → decisión de paleta 1-bit.
- Sirvió de referencia de contenido y feel para el port a producción; se conserva
  como pieza jugable independiente.
