# CONTRATO TÉCNICO — VÉSPER 128 (v0.0.1)

Documento de referencia vinculante para continuar el desarrollo. Define arquitectura,
contratos de runtime, datos, editor y pruebas. Cualquier cambio futuro debe respetar
los **invariantes** (§1) o actualizar este documento y el `CHANGELOG.md` en el mismo
commit.

---

## 1. Propósito, alcance e invariantes

VÉSPER 128 es un SO ficticio de los 80 (horror liminal, sin jumpscares) más un editor
web que permite configurar todo su contenido sin tocar código.

**Invariantes (no negociables):**

1. **Datos ≠ código.** Todo contenido (narrativa, textos, sonidos, estética, menús,
   grafo) vive en `data/game.json` y `data/effects.json`. El código sólo interpreta.
   Con `runtime.narrative: false` el juego debe seguir siendo un SO funcional completo
   (probar siempre, §10).
2. **Un solo JSON compartido.** Juego y editor leen/escriben el mismo `game.json`.
   El editor nunca duplica estructuras: las edita in-place sobre el objeto cargado.
3. **Sin dependencias externas** en runtime (vanilla ES6, módulos nativos). Puppeteer
   sólo existe en el harness de pruebas (fuera del entregable).
4. **Servir por HTTP.** Módulos ES6 + `fetch` no funcionan bajo `file://`.
5. **Estética:** 1-bit (2 tonos + tramas), resolución lógica fija **512×342**,
   barra de menú de 20 px. Curvatura CRT **sutil** (default 0.14) y configurable.
6. **Sin guías ni rupturas de la cuarta pared** dentro del juego; descubrimiento
   intuitivo y progreso gated (A habilita B) mediante el grafo.
7. **Texto adaptativo:** todo documento envuelve a las columnas que quepan en su
   ventana actual (recalcula al redimensionar/maximizar).
8. **Horror supraliminal pero desatendido:** nada subliminal afirmado como tal;
   anomalías visibles sólo si se buscan (base bibliográfica en investigación previa).

---

## 2. Arquitectura y módulos

```
index.html / editor.html      puntos de entrada (sin lógica embebida)
css/base.css                  reset + layout común (cuidado: body centra por defecto;
                              body.editor debe usar align/justify-items: stretch)
css/game.css                  escenario (canvas #view, capas #stage/#bloom)
css/editor.css                interfaz del editor
js/core/util.js               clamp/lerp/rnd/pick/chance, get/setPath, download, uid
js/core/framebuffer.js        FB: lienzo 1-bit 512×342, text/textC/textW, tramas,
                              palette por tensión, font (advance/size), menuBarH=20
js/core/icons.js              registro de sprites 32×32 y 16×16 (registerIcons)
js/core/input.js              ratón/teclado normalizados a coords lógicas (Input)
js/core/events.js             bus pub/sub (Events.on/emit)
js/core/audio.js              sintetizador WebAudio por capas + GainOverride (Map)
js/core/crt.js                CRT: curvatura por filas, scanlines, viñeta, bloom,
                              roll, flicker, noise, glitch (attach/configure/resize)
js/core/config.js             Config.load/get (ruta con puntos), merges de defaults
js/os/ui.js                   ventanas (newWin/closeWin/bringFront/frontWin/
                              winByTitle/body/partAt), diálogos modales (dialog/
                              dismissDialog/topModal), menús (setMenuItems/menuAt/
                              uiDraw), Clock, UI state (cursor, drag, latency…)
js/os/desktop.js              iconos de escritorio (addIcon/removeIcon/layout),
                              patrón de fondo, selección
js/os/filesystem.js           FS: nodos por path, open/openDoc/openTrash/folderOpen,
                              docLines (envoltura), grow on scrollend, Trash, apps FS
js/game/story.js              Story: grafo, tensión, flags, discovered, log,
                              anomalías ambientales, Subliminal, wireStoryEvents
js/game/effects.js            Registry + define(), runEffect/runList, loadCatalog
js/game/apps.js               OYENTE / VISITANTE(live) / ACERCA / CAJÓN (declarativos)
js/game/boot.js               Boot (fases) y Endgame (pasos 1-5)
js/game/main.js               bucle, input→acción, MENU_ACTIONS, ctx compartido,
                              rebuildMenus, exports de depuración (__VESPER__)
js/editor/store.js            Store (fuente de verdad), validate(), save/load
js/editor/graph.js            lienzo del grafo: drag, link entre puertos, pan/zoom
js/editor/inspector.js        paneles reactivos (nodo/arista/efectos/condiciones)
js/editor/panels.js           pestañas Catálogo/Pantalla/Audio/Datos
js/editor/preview.js          mini-CRT en vivo del editor
js/editor/dom.js              el/field/textInput/numInput/selectInput/checkInput/…
js/editor/editor.js           ensamblaje; expone __STORE__ y __GRAPH__
```

