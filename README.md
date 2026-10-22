# Interacciones de los Rayos X en Radiodiagnóstico Convencional

[![Licencia: CC BY-NC-SA 4.0](https://img.shields.io/badge/Licencia-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)

Web interactiva e isométrica para entender cómo interaccionan los rayos X con la materia en el rango de la radiología convencional (20–150 kVp). Está pensada para estudiantes de Imagen para el Diagnóstico (TSID / IDMN).

Es un único archivo HTML autocontenido: no necesita instalación, servidor ni compilación.

## Qué muestra

La escena isométrica, que se puede rotar, incluye:

- **Tubo de rayos X** que dispara un haz cuyo color y opacidad dependen de los parámetros de exposición.
- **Bloque de tejido biológico** con un **átomo magnificado** (núcleo y capas K, L y M con sus electrones en órbita).
- **Receptor digital** (chasis virtual) con la imagen radiográfica resultante de un fantoma sencillo (tejido blando, hueso y un nódulo de bajo contraste).

### Tres estados de interacción

| Estado | Qué se anima | Resultado en el receptor |
|---|---|---|
| **Efecto Fotoeléctrico** (baja energía, alto Z) | El fotón es absorbido por completo por un electrón de la capa K, que sale como electrón expulsado. Queda una vacante en la capa K. | Zona **blanca** (radiopaca) |
| **Efecto Compton** (energía media-alta) | El fotón choca con un electrón de la capa externa. El electrón sale y el fotón disperso continúa con menos energía y otra dirección. | Zona **gris** (niebla base), solo si el fotón disperso llega al receptor |
| **Transmisión Directa** (sin interacción) | El fotón atraviesa el átomo sin atenuarse. | Zona **negra** (radiolúcida) |

En el efecto Compton, el ángulo de dispersión (0–180°) es distinto en cada disparo. Cada vez se muestran el ángulo θ, la energía que pierde el fotón y su energía final E′. Si el fotón disperso llega al receptor, oscurece un punto distinto del esperado, que queda marcado con un recuadro discontinuo.

### Panel de control

- **kVp** (20–150), **mA** (10–800) y **tiempo** (0,005–2 s), con la carga en mAs y la energía media del haz.
- **Indicador de exposición**: subexposición, exposición adecuada o sobreexposición (quemado), con la densidad media y el contraste de la imagen (diferencia de señal entre hueso y tejido blando en el receptor: baja al subir el kVp, sube con la rejilla y no depende de los mAs).
- **Rejilla antidifusora** (5:1, 8:1, 12:1 o 16:1): bloquea los fotones dispersos cuyo ángulo supera el de aceptación de la rejilla, reduce la niebla de la imagen y aplica el factor de Bucky, por lo que muestra los mAs necesarios y la dosis relativa.
- **Probabilidad de interacción** (fotoeléctrico, Compton y transmisión) para tejido blando o hueso, con un botón de información «i».
- **Vista frontal del receptor** ampliada.
- Botones **Pausar / Reanudar**, **Disparo manual**, **Repetir disparo automáticamente** y **Reiniciar parámetros de exposición**.

### Controles de la escena

- **Arrastrar** con el ratón: rota la cámara de 0° a 90° en horizontal y en vertical.
- **Doble clic**: restablece la vista.
- En pantalla táctil, arrastrar rota solo en horizontal para no bloquear el desplazamiento de la página.

## Cómo usarla

### En local

1. Descarga el archivo HTML del repositorio.
2. Ábrelo en un navegador moderno (Chrome, Edge, Firefox o Safari recientes).

Solo necesita conexión a internet para cargar las tipografías de Google Fonts. Sin conexión funciona igual, con tipografías del sistema.

### Con GitHub Pages

1. Renombra el archivo a `index.html` (o enlaza directamente al nombre actual).
2. En el repositorio ve a **Settings → Pages** y elige la rama y la carpeta raíz.
3. La web quedará publicada en `https://<usuario>.github.io/<repositorio>/`.

## Estructura del repositorio

```
.
├── interacciones-rayos-x.html   # Aplicación completa (HTML + CSS + JavaScript)
├── README.md
└── LICENSE                      # Opcional: texto de la licencia CC BY-NC-SA 4.0
```

## Tecnología

- HTML, CSS y JavaScript sin dependencias ni frameworks.
- Renderizado en **Canvas 2D**.
- Núcleo isométrico adaptado de `iso-kit.mjs` (proyección isométrica con acimut y elevación, ordenación por profundidad, extrusión de prismas con sombreado por caras y planta redondeada).
- Tipografías Instrument Sans y JetBrains Mono (Google Fonts).
- Requiere un navegador con soporte de `CanvasRenderingContext2D.roundRect` (Chrome 99+, Safari 16+, Firefox 112+).

## Modelo físico y limitaciones

**Es un modelo simplificado. Los valores incluidos son solo orientativos y de carácter didáctico, y no deben usarse para cálculos de dosimetría ni de protección radiológica.**

Principales simplificaciones:

- **Alcance:** solo radiología convencional (20–150 kVp). Se excluyen la producción de pares (> 1,02 MeV) y la fotodesintegración (> 10 MeV), que no ocurren en este rango. No se modela el efecto Rayleigh.
- **Energía media del haz:** se aproxima como 0,4 × kVp.
- **Índice de exposición:** E = (kVp / 70)² · (mAs / 10), con 70 kVp y 10 mAs como referencia. Hay subexposición con E < 0,45 y sobreexposición con E > 2,2.
- **Imagen del receptor:** fantoma de 96 × 96 píxeles con transmisión dependiente del kVp, radiación dispersa (relación dispersa/primaria) que aumenta con el kVp y se reduce con la rejilla y ruido cuántico que disminuye con la exposición.
- **Probabilidades de interacción:** fotoeléctrico ≈ (Z/7,4)^2,5 · (30/E)^3,1 respecto a Compton, que se toma constante. El efecto fotoeléctrico crece con Z³/E³ en la teoría, mientras que Compton depende de la densidad electrónica y disminuye lentamente con la energía.
- **Efecto Compton:** la energía final del fotón disperso sigue la fórmula de Compton, y el ángulo del electrón de retroceso se calcula a partir de ella. El plano de dispersión se sortea en cada disparo. Para que se vea con frecuencia el caso en que el fotón llega al receptor, el 40 % de los disparos se sortea entre 3° y 14°. La diferencia de longitud de onda dibujada entre el fotón incidente y el disperso está exagerada visualmente.
- **Rejilla:** acepta fotones a menos de atan(1/razón) respecto al eje. Se usa un factor de Bucky de 2, 4, 5 y 6 para las razones 5:1, 8:1, 12:1 y 16:1, y una eliminación de dispersión del 70, 82, 90 y 95 %. No se modelan el centrado ni la distancia focal.
- **Mismo mA, misma probabilidad:** el mA y el tiempo cambian cuántos fotones hay, no la probabilidad de interacción de cada fotón.

## Licencia

«Web interactiva sobre interacciones de los rayos X en radiodiagnóstico convencional» por Pablo A. Sendra Lleó (2026), disponible bajo licencia [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es).

## Créditos

Este material fue desarrollado con la asistencia del modelo de inteligencia artificial [Claude (Anthropic)](https://claude.ai), utilizando como base el framework geométrico e isométrico del repositorio de código abierto [Anatomy](https://github.com/wheresryan22/anatomy).

> Antes de publicar, comprueba que la licencia del repositorio Anatomy es compatible con CC BY-NC-SA 4.0 y conserva los avisos de copyright que exija.
