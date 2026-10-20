# PCI · Comité — prototipo HTML (6 slides)

Presentación navegable de **PCI (Price Competitiveness Index)** construida sobre el deck **HIGH-BEAM** original.
Recorrido: **From data to decision → The data behind PCI → The PCI platform**.

## Cómo abrirla

Necesita servirse por HTTP (no abrir el archivo con doble clic), o abrir la versión publicada en GitHub Pages:

```bash
python -m http.server 8131 --bind 127.0.0.1 --directory pci-committee
```

Después, abrir `http://127.0.0.1:8131/` en Chrome, Edge o Safari.

- `→`, `Espacio`, `Enter` o clic: avanzar · `←`: retroceder · `F`: pantalla completa · `1`–`3`: ir a una slide.
- Enlaces directos a un paso, sin pasos automáticos: `#2.1` (iceberg revelado), `#3.2` … `#3.8` (plataforma, de Executive Summary a Full Detail).
- La tipografía (Manrope) se carga de Google Fonts, igual que en HIGH-BEAM: en la sala hace falta conexión a internet.

## Estructura

| Slide | Contenido | Pasos |
|---|---|---|
| 00 · Intro | Portada original «From data to decision» | 1 |
| 01 · Pricing Data | Iceberg original: punta (lo visible de PCI) → iceberg completo con 4 capas de datos | 2 (clic para revelar) |
| 02 · PCI Platform | Logo PCI (reveal de la suite) → titular → tablet que recorre las 7 vistas reales (Executive Summary, Group Footprint, Powertrain Mix, Pricing Architecture, Affordability, Market Share, Full Detail). Con Full Detail llegan el recorrido Markets → Price y la frase final. Clic en una vista de la lista lateral = saltar a su captura | 9 (los 2 primeros avanzan solos) |
| 03 · Use cases (`#4`) | 8 tarjetas 4 × 2 con la imagen de cada caso: chip de categoría arriba a la izquierda, título y frase. Entrada escalonada 01→08 | 1 |

| 04 · Business applications (`#5`) | Sistema orbital: el motor PCI (logo) sobre el horizonte del iceberg; 3 semicírculos concéntricos (Core pricing · Decision tools · Extended intelligence) con 3/3/2 aplicaciones; 8 rótulos laterales con título y frase, unidos a su nodo por líneas finas | 1 (entrada de ~2,3 s, de dentro hacia fuera) |

| 05 · Ending (`#6`) | Cierre en círculo con la portada: sus líneas convergen desde los laterales y desde arriba en el logo PCI. El logo se revela con el barrido del wordmark de la suite y lanza un pulso único. Después aparecen la firma «Price Competitiveness Index · by Astara Intelligence» y el eco «From data *to decision.*». Fondo: haces del reveal de HighBeam | 1 (unos 3,5 s, termina quieta) |

### Business applications · cómo está hecho

- **Referencia:** el mapa Signals de la web Astara Intelligence (componente `signalsmap/CircularMap.tsx` + `signalsmap.css` del fuente de la web). Se ha reimplementado solo su mecánica, con clases `pci-`:
  - arcos que se trazan (`smap-draw` → `pci-draw`);
  - rótulo de familia sobre el arco con `textPath`;
  - nodos punto + anillo que brotan con rebote (`smap-pop` → `pci-pop`);
  - foco que atenúa el resto.
  No se han copiado sus datos, estados de madurez, filtros, zoom ni línea temporal.
- **Geometría:** se calcula en el script a partir de las listas `RINGS` y `APPS`, al principio de `tools/build_pci_deck.py`.
  - Escenario de 1680 × 640, con radios de 200, 350 y 500 desde el motor.
  - Los ángulos de los nodos están elegidos para que ninguna línea cruce otra ni pise los rótulos de familia.
  - Las líneas salen de cada nodo en dirección radial y luego en horizontal hasta su rótulo.
- **Entrada:** horizonte → logo PCI (con un pulso, una sola vez) → anillo 1 y sus nodos → anillo 2 → anillo 3 → rótulos. No hay rotación ni bucles. Con movimiento reducido se ve directamente el estado final.
- **Foco:**
  - pasar el ratón o el Tab por una aplicación la ilumina, junto con su anillo, y atenúa el resto;
  - clic, Enter o Espacio fija el foco;
  - un segundo clic o Escape lo limpia.
  Las flechas y los controles de la presentación siguen funcionando. Los 8 nombres y sus frases están siempre visibles, sin depender del hover.