Regla de dependencias: `core` no importa de `os/game/editor`; `os` sólo importa
`core`; `game` importa `core`+`os`; `editor` importa `core` (+lectura de datos).
El editor **no** importa `game` (usa su propio preview).

---

## 3. Contrato de runtime

### 3.1 Coordenadas y escalado
- Todo se dibuja en coords lógicas 512×342 (`FB.W`, `FB.H`); `fitScale(scaleMode)`
  escala al viewport (`fill` por defecto). El harness convierte con el mismo cálculo.
- `UI.cursor` es el cursor **con inercia** (lerp); las acciones usan `UI.cursor`, no
  el ratón crudo. Con tensión alta puede haber `UI.latency` (retardo añadido).

### 3.2 Fases de Boot
`warn → power → post → happy → desktop` y, al final, `post2 → ending`.
- `warn`: pantalla de aviso (clic/tecla enciende).
- `power`: negro + sonido; `post`: líneas ROM (`boot.post.lines`).
- `happy`: logo; al terminar → `desktop` + `onDesktop()` (Story.activateStart).
- `post2`: líneas post-final; `ending`: dibuja Endgame (pasos ≥1).
- `Boot.bootCount` y `tension>0.9` eligen el variant `happyWrong`.

### 3.3 Endgame (pasos)
0 inactivo · 1 barra COPIANDO (3.2 s) · 2 slam blanco (1.4 s) · 3 `post2` ·
4 colapso CRT (2.6 s) · 5 negro + `offLabel` fundido. `Endgame.begin()` cierra
ventanas una a una (220 ms) y luego muestra el diálogo `ending` (modal).
`Story.session++` en `startCopy()`.

### 3.4 Tensión
`Story.tension` converge a `Story.target` (`dt/4200`). Gobierna: paleta, ruido,
flicker, glitch, audio (mix), `hitDrift` (≥0.55), `latency`, menús condicionales y
`minT` de anomalías. `setTension(v)` fija target (uso: tests/editor).

---

## 4. Contrato de eventos (bus `Events`)

Payload entre `{}`; el grafo filtra con `trigger.match` (igualdad exacta o
`{op,value}`). Emitidos por el SO (no por la narrativa):

| evento | payload | emisor |
|---|---|---|
| `open` | `{path, kind}` | FS.open / openDoc |
| `open:locked` | `{path, node}` | folderOpen/open sobre nodo `locked` |
| `open:hidden` | `{path}` | intento sobre nodo `hidden` |
| `open:unknown` | `{path, name}` | path inexistente |
| `open:generated` | `{path, name}` | fila auto-generada (registro live) |
| `read` | `{path, doc}` o `{path, app}` | openDoc / apps |
| `doc:grow` | `{path, shown, total}` | scroll al fondo de doc con `grow` |
| `scrollend` | `{path, y}` | fin de scroll |
| `play` / `play:empty` | `{id, path}` | reproducir sonido |
| `app` | `{app, button, path, repeat?}` | botones de apps |
| `close` / `close:refused` | `{path|title}` | cerrar ventana |
| `password` | `{path, value, ok}` | diálogo de clave |
| `menu` | `{action, label, arg?}` | ítem de menú ejecutado |
| `menu:emptyTrash` | `{count}` | Especial › Vaciar papelera (también con 0 ítems) |
| `drop:trash` | `{icon, name}` | soltar icono sobre papelera |
| `click` | `{where, x, y}` | clic en escritorio |
| `key` | `{key}` | teclas relevantes |
| `resize` | `{w, h}` | viewport |
| `desktop` | `{}` | escritorio listo |
| `discover` | `{id, node}` | Story al activar un nodo |

La narrativa **sólo escucha** estos eventos (`Story.onEvent`) o evalúa por tick
(`auto`, `idle`). El SO nunca consulta nodos del grafo directamente.

---

## 5. Contrato del motor narrativo (`Story`)

- **Nodo**: `{id, label, type: start|event|auto, trigger, conditions[], effects[],
  repeat?, cooldown?, x, y}`. `trigger`: `{type:'event', event, match}` |
  `{type:'auto'}` | `{type:'idle', ms}` | `{type:'start'}`.
