# DESIGN.md — Pulso

Capa visual de Pulso. Todo lo que se ve en `index.html` sale de este documento. Si falta algo, se propone aquí antes de implementarlo (ver `CLAUDE.md`).

## Mood: Tech Brutalist

La interfaz se ve como una herramienta. Las reglas que la definen:

- **Estructura a la vista:** grilla visible, bordes gruesos, esquinas rectas y sombras duras sin desenfoque.
- **Una sola familia tipográfica, monoespaciada:** la jerarquía sale del tamaño, el peso y las mayúsculas, no de cambiar de fuente.
- **Escala de grises:** el color aparece solo cuando algo tiene un estado que comunicar.
- **Nada decorativo:** sin degradados, ilustraciones, íconos de adorno ni transparencias.
- **Densidad útil:** el equipo comercial ve mucha información a la vez, pero con aire suficiente (múltiplos de 4) para leerla.

---

## 1. Tokens

Los nombres de token están en español y se usan como variables CSS (`--fondo`, `--espacio-4`, etc.). Todos los colores van en OKLCH (`oklch(L C H)`) y quedan dentro de la gama sRGB.

### 1.1 Color

#### Neutros

| Token | Uso | Oscuro | Claro |
|---|---|---|---|
| `--fondo` | Fondo de la app | `oklch(0.15 0 0)` | `oklch(0.97 0 0)` |
| `--superficie` | Tarjetas, tabla, ficha, paleta | `oklch(0.20 0 0)` | `oklch(1 0 0)` |
| `--superficie-alta` | Hover de fila, cabecera de tabla, ítem activo | `oklch(0.25 0 0)` | `oklch(0.93 0 0)` |
| `--texto` | Texto principal y cifras | `oklch(0.96 0 0)` | `oklch(0.15 0 0)` |
| `--texto-tenue` | Etiquetas, metadatos, texto secundario | `oklch(0.74 0 0)` | `oklch(0.44 0 0)` |
| `--borde` | Bordes estructurales (2 px) y sombras duras | `oklch(0.96 0 0)` | `oklch(0.15 0 0)` |
| `--borde-suave` | Divisores internos (1 px): filas de tabla, separadores | `oklch(0.56 0 0)` | `oklch(0.58 0 0)` |

#### Acento (estado de interacción)

El acento marca solo **foco, selección y la acción primaria**. Nunca se usa para decorar.

| Token | Uso | Oscuro | Claro |
|---|---|---|---|
| `--acento` | Relleno: botón primario, ítem seleccionado en ⌘K | `oklch(0.92 0.20 120)` | `oklch(0.92 0.20 120)` |
| `--acento-tinta` | Texto sobre `--acento` | `oklch(0.15 0 0)` | `oklch(0.15 0 0)` |
| `--acento-linea` | Anillo de foco, subrayado de pestaña activa | `oklch(0.92 0.20 120)` | `oklch(0.50 0.14 125)` |

En tema claro, el relleno lima siempre lleva un borde de 2 px en `--borde`, porque contra el fondo claro no se distingue solo.

#### Estados comerciales

Cada estado tiene tres variantes:

- **Línea:** texto, ícono o barra sobre `--fondo` o `--superficie`.
- **Relleno:** fondo de chip o de marca.
- **Tinta:** texto sobre el relleno.

| Estado | Token | Oscuro | Claro |
|---|---|---|---|
| **Ganada** | `--ganada` (línea) | `oklch(0.80 0.18 150)` | `oklch(0.50 0.13 150)` |
| | `--ganada-relleno` | `oklch(0.80 0.18 150)` | `oklch(0.80 0.18 150)` |
| **En riesgo** | `--riesgo` (línea) | `oklch(0.85 0.16 85)` | `oklch(0.52 0.11 70)` |
| | `--riesgo-relleno` | `oklch(0.85 0.16 85)` | `oklch(0.85 0.16 85)` |
| **Perdida** | `--perdida` (línea) | `oklch(0.72 0.17 25)` | `oklch(0.53 0.19 27)` |
| | `--perdida-relleno` | `oklch(0.72 0.17 25)` | `oklch(0.72 0.17 25)` |
| Todas | `--estado-tinta` | `oklch(0.15 0 0)` | `oklch(0.15 0 0)` |