- **Anillo Extended intelligence:** es discontinuo para diferenciarlo visualmente. No indica disponibilidad: el copy de la slide dice «can power».

### Revisión UX/UI de movimiento (aplicada)

Las piezas originales de HIGH-BEAM (portada, iceberg y reveal) no se han tocado. Los cambios son solo en las slides nuevas:

- **Platform · cambio de vista:**
  - Antes, un fundido de 0,9 s entre dos interfaces claras que se emborronaban.
  - Ahora la nueva pantalla entra rápida desde la derecha (0,38 s de opacidad y 0,6 s de desplazamiento), como al navegar en la propia herramienta.
- **Platform · lista de vistas:**
  - Antes, puntos que se encendían y apagaban.
  - Ahora sigue el lenguaje de línea + nodo del deck (capas del iceberg, recorrido Markets → Price, nodos del orbital): un raíl fino con una parada por vista. El tramo recorrido y las paradas ya pasadas se encienden. Un nodo luminoso se desliza a la vista activa y una línea corta lo une al nombre.
  - Sustituye a la caja redondeada con barra lateral, que era un patrón genérico de dashboard.
- **Use cases · entrada:**
  - Antes, 8 imágenes hacían zoom a la vez (1.18 en 2,2 s), justo lo que el brief pedía evitar.
  - Ahora entran en ola diagonal (fila + columna, 0,07 s) con un zoom contenido (1.07 en 1,6 s).
  - Al pasar el ratón, la imagen se acerca un poco, como en los módulos de la suite.
- **Business applications · coreografía:**
  - Cada anillo se traza con curva de aceleración y frenado.
  - Cada nodo se enciende justo cuando el trazo pasa por él, con un destello único. El script resuelve la curva para calcular cada retardo.
  - Después la línea viaja hacia fuera, aparece un punto en su extremo y el rótulo llega deslizándose en la dirección de la línea.
  - En total, unos 2,4 s.
- **Business applications · foco:** se ilumina el tramo del anillo junto a la aplicación (±14°), además de su familia.
- **Business applications · jerarquía:** los rótulos de familia pasan de 13 a 15 px, el tamaño de Signals.
- **Business applications · legibilidad a 1366:** el texto tiene un mínimo de 11 px y se han reajustado los ángulos de cuatro nodos, de modo que entre rótulos quedan al menos 15 px.
- **Pulsos:** el del motor y el de los nodos ya no se ven antes de dispararse; antes se veía el primer fotograma durante la espera.

### Use cases · imágenes

Las 8 imágenes salen de `MATERIALES/imagenes slide 3/`. El script las convierte a WebP de 1200 px en `assets/use-cases/01…08.webp`. La asignación se hizo por contenido, porque los archivos no seguían el orden de los casos:

| Caso | Imagen | | Caso | Imagen |
|---|---|---|---|---|
| 01 Benchmarking | `ima (8)` | | 05 Promotions | `ima (4)` |
| 02 Price Evolution | `ima (7)` | | 06 Fleet & TCO | `ima (3)` |
| 03 Strategy & Positioning | `ima (6)` | | 07 Opportunities | `ima (2)` |
| 04 Portfolio & Trim | `ima (5)` | | 08 Residual Value | `ima (1)` |

Cada imagen va como `background-image` de `.pci-card-visual`, por debajo del degradado de lectura (CSS, `.pci-card-surface::after`) y de los textos.
- Se amplía un 15 % (`--img-z`) para poder subir el coche por encima del texto. El encuadre vertical de cada tarjeta se ajusta en la lista `USE_CASES` del script (`--img-pos`).
- Para cambiar una imagen, basta con sustituir el PNG en `MATERIALES/imagenes slide 3/` y regenerar.

## De dónde sale cada pieza (fuente: `index.html` del repositorio HIGH-BEAM, commit `f23aeb3`, idéntico a la web publicada)