- **Activación**: `canActivate` (no descubierto, o `repeat` y cooldown vencido) ∧
  `isEnabled` (alguna arista entrante con origen descubierto y condiciones de arista
  verdaderas; sin aristas entrantes: condiciones propias del nodo) ∧ trigger/condiciones.
- **Al activar**: se marca `discovered` **siempre** (aun siendo `repeat`: habilita
  aristas salientes), se fija `_last`, emite `discover` y ejecuta `effects` en orden.
- **Aristas**: `{from, to, conditions[]}`. Entrantes múltiples = **OR**; condiciones
  de arista = ramas. Condiciones: `{kind: flag|tension|time|node|always|never,
  name|value|op|node, negate?}`.
- **repeat/cooldown**: para diálogos que deben volver (clave, bloqueos). Cooldown en
  ms (default 6000; 900 en n_key/n_sound_locked para permitir reintento humano y
  absorber el doble clic).
- **Anomalías**: `anomalies.pool` con `minT`, `weight`, `cooldown`, `params`;
  programación razón/intervalo variable (`intervalBase/intervalMin`). Sólo con
  narrativa activa.
- **Subliminal**: `Subliminal.flash()` desde efectos; respeta `reducedFlicker`.
- **Log**: `Story.note(txt)` con timestamp; tope 400 líneas.

---

## 6. Contrato de efectos (`effects.json` + `effects.js`)

- Catálogo externo: `{meta, paramTypes, categories[], effects[], triggers[],
  events[], conditions[]}`. Cada efecto: `{id, name, category, impl?, description,
  params[], macro?}`; `params`: `{key, label, type, min?, max?, step?, default?,
  options?}`. El editor genera widgets desde `params` (tipos: number, text, textarea,
  select, check, color, json, list…).
- `macro`: lista de pasos `{effect, params}` que se expande al ejecutar.
- Instancia en nodo: `{effect, params, delay?, enabled?}`. `delay` ms vía setTimeout;
  `enabled:false` se salta.
- Implementación: `define('id', (params, ctx) => …)` en `effects.js`. **ctx**
  garantizado: `{Story, UI, FS, Trash, Desktop, Audio, Config, Events, Clock,
  MenuState, CRTBoost, Blink, Subliminal, Boot, Endgame, AudioOverride,
  reducedFlicker, rebuildMenus}`. Efecto sin implementación = `console.warn` y skip
  (nunca rompe); el validador del editor lo reporta.
- Añadir efecto nuevo = entrada en `effects.json` + (si procede) `define()`; sin
  código, el efecto aparece en el catálogo pero avisa al usarse.

---

## 7. Contrato del SO

### 7.1 Ventanas y diálogos (`ui.js`)
- `newWin({x,y,w,h,title,kind,data,scrollable,fx,fy})`; barra de título 16 px
  (`body(w).y = w.y+16`), filas de carpeta 22 px (primera en `body.y+20`).
- Zoom box: esquina sup._der. del título (`w.x+w.w-17..-4`, `w.y+2..+14`).
- Diálogos = modales: cualquier press fuera de sus partes → `Audio.alert`.
  `UI.draggableDialogs` habilita arrastre por barra (lo activa el Endgame).
- Menús: barra de 20 px; ítems con `_x/_w`; menú abierto en `UI.openMenu._box`
  (`rows[].y`, altura 15). Condicionales: `showIf`, `needsFlag`, `hideIf`,
  `alwaysDisabled`, `effects` por ítem, `action` (`open:PATH`, `dialog:KEY`,
  `close`, `closeAll`, `emptyTrash`, `about`, `graph`, `effect:ID`).

### 7.2 Sistema de archivos (`filesystem.js`)
- Nodos por path absoluto: `{type: folder|doc|sound|app|trash, name, icon?, children[],
  locked?, hidden?, doc?, generated?}`. `unlocked` se setea en runtime.
- Docs: `docs[clave] = {type, cover?, initial?, grow?, corruptLast?, entries[]}`;
  `grow` revela 1 entrada por scroll-al-fondo (`doc:grow`). Envoltura por columnas
  según ancho **actual** del body (invariante 7).
- Sonidos: `sounds[]` con `freq/plays/seconds/meta/desc/locked`; reproducir emite
  `play {id}` (id = índice+1).
- Apps: `apps{oyente, live, about}` declarativas; `registerApps()` las monta;
  botones emiten `app {app, button}`.