**Contraste verificado (WCAG 2.x):**

- Todo el texto supera 4,5:1 sobre `--fondo`, `--superficie` y `--superficie-alta`, en ambos temas.
- `--estado-tinta` sobre cualquier relleno supera 7:1.
- `--borde-suave` y `--acento-linea` superan 3:1, el mínimo para elementos no textuales.
- El peor caso es `--riesgo` claro sobre `--superficie-alta`, con 4,59:1. No se debe oscurecer más esa superficie.

**Tema:** por defecto se sigue `prefers-color-scheme`, y el usuario puede fijarlo en Ajustes (se guarda en `localStorage`). El tema oscuro es el de referencia.

### 1.2 Tipografía

- **Familia única, `--fuente`:** `ui-monospace, "SF Mono", "Cascadia Mono", "Segoe UI Mono", Menlo, Consolas, "Liberation Mono", monospace`. Es una pila del sistema, sin fuentes externas.
- **Cifras:** `font-variant-numeric: tabular-nums slashed-zero` en toda la app. La monoespaciada ya alinea, pero se declara igual por si el sistema cae en una fuente proporcional.
- **Pesos:** solo 400 (regular) y 700 (negrita).

| Token | Tamaño / interlineado | Peso | Uso |
|---|---|---|---|
| `--texto-metrica` | 48 / 48 px | 700 | La métrica principal (una por vista) |
| `--texto-titulo` | 28 / 32 px | 700 | Título de vista y nombre en la ficha |
| `--texto-subtitulo` | 20 / 24 px | 700 | Encabezados de sección y columnas del pipeline |
| `--texto-cuerpo` | 14 / 20 px | 400 | Texto general, celdas de tabla y campos |
| `--texto-etiqueta` | 12 / 16 px | 700 | Etiquetas en MAYÚSCULAS con `letter-spacing: 0.06em`; chips y cabeceras de tabla |
| `--texto-micro` | 12 / 16 px | 400 | Metadatos: fechas, responsable, días sin actividad |

Ningún texto baja de 12 px. No se usa cursiva; el énfasis se hace con negrita o con mayúsculas.

### 1.3 Espaciado (múltiplos de 4)

| Token | Valor | Uso típico |
|---|---|---|
| `--espacio-1` | 4 px | Separación ícono–texto y dentro de un chip |
| `--espacio-2` | 8 px | Padding vertical de botones y celdas |
| `--espacio-3` | 12 px | Padding de chip horizontal y entre campos |
| `--espacio-4` | 16 px | Padding de tarjeta y gutter de grilla |
| `--espacio-6` | 24 px | Entre bloques dentro de la ficha |
| `--espacio-8` | 32 px | Entre secciones de una vista |
| `--espacio-12` | 48 px | Margen superior de la vista y aire alrededor de la métrica |
| `--espacio-16` | 64 px | Solo en vacíos (estados sin datos) |

Grilla de 12 columnas con gutter de 16 px. Todos los altos de control son múltiplos de 4: 32 px (compacto) y 40 px (normal).

### 1.4 Radios

| Token | Valor |
|---|---|
| `--radio` | `0` |

Todo es recto. No hay excepciones; un elemento redondeado rompe el mood.

### 1.5 Bordes

| Token | Valor | Uso |
|---|---|---|
| `--borde-grueso` | `2px solid var(--borde)` | Contorno de tarjetas, botones, tabla, ficha y paleta |
| `--borde-fino` | `1px solid var(--borde-suave)` | Divisores internos |
| `--borde-estado` | `4px solid var(--<estado>)` | Barra izquierda de la tarjeta según su estado |

### 1.6 Sombras

Son sombras duras: desplazamiento sin desenfoque, en el color de `--borde`.

