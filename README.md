# story-template: Historias científicas verticales 9:16

Template minimalista para publicar historias científicas en redes sociales (Instagram Stories, TikTok, Pinterest). Verticales, tipográficamente densas, con énfasis en ecuaciones y figuras. Motor: pdfLaTeX + tcbposter.

**Filosofía**: las redes sociales recortan los bordes. Este template respeta una zona segura garantizada (columnas 2-11, filas 3-22) para que el contenido crítico nunca sea cortado. Usa principios editoriales japoneses (Ma, balance asimétrico, whitespace) + paleta grayscale Catppuccin.

---

## Quick Start

### 1. Clonar y preparar
```bash
git clone <repo> && cd story-template
```

### 2. Editar contenido
Abre `story-content.tex` y reemplaza las 11 macros:

```latex
\providecommand{\StoryTitle}{Tu título aquí}
\providecommand{\StoryEquation}{e^{i\pi} + 1 = 0}  % ecuación principal
\providecommand{\StoryFigure}{\includegraphics[width=0.9\linewidth]{figs/chart.pdf}}
\providecommand{\StoryBody}{Párrafo de 1-2 líneas (7.5pt).}
```

### 3. Compilar
```bash
pdflatex main-story.tex
pdflatex main-story.tex  # segunda pasada (TikZ)
```

### 4. Exportar a SVG (opcional, para edición posterior)
```bash
inkscape main-story.pdf -o story-001.svg
```

Output: `main-story.pdf` (13.5cm × 24cm, 300 DPI recomendado).

---

## Anatomía del template

```
story-template/
├── main-story.tex                    ← Root (no edites)
├── story-content.tex                 ← ÚNICO archivo a editar por historia
│
├── config/                           ← Configuración reutilizable
│   ├── packages-story.tex            Paquetes (inconsolata, pgfplots, etc.)
│   ├── geometry-story.tex            Tamaño: 13.5cm × 24cm (9:16)
│   ├── typography-story.tex          Escala: \DisplayNum, \HeroFont, \TitleFont...
│   ├── tcolorbox-story.tex           Estilos: ghost, AccentBar, StoryCat
│   ├── macros.tex                    Matemáticas: \R, \N, \E[...], \argmin
│   ├── equation-styles.tex           Coloreado semántico: \equ, \eqk, \eqop
│   └── pgfplots-config.tex           Gráficos: cycle list, colormaps
│
├── environments/
│   ├── generator.tex                 Factory de ambientes (infraestructura)
│   └── story-blocks.tex              Bloques editoriales: KeyFact, StorySep
│
├── themes/
│   ├── catppuccin-palette.tex        Colores Catppuccin (referencia)
│   └── story-accents.tex             Paleta grayscale: InkBlack/Dark/Mid/Light/Faint
│
├── deco/
│   └── story-deco.tex                Decoraciones TikZ (opcional, deshabilitado)
│
├── code/                             (directorio vacío para snippets)
├── figs/                             (directorio vacío para imágenes)
└── main-story.pdf                    (output compilado)
```

---

## Maquetación: zonas y proporciones

El template divide el espacio en 24 filas × 12 columnas. **Zona segura**: columnas 2-11 (contenido garantizado visible), filas 3-22 (buffers superiores e inferiores para sangrado de redes).

```
┌────────────────────────────────┐
│ [F1-2] METADATOS + ACENTO      │ ← Buffers (pueden sangrar)
├─ ZONA SEGURA COMIENZA ─────────┤
│ [F3-8]   HÉROE ECUACIÓN (6 filas, ~25% área) ┌─ Logo en col 12
│ [F9]     Barra de acento                      │
│ [F10-12] Título (+ número col 12) ────────────┘
│ [F13]    Subtítulo
│ [F14-15] Abstract (borde acentuado)
│ [F16-21] Figura/gráfico (6 filas, ~25% área)
│ [F22]    Body paragraph
├─ ZONA SEGURA TERMINA ──────────┤
│ [F23-24] FOOTER + MARCA        │ ← Buffers (pueden sangrar)
└────────────────────────────────┘
```

**Proporción de contenido**: ecuación héroe y figura ocupan ~25% cada una. El balance es deliberado: dos "pesos visuales" que dialogan en el espacio blanco (Ma).

---

## Filosofía de diseño

### Ma (間): whitespace como estructura

El template hereda principios editoriales japoneses. No hay cajas visibles, bordes gruesos ni fondos coloreados (todo `ghost` style). La jerarquía surge de **escala tipográfica + espacio negativo**. Un lector escanea verticalmente y diferencia secciones por tamaño, no por color.

### Tipografía escalonada