| Pieza reutilizada | Origen en HIGH-BEAM |
|---|---|
| Tokens, tipografía, fondos, aurora, grano, `--u` 16:9, transiciones | `<head>` / `:root` completo, copiado sin cambios |
| Portada | escena `.s01` (slide 01 publicada), sin cambios |
| Iceberg | escena `.s02` (slide 02 publicada): misma imagen `iceberg.webp`, SVG de anotaciones, `tag-o`, `tag-l`, `mla`, `close`, `data-lock` |
| Reveal + titular + recorrido + frase final | escena `.s05` (slide 05 publicada): `.wm`, `.sweep`, `.streaks.img`, `.eco`, `.journey`, `.truth`, `data-auto` |
| Tablet de producto | página Pricing Intelligence `.s-pi` (slide 10 publicada): `.hdev`, `.tab.ls`, `.scr.ls`, esquinas `.br`, `.scan`, inclinación al pasar el ratón |
| Navegación, contador, barra de progreso, teclado, deep links | `<nav class="nav">` + motor de escenas del `<script>` |

## Diferencias respecto a HIGH-BEAM

- 6 escenas. Nav: `Intro · Pricing Data · Platform · Use Cases · Applications` (el cierre no lleva chip propio; se llega con → o con la tecla `6`/`Fin`), contador `01 / 06`. Sin la barra de productos HighBeam.
- Use cases es una escena nueva, hecha con piezas del sistema: fondo `highbeam-01` atenuado (el de las páginas de producto), aristas de cristal `--edge`, degradado inferior de los módulos de la suite (`.dim::after`), línea de acento al pasar el ratón y animación `.a`. Las imágenes entran con el mismo zoom suave que las de los módulos de la suite (`imgIn`). Sin iconos ni botones.
- Iceberg: textos de PCI. 3 resultados visibles en lugar de 5 y 4 capas sumergidas en lugar de 8 (líneas recalculadas sobre las mismas profundidades del iceberg). Titular `The insight you see. / The data behind it.`: se cambió «intelligence» por «insight» para que quepa en 4 líneas (con «intelligence» eran 5).
- Logotipo PCI (`MATERIALES/logotipo PCI.svg`, copiado sin cambios a `assets/brand/pci-logo.svg`): sustituye al wordmark de texto en el reveal de la slide 02, con el mismo barrido de luz, y al subir queda reducido sobre el titular. También es el icono de la pestaña del navegador.
- Slide 02: el bloque de 5 tarjetas de la suite se sustituye por la tablet de `.s-pi` con capturas reales de PCI y una columna con las 7 vistas del producto (se ilumina la que está en pantalla). El kicker `02 / PCI PLATFORM` del brief no se ha añadido: ese papel lo hace el wordmark «PCI».
- Capturas: las 7 de `MATERIALES/` (`01-exe.jpg` … `07-fu.jpg`, 1920×1080), convertidas a WebP por el script, sin retoques ni recortes. La tablet tiene la proporción 16:9 de las capturas.
- Motor: el original marca los pasos hasta `st-4`; aquí llega hasta `st-8` (la slide 02 tiene 9 pasos). El recorrido y la frase final, que en la `.s05` original salen en el paso 4, se retrasan al último paso.
- CSS: un bloque añadido al final («PCI · COMITÉ»), sin tokens nuevos. Cambios de JS: (1) avanzar durante un paso automático de la última escena adelanta el paso, en lugar de saltar a una escena que no existe; (2) se quitan las referencias a la navegación de HighBeam.
- Las reglas CSS de las escenas no usadas (Impact, Solutions, productos) siguen en el archivo sin efecto, para no tocar el CSS original.

## Regenerar

`index.html` se genera desde el HIGH-BEAM original con `..\tools\build_pci_deck.py`. Los cambios de contenido van en ese script; si se edita `index.html` a mano, el script los sobrescribe al regenerar.

```bash
python tools/build_pci_deck.py
```

## Verificación

`verification/`: capturas 1920×1080 (Chrome headless) de cada slide y de sus estados clave.
Las de la slide 02 se hicieron con movimiento reducido. En `01b`, el antetítulo «01 / PRICING DATA» sale más pequeño de lo real: es un efecto del modo headless (en el navegador mide 25,5 px a escala 0,46).