| Token | Valor | Uso |
|---|---|---|
| `--sombra-0` | `none` | Elementos planos y botón presionado |
| `--sombra-1` | `4px 4px 0 0 var(--borde)` | Tarjeta, botón y chip interactivo en reposo |
| `--sombra-2` | `6px 6px 0 0 var(--borde)` | Hover de tarjeta y tarjeta arrastrada |
| `--sombra-3` | `8px 8px 0 0 var(--borde)` | Elementos flotantes: ficha y paleta ⌘K |

### 1.7 Movimiento

| Token | Valor | Uso |
|---|---|---|
| `--duracion-instantanea` | `80ms` | Hover, presionado y cambio de sombra |
| `--duracion-rapida` | `120ms` | Apertura de la paleta ⌘K y de menús |
| `--duracion-panel` | `200ms` | Entrada de la ficha lateral |
| `--curva` | `cubic-bezier(0.2, 0, 0, 1)` | Todas las transiciones |

El movimiento es corto y seco: se desplaza (`translate`) y cambia la sombra. No hay rebotes, escalados ni desvanecidos largos.

---

## 2. Reglas

1. **El color solo comunica estado.**
   - Hay dos tipos de estado:
     - **Comercial:** ganada, en riesgo o perdida.
     - **De interacción:** foco, selección o acción primaria, que se marcan con el acento.
   - Todo lo demás va en neutros: la marca, los títulos, las etapas intermedias y los gráficos sin estado.
   - El color nunca va solo. Siempre lo acompaña un texto («GANADA») o un símbolo (`✓`, `!`, `✕`), para que se entienda sin distinguir colores.
2. **Una métrica principal arriba a la izquierda.**
   - Cada vista tiene una sola cifra protagonista, en `--texto-metrica` y en la esquina superior izquierda del área de contenido, con su etiqueta encima en `--texto-etiqueta`.
   - En el pipeline es **PIPELINE PONDERADO** (suma de monto × probabilidad, en CLP).
   - Las métricas secundarias van a su derecha en `--texto-subtitulo`, como máximo tres.
3. **Las cifras usan números tabulares.**
   - Todos los montos, fechas, porcentajes y conteos se alinean con `tabular-nums`.
   - En tablas, los montos van alineados a la derecha.
   - Los montos se escriben en CLP según `CLAUDE.md`: `$1.250.000`. Los montos abreviados (`$12,5 M`) solo se permiten en tarjetas, y llevan el monto completo en el `title`.
4. **El contraste cumple AA.**
   - Texto: 4,5:1 como mínimo.
   - Texto de 24 px o más, o de 19 px en negrita: 3:1 como mínimo.
   - Bordes de controles, foco e íconos: 3:1 como mínimo.
   - Cualquier token nuevo se verifica en ambos temas antes de agregarlo.
5. **Se respeta «reducir movimiento».**
   - Con `prefers-reduced-motion: reduce`, todas las duraciones pasan a `0ms`.
   - La ficha y la paleta aparecen sin desplazarse.
   - Al arrastrar tarjetas, la tarjeta no se inclina ni tiene inercia.
6. **El foco siempre se ve.**
   - Contorno de 3 px en `--acento-linea` con 2 px de separación (`outline-offset`).
   - Nunca se usa `outline: none` sin un reemplazo equivalente.
7. **La app se puede usar entera con teclado.** Todo lo que se hace con el mouse tiene un camino con teclado, y la paleta ⌘K es el atajo universal.

---

## 3. Componentes

### 3.1 Botón

| Variante | Fondo | Texto | Borde | Sombra |
|---|---|---|---|---|
| Primario (uno por vista) | `--acento` | `--acento-tinta` | `--borde-grueso` | `--sombra-1` |
| Secundario | `--superficie` | `--texto` | `--borde-grueso` | `--sombra-1` |
| Fantasma | transparente | `--texto` | ninguno; subrayado en hover | `--sombra-0` |
| Peligro | `--superficie` | `--perdida` | `2px solid var(--perdida)` | `--sombra-1` |

**Medidas y texto**

- Alto de 40 px (32 px en la versión compacta) y padding horizontal de `--espacio-4`.
- Texto en `--texto-etiqueta`, en MAYÚSCULAS. Si lleva ícono, va a la izquierda con `--espacio-2` de separación.

**Estados**

- **Hover:** el botón sube con `translate(-2px, -2px)` y la sombra pasa a `--sombra-2`.
- **Presionado:** el botón baja con `translate(4px, 4px)` y queda con `--sombra-0`, como si se hundiera.
- **Deshabilitado:**
  - El texto pasa a `--texto-tenue` y el borde a `--borde-suave`.
  - No tiene sombra ni cursor de mano.
- **Atajo:** si la acción tiene uno, se muestra a la derecha en `--texto-micro` (p. ej. `N`).

### 3.2 Chip de etapa

Las etapas del embudo son PROSPECTO → CALIFICADA → PROPUESTA → NEGOCIACIÓN → GANADA / PERDIDA.

**Medidas y texto**

- Alto de 24 px y padding de `0 var(--espacio-3)`.
- Texto en `--texto-etiqueta`, en MAYÚSCULAS, con `--borde-grueso`.

**Etapas abiertas** (de Prospecto a Negociación)

- Fondo `--superficie` y texto `--texto`.
- A la izquierda llevan el número de etapa en `--texto-tenue`: `02 CALIFICADA`.

**Cierres**

- **Ganada:** fondo `--ganada-relleno`, texto `--estado-tinta` y prefijo `✓`.
- **Perdida:** fondo `--perdida-relleno`, texto `--estado-tinta` y prefijo `✕`.

**«En riesgo»**

No es una etapa: es una marca que se suma a una etapa abierta. Va como un segundo chip pegado al de la etapa, con fondo `--riesgo-relleno`, texto `--estado-tinta` y el texto `! RIESGO`.

Si el chip es interactivo (sirve para cambiar de etapa), lleva `--sombra-1` y el mismo hover que el botón. Si es informativo, no tiene sombra.

### 3.3 Tarjeta del pipeline

El pipeline se organiza en columnas, una por etapa. El encabezado de cada columna va en `--texto-subtitulo` e indica:

- el nombre de la etapa;
- debajo, la cantidad de oportunidades y la suma en CLP, en `--texto-micro` y `--texto-tenue`.

**Estructura de la tarjeta**, de arriba hacia abajo y con padding `--espacio-4`:

1. **Cliente** en `--texto-cuerpo` y negrita; corta con «…» en una línea.
2. **Oportunidad** en `--texto-cuerpo`, con un máximo de dos líneas.
3. **Monto** en `--texto-subtitulo`, alineado a la izquierda, junto a la probabilidad (`60 %`) en `--texto-micro`.
4. **Pie** con `--borde-fino` arriba: tipo (CAPACITACIÓN o PROYECTO, en `--texto-etiqueta` y `--texto-tenue`), iniciales del responsable y fecha estimada de cierre (`dd-mm-aaaa`).

**Contorno**

- `--borde-grueso`, fondo `--superficie`, `--radio` y `--sombra-1`.
- Si la oportunidad está en riesgo, ganada o perdida, lleva además `--borde-estado` a la izquierda con el color correspondiente. Sin estado, no tiene barra.
- Si está en riesgo, el chip `! RIESGO` aparece arriba a la derecha.

**Interacción**

- **Hover:** se aplica `--sombra-2` y la tarjeta sube 2 px.
- **Clic o `Enter`:** abre la ficha.
- **Arrastrar:** mientras se arrastra, la tarjeta queda con `--sombra-2` y la columna de destino se marca con un borde punteado de 2 px en `--acento-linea`.
- **Sin mouse:** con la tarjeta enfocada, `Alt` + `←`/`→` la mueve de etapa.

### 3.4 Tabla

**Contorno**

- `--borde-grueso` alrededor y fondo `--superficie`.
- No tiene sombra: es contenido, no es un objeto flotante.

**Cabecera**

- Fondo `--superficie-alta` y texto en `--texto-etiqueta`, en MAYÚSCULAS.
- Queda fija al hacer scroll.
- Si la columna se puede ordenar, muestra `↑` o `↓` junto al nombre.

**Filas**