- Papelera: `Trash.items` (nombres). Con narrativa, vaciar/soltar **emite** y el grafo
  decide; sin narrativa, soltar mueve el ítem a `Trash.items` (oculta nodo FS y quita
  icono) y vaciar limpia.
- `openDoc` deduplica por título (trae al frente si ya existe, salvo `force`).

### 7.3 Escritorio (`desktop.js`)
- Iconos: fijos (PAPELERA) + `fs.desktop` + añadidos (`addIcon`, p.ej. DISCO 2,
  visitor, ítems devueltos). `layout()` auto-apila los no `keep`.
- Doble clic abre (`maybeDouble` 470 ms); arrastre >4 px activa `UI.drag`.

---

## 8. Contrato de datos (`data/game.json`)

Claves de primer nivel: `meta, runtime, display, audio, clock, ui, desktop, boot,
fs, docs, sounds, apps, menus, dialogs, subliminal, anomalies, ending, story`
(+ `sprites` opcional: override de iconos desde el editor).

- `meta{workingName, version, title}` · `runtime{narrative, effects}`.
- `display{resolution{w,h}, scaleMode, logo, font{size, advance, lineH…},
  palette{…4 estados t/papel/tinta/contraste/lift…}, crt{curvature, scanlines,
  vignette, bloom, roll, flickerBase, flickerTension, noiseMax, glitchAbove,
  glitchAmount, glow}}`.
- `audio{master, layers{sub{freqs,gain,harmonic}, mains, flyback, bed…}}`.
- `clock{base[], frozen, mode, drift}` (deriva con tensión vía nodos clock1/2).
- `ui{closeFail{above,chance,dialogKey,dialogKey2}, hitDrift{above,scale}}`.
- `boot{warn, post{lines}, happy{duration}, post2{…}}`.
- `fs{desktop[], generatedRows?, nodes{PATH: nodo}}`.
- `docs{CLAVE: doc}` · `sounds[]` · `apps{}` · `menus{aboutPath, base[], extra[]}`.
- `dialogs{CLAVE: {title,text,buttons,field?}}` (trashEmpty, closeFail, closeFail2,
  noClose, appInUse, readonly, invitados).
- `subliminal{words[], frames, contrast}`.
- `anomalies{intervalBase, intervalMin, pool[]}`.
- `ending{title,text,buttons,copyLabel,copySub,offLabel}`.
- `story{nodes[], edges[]}` según §5.

**Regla de edición:** toda modificación de contenido se hace vía editor o editando
el JSON y validando (`b-validate`); el juego no tiene fallbacks de contenido: una
ruta/clave inexistente debe ser visible en validación, no silenciosa en runtime.

---

## 9. Contrato del editor

- `Store` (única fuente): `data`, `catalog`, `view{zoom,panX,panY,grid,snap}`,
  `sel{kind,id}`, `dirty`, `fileHandle`; API: `subscribe/notify/touch`,
  `nodes/edges/node/edge`, `select`, `addNode/removeNode/addEdge/removeEdge`,
  `effect/categories/addEffectTo`, `save()` (File System Access → descarga fallback),
  `load`. `validate(Store)` devuelve issues `{level: error|warn|note, …}`.
- `Graph`: `toWorld/toScreen/portPos/hitNode/hitPort/hitEdge`, drag de nodos (snap a
  grid si activo), link puerto-salida→nodo crea arista, clic en arista selecciona,
  `Supr` borra selección, `N` nodo, `F` fit, rueda zoom, shift/medio pan, doble clic
  en vacío crea nodo, doble clic en nodo enfoca etiqueta.
- Inspector: panel de nodo (id, etiqueta, tipo, **repetible+cooldown**, posición,
  disparador con match JSON, condiciones, efectos con picker del catálogo y widgets
  por `params`), panel de arista (desde/hasta + condiciones), render reactivo
  (`renderSoft` no pisa inputs enfocados).
- Pestañas: Grafo / Catálogo / Pantalla (CRT completo + preview en vivo + tensión de
  prueba) / Audio / Datos (JSON crudo).
- Ciclo: **editar → guardar game.json → recargar juego**. El editor no transforma el
  JSON: lo serializa tal cual lo cargó más los cambios.

---

## 10. Contrato de pruebas y depuración

- `window.__VESPER__` (juego): `{Config, FB, CRT, Audio, UI, Desktop, FS, Trash,
  Story, Subliminal, Blink, MenuState, Boot, Endgame, ctx, runEffect, runList,
  winByTitle, frontWin, windowAt, menuAt, body, closeWin, Clock, Input, Events}`.
  Sólo para tests/editor-preview; el juego no lo usa internamente.