| Comando | Tamaño | Rol | Color |
|---------|--------|-----|-------|
| `\DisplayNum{n}` | 40pt | Número decorativo | InkFaint (gris claro) |
| `\HeroFont` | 16pt | Ecuación principal | — (math mode) |
| `\TitleFont` | 20pt | Título de la historia | InkBlack |
| `\SubFont` | 9pt | Subtítulo | InkMid |
| `\BodyFont` | 7.5pt | Párrafos | InkMid |
| `\CaptFont` | 6.5pt | Captions, labels | InkLight |
| `\MetaFont` | 5pt | Metadatos, footer | InkLight |

La fuente base es **Inconsolata** (monospace) para toda la página. Matemáticas conservan Computer Modern (TeX estándar). Esto da "identidad de código" apropiada para ciencias computacionales.

### Paleta: grayscale + acento

**Grayscale base** (`InkBlack` → `InkFaint`): 5 tonos neutros. Cada historia elige **UN acento Catppuccin** (`StoryAccentColor`) que aparece solo en 3 lugares:
1. Línea divisoria [F2] (AccentBar)
2. Chip de categoría [F2] (StoryCat)
3. Borde izquierdo del abstract [F14-15] (leftaccent)

El acento es un susurro visual, no un grito. Permite que cada historia tenga "identidad de color" sin saturar la paleta.

### Chip flotante sobre línea

La línea [F2] ocupa columnas 1-12. El chip de categoría flota sobre ella, posicionado a la derecha (columnas 9-12). Genera tensión visual: dos elementos en la misma fila, uno tira hacia abajo, el otro flota. Es un detalle tipográfico que dice "esto es diseñado".

---

## Guía de contenido: story-content.tex

El archivo `story-content.tex` define 11 macros. Reemplaza los valores placeholder:

```latex
% Colores y metadatos
\providecommand{\StoryAccentColor}{CtpMauve}   % CtpBlue, CtpMauve, CtpGreen, CtpPeach, CtpRed...
\providecommand{\StoryNumber}{001}
\providecommand{\StoryDate}{2025}
\providecommand{\StorySeriesTag}{QbitLab Stories}
\providecommand{\StoryCategory}{Modelos Generativos}

% Contenido principal
\providecommand{\StoryTitle}{Nombre breve de la historia}
\providecommand{\StorySubtitle}{Frase descriptiva (máx 1 línea)}

% Ecuación héroe (display math, centrada, 16pt)
\providecommand{\StoryEquation}{
  e^{i\pi} + 1 = 0
}
\providecommand{\StoryModelLabel}{Identidad de Euler}  % Label bajo la ecuación

% Figura (pgfplots axis, \includegraphics, o TikZ)
\providecommand{\StoryFigure}{
  \includegraphics[width=0.85\linewidth]{figs/example.pdf}
}
\providecommand{\StoryFigCaption}{Comportamiento de X en función de Y.}

% Abstract (2-3 líneas máximo)
\providecommand{\StoryAbstract}{
  Explicación breve del concepto. Una frase sobre la intuición física,
  una sobre la aplicación. Mantén densidad baja.
}

% Body (1-2 líneas máximo, 7.5pt)
\providecommand{\StoryBody}{
  Párrafo final que ancle la historia. Qué es, por qué importa, cómo se usa.
}

% Referencias (formato libre, 5.5pt)
\providecommand{\StoryRefs}{
  \textbf{Refs}: Author et al. (2023) | doi:10.xxxxx
}
```

### Consejos de contenido

**Ecuación héroe**: debe ser reconocible en 1 segundo. Preferir identidades, conservación, transformación. Evitar derivaciones largas.

**Figura**: 6 filas = espacio comprimido. Si es gráfico, usa `pgfplots` (configurado automáticamente). Si es diagrama, mantén líneas gruesas y colores contrastantes. Las fuentes pequeñas se pierden.

**Abstract + Body**: 5.5 pt es muy pequeño. Menos es más. Cada palabra cuenta.

**Categoría y referencias**: usa anglicismos sin miedo (ML, SDE, PDE, VAE). La audiencia es física/CS.

---

## Colorimetría: cómo cambiar el acento

En `story-content.tex`:
```latex
\providecommand{\StoryAccentColor}{CtpMauve}  % ← cambiar esto
```

Opciones disponibles (del repo Catppuccin):
```
CtpBlue       (azul — ecuaciones, lógica)
CtpMauve      (púrpura — teoría, abstracción)
CtpGreen      (verde — optimización, soluciones)
CtpPeach      (melocotón — aplicaciones, hechos)
CtpRed        (rojo — advertencia, divergencia)
CtpYellow     (amarillo — destacado, atención)
CtpPink       (rosa — creatividad, experimentación)
CtpRosewater  (rosa claro — meta, comentario)
CtpSapphire   (turquesa — física, espacios)
CtpTeal       (teal oscuro — equilibrio)
CtpSky        (celeste — cielo, límites)
CtpLavender   (lavanda — suavidad, continuidad)
```