- Alto de 40 px, `--borde-fino` entre filas y padding de celda `--espacio-2 --espacio-3`.
- Texto en `--texto-cuerpo`.
- Hover en `--superficie-alta`.
- **Fila seleccionada:**
  - Barra izquierda de 4 px en `--acento-linea`.
  - Fondo `--superficie-alta`.

**Columnas**

- **Montos y porcentajes:** alineados a la derecha.
- **Fechas:** en `dd-mm-aaaa`, alineadas a la derecha.
- **Texto:** alineado a la izquierda.
- **Estado:** usa el chip de etapa.

**Total**

- Última fila fija con `--borde-grueso` arriba y negrita.

**Vacío**

- Un mensaje en `--texto-tenue`, centrado, con `--espacio-16` de aire y un botón secundario para crear la primera oportunidad.

### 3.5 Ficha

Es el detalle de una oportunidad. Se abre como panel lateral derecho de 480 px que ocupa todo el alto. En pantallas de menos de 768 px ocupa todo el ancho.

**Contorno y entrada**

- `--borde-grueso` a la izquierda, fondo `--superficie` y `--sombra-3`.
- Entra desde la derecha en `--duracion-panel`.

**Estructura**, con padding `--espacio-6`:

1. **Cabecera:**
   - Nombre de la oportunidad en `--texto-titulo`.
   - Debajo van el cliente y el RUT en `--texto-tenue`.
   - Botón fantasma `✕ CERRAR` (`Esc`) arriba a la derecha.
2. **Cifra principal:**
   - Monto en `--texto-metrica`.
   - Debajo, la probabilidad y el monto ponderado.
   - Los chips de etapa y estado van al lado.
3. **Datos** en una grilla de dos columnas: etiqueta en `--texto-etiqueta` y `--texto-tenue`, y valor en `--texto-cuerpo`. Los campos son: tipo, responsable, región, contacto, fecha estimada de cierre y última actividad.
4. **Actividad:** lista cronológica, la más reciente arriba, separada con `--borde-fino`. Cada entrada lleva la fecha en `--texto-micro` y el texto.
5. **Acciones:** fijas abajo, con `--borde-grueso` arriba. Incluyen el botón primario para registrar una actividad, «Marcar ganada» y «Marcar perdida».

**Edición**

- Los campos se editan en línea, con un contorno de foco en `--acento-linea`.
- Los cambios se guardan al salir del campo y aparece «GUARDADO» en `--texto-micro` durante 2 segundos. Si el usuario pidió reducir movimiento, no se anima.

### 3.6 Paleta ⌘K

Es el buscador y lanzador universal. Se abre con `⌘K` en Mac y `Ctrl+K` en Windows y Linux.

**Capa de fondo**

- Cubre la app con `--fondo` al 80 % de opacidad.
- No tiene desenfoque, para mantener el mood.

**Contenedor**

- 640 px de ancho, a 96 px del borde superior y centrado.
- `--borde-grueso`, fondo `--superficie`, `--sombra-3` y `--radio`.
- Se abre en `--duracion-rapida`.

**Campo de búsqueda**

- 48 px de alto, texto en `--texto-subtitulo` y `--borde-grueso` abajo.
- Lleva el prefijo `>` en `--texto-tenue`.
- Placeholder: «Busca una oportunidad, cliente o acción…».

**Resultados**

- Se agrupan bajo encabezados en `--texto-etiqueta` y `--texto-tenue`: OPORTUNIDADES, CLIENTES y ACCIONES.
- Cada ítem mide 40 px y muestra el nombre, el monto o la etapa a la derecha en `--texto-micro`, y el atajo si existe.

**Ítem activo**

- Fondo `--acento` y texto `--acento-tinta`. Es el único lugar, además del botón primario, donde el acento se usa como relleno.

**Teclado**

- `↑`/`↓` mueven la selección, `Enter` ejecuta y `Esc` cierra.
- Al cerrar, el foco vuelve al elemento que lo tenía antes.

**Sin resultados**

- Muestra «Sin resultados para "…"» y una acción «Crear oportunidad "…"».