- `window.__STORE__` / `window.__GRAPH__` (editor).
- Harness (`/home/user/testharness`, fuera del entregable):
  - `prod.js`: walkthrough completo (aceptación: **0 errores y 0 avisos de consola**,
    todos los nodos/flags esperados, `fase: ending`, `session≥1`).
    Reglas del harness: `settle()` espera al cursor con inercia antes de cada clic;
    `dismissDialogs()` antes de interacciones; `clearAt()` cierra ventanas que tapan
    iconos; papelera/arrastre se prueban **antes** de DISCO 2; menús con
    `Story.enabled=false` temporal (el modal COPIAR congela el escritorio).
  - `ed.js`: interacciones del editor (aceptación: 0 errores).
  - `smoke.js`: carga limpia de ambas páginas.
  - `dbg10.js`: modo `runtime.narrative:false` (SO funcional puro).
- Chrome headless requiere libs de sistema (reinstalar tras reset del sandbox):
  `libnspr4 libnss3 libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm2 libxkbcommon0
  libxcomposite1 libxdamage1 libxfixes3 libxrandr2 libgbm1 libpango-1.0-0 libcairo2
  libatspi2.0-0 libxshmfence1 fonts-liberation libasound2t64`.

---

## 11. Recetas de extensión

- **Nuevo efecto**: entrada en `effects.json` (categoría existente o nueva) +
  `define('id', …)` si requiere código. Probar en editor (widget auto) y en un nodo.
- **Nuevo nodo narrativo**: editor › Grafo (doble clic en vacío) → configurar
  trigger/condiciones/efectos → arista desde su predecesor → validar → guardar.
- **Nueva app**: entrada en `apps` + registro en `apps.js` (patrón declarativo
  existente: botones→eventos `app`), nodo FS `type:'app'`.
- **Nuevo sonido**: entrada en `sounds[]` (síntesis por parámetros) + nodo FS
  `type:'sound'`; `locked:true` si abre con clave.
- **Nueva anomalía**: `anomalies.pool[]` con efecto existente o nuevo + `minT/weight`.
- **Nuevo menú/ítem**: `menus.base/extra` con `showIf/needsFlag/effects/action`.
- **Nuevo final/variante**: nodo `auto` con condiciones + efecto `ending` (o nuevo
  efecto macro) — recordar que `Endgame.begin()` es modal y cierra el escritorio.

---

## 12. Convenciones de código

- ES6 modules, sin build; comentarios y UI en **español**.
- Sin estado global nuevo: módulos exportan singletons; comunicación por `Events` o
  imports directos documentados en §2.
- Nuevos efectos/OS: nunca `console.log` en runtime (sólo `console.warn` para
  configuraciones rotas); los tests cuentan avisos como fallos.
- Coordenadas/tiempos en ms y px lógicos; colores como índice de paleta (0/1) o
  fracción de tinta; hex sólo en `crt.glow` y editor.
- `package.json` con `"type":"module"` para `node --check` de los módulos.

---

## 13. Limitaciones y deuda conocida

1. Editor sin undo/redo ni autosave; `save()` depende de File System Access o
   descarga (en headless se captura el blob).
2. `game.json` único de ~72 KB: cómodo para el contrato "un solo JSON" pero grande;
   si crece, considerar `docs`/`sounds` como refs externas **sin romper** el
   invariante 2 (el editor debería seguir viéndolo como un documento).
3. Sin tests unitarios: sólo E2E (puppeteer). Las anomalías aleatorias obligan al
   harness a defensivas (`dismissDialogs`, `clearAt`, `settle`); cualquier test nuevo
   debe seguir ese patrón.
4. Audio no verificable en headless (sólamente estructura); revisión manual pendiente.
5. El preview del editor reproduce CRT pero no todos los efectos de pantalla.
6. `n_about` y otros nodos de menú quedan sin cubrir cuando el harness pausa la
   narrativa (§10): cubrirlos en una pasada con tensión controlada.
7. Rendimiento en hardware muy bajo sin perfilar (curvatura por filas ya es la
   opción barata; no volver a barrel per-pixel ni feDisplacementMap).

---

## 14. Versionado

SemVer subjetivo-artístico: `0.MINOR.PATCH`. Mientras `meta.version` sea `0.0.x`,
pueden cambiar esquemas **sólo** añadiendo campos con default (merge de defaults en
`Config`/`Store`). Romper un campo existente exige bump de MINOR + migración en
`Store.load` + entrada en CHANGELOG. Historial: ver `CHANGELOG.md`.