Elije un color por "significado semántico", no estética. CtpBlue para ecuaciones, CtpGreen para soluciones, CtpRed para divergencias.

---

## Ecuaciones coloreadas (opcional)

Las ecuaciones soportan **coloreado semántico** (archivo `config/equation-styles.tex`). Sistema de 3 ejes:

**Eje 1: Rol sintáctico** (estable)
- `\eqop{...}` — operadores: ∫, ∑, ∇, Ĥ
- `\eqfn{...}` — funciones: f(·), L(θ), σ(·)
- `\eqdm{...}` — diferenciales: dx, dt, dW
- `\eqdom{...}` — dominios: Ω, ℝⁿ, S²

**Eje 2: Estatus epistémico** (lo que se busca vs. se asume dado)
- `\equ{...}` — incógnita/buscado (Peach)
- `\eqk{...}` — conocido/dato (Mauve)
- `\eqcon{...}` — constante universal (Lavender)

**Eje 3: Tipo matemático** (refuerzo sutil)
- `\eqvec{...}` — vectores (Red)
- `\eqten{...}` — tensores/matrices (Rosewater)

Ejemplo:
```latex
\eqop{\int_{\eqdom{\Omega}}} \equ{\nabla u \cdot \nabla v} \eqdm{dx}
= \eqop{\int_{\partial\eqdom{\Omega}}} \equ{u} \eqop{\frac{\partial v}{\partial n}} \eqdm{dS}
```

Para deshabilitarlo (impresión B/N):
```latex
{\eqcolorsfalse ... }  % grupo local sin colores
```

---

## Compilación y exportación

### pdfLaTeX (recomendado)

```bash
pdflatex main-story.tex
pdflatex main-story.tex  # IMPORTANTE: segunda pasada para TikZ remember picture
```

Genera `main-story.pdf` (listo para Instagram, 1080×1920 px aprox.).

### Inkscape → SVG (para post-edición)

```bash
inkscape main-story.pdf -o story-001.svg
```

SVG permite:
- Editar texto en diseño gráfico (Figma, Illustrator)
- Ajustar colores sin recompilar LaTeX
- Exportar a múltiples formatos

### Dimensiones y DPI

El template compilado es **13.5cm × 24cm** en tipografía. Al rasterizar:
- **300 DPI** → 1599 × 2835 px (para imprenta, si es necesario)
- **100 DPI** → 533 × 945 px (web, redes)
- **150 DPI** → 800 × 1417 px (estándar equilibrado)

Instagram Stories nativas requieren **1080 × 1920 px**. Ajusta en tu exportador (Inkscape, ImageMagick, etc.):
```bash
convert main-story.pdf -density 150 -resize 1080x1920 story-001.png
```

---

## Estructura reutilizable: de una historia a muchas

El template es **completamente reutilizable**. La arquitectura permite crear N historias sin duplicar configuración.

### Workflow para historia N+1

1. **Copia los archivos raíz**:
   ```bash
   cp main-story.tex main-story-002.tex
   cp story-content.tex story-content-002.tex
   ```

2. **Edita solo `story-content-002.tex`**:
   ```latex
   \providecommand{\StoryTitle}{Nueva historia}
   % ... resto de variables
   ```

3. **Actualiza `main-story-002.tex`** (1 línea):
   ```latex
   \input{story-content-002.tex}  % cambiar esto
   ```

4. **Compila**:
   ```bash
   pdflatex main-story-002.tex
   pdflatex main-story-002.tex
   ```

### Qué NO se replica

Los directorios `config/`, `environments/`, `themes/` son **compartidos**. Si cambias `config/typography-story.tex`, afecta a TODAS las historias. Esto es intencional: coherencia visual global.

**Cambios globales** (afectan todas las historias):
- Escala tipográfica → edita `config/typography-story.tex`
- Paleta grayscale → edita `themes/story-accents.tex`
- Estilos de caja → edita `config/tcolorbox-story.tex`

**Cambios locales** (una historia):
- Contenido → edita `story-content-00N.tex`
- Acento de color → `\StoryAccentColor` en `story-content-00N.tex`

---

## Personalización avanzada

### Cambiar tipografía base

En `config/packages-story.tex`, línea ~16:
```latex
\renewcommand{\familydefault}{\ttdefault}  % ← Inconsolata
```

Para cambiar a serif (Computer Modern):
```latex
% \renewcommand{\familydefault}{\ttdefault}  % comentar
```

Para otra monospace (TeX Gyre Cursor):
```latex
\usepackage{tgcursor}
\renewcommand{\familydefault}{\ttdefault}
```

### Ajustar escala tipográfica

En `config/typography-story.tex`, edita los valores `\fontsize{}{}``:
```latex
\newcommand{\HeroFont}{\fontsize{16}{20}\selectfont}  % 16pt display
```

Para ecuaciones más grandes (20pt):
```latex
\newcommand{\HeroFont}{\fontsize{20}{24}\selectfont}
```

### Modificar márgenes de zona segura

En `main-story.tex`, busca:
```latex
\posterbox[ghost]{column=2, span=10, row=3, rowspan=6}  % cols 2-11
```

Para zona segura más amplia (cols 2-12):
```latex
\posterbox[ghost]{column=2, span=11, row=3, rowspan=6}
```

**Advertencia**: reducir márgenes aumenta riesgo de sangrado en redes.

### Agregar secciones nuevas

El grid es flexible. Para agregar una sección entre Abstract [F14-15] y Figura [F16-21], usa la fila 16:

```latex
\posterbox[ghostpad]{column=2, span=10, row=16}{%
  \BodyFont Tu contenido aquí
}

% Desplaza la figura:
\posterbox[ghost]{column=2, span=10, row=17, rowspan=6}{%
  % figura
}
```

---

## Limitaciones y notas técnicas

### pdfLaTeX vs. XeLaTeX/LuaLaTeX

El template está optimizado para **pdfLaTeX**. Si usas XeLaTeX:
- La fuente Inconsolata puede compilar más lentamente
- TikZ `remember picture` requiere dos pasadas obligatorias
- Recomendación: mantén pdfLaTeX

### Compatibilidad de paquetes

El template requiere:
- `tcolorbox` (v5.0+) — estilos de cajas
- `pgfplots` (v1.18+) — gráficos
- `inconsolata` — monospace
- `amsmath, amssymb` — matemáticas
- `babel` (spanish) — tipografía española

Si compilas en Overleaf o TeX Live 2021+, todo está incluido.

### TikZ y remember picture

La decoración [F2] (línea flotante + chip) usa TikZ. Por eso **compila DOS veces**:
1. Primera pasada: genera `.aux` con coordenadas
2. Segunda pasada: dibuja superposiciones con coordenadas exactas

Si cambias solo contenido (no estructura), basta una compilación.

### Máximo contenido recomendado

- **Título**: máx 7 palabras (20pt) ← si pasa 2 líneas, reduce
- **Subtítulo**: 1 línea (9pt)
- **Abstract**: 4-5 líneas (7.5pt)
- **Body**: 2-3 líneas (7.5pt)
- **Referencias**: 1 línea (5.5pt)

Exceder estos límites comprime el whitespace, viola el espíritu Ma.

---

## Ejemplos de uso

### Historia 1: Identidad de Euler

```latex
\providecommand{\StoryAccentColor}{CtpBlue}
\providecommand{\StoryTitle}{La identidad más hermosa}
\providecommand{\StoryEquation}{e^{i\pi} + 1 = 0}
\providecommand{\StoryModelLabel}{Identidad de Euler}
\providecommand{\StoryAbstract}{Cinco constantes matemáticas: $e$, $i$, $\pi$, $1$, $0$. 
  Una ecuación. Belleza pura.}
```

### Historia 2: Divergencia en ML

```latex
\providecommand{\StoryAccentColor}{CtpRed}
\providecommand{\StoryTitle}{KL Divergence}
\providecommand{\StoryEquation}{D_{\text{KL}}(p \| q) = \sum_x p(x) \log \frac{p(x)}{q(x)}}
\providecommand{\StoryFigure}{\begin{axis}...\end{axis}}
```

### Historia 3: Ecuación de Schrödinger

```latex
\providecommand{\StoryAccentColor}{CtpSapphire}
\providecommand{\StoryTitle}{Mecánica cuántica}
\providecommand{\StoryEquation}{\eqop{\hat{H}} \equ{\psi} = \equ{E} \equ{\psi}}
```

---

## Licencia y contribuciones

Template derivado de [latex-catppuccin-academic](https://github.com/Zessinthel/latex-catppuccin-academic) (MIT). Usa Catppuccin palette bajo licencia MIT.

Contribuciones bienvenidas. Para cambios mayores, abre un issue primero.

---

## Contacto y soporte

- **Repo**: [github.com/Zessinthel/story-template](https://github.com/Zessinthel/story-template)
- **Issues**: para bugs, solicitudes de features, preguntas de compilación
- **Telegram**: @QbitLab

Disfruta escribiendo historias científicas.