# Landing BBPS x CCA Cinemacenter — Pantalla LED de entrada

Landing comercial de una sola página, con scroll por bloques (una sección = una pantalla completa), pensada para enviar como propuesta a clientes. Es un sitio 100% estático (HTML/CSS/JS puro), no necesita build ni backend.

## Estructura del repo

```
.
├── index.html        → toda la página (estructura, estilos y lógica)
├── config.json        → el contenido "publicado": textos, precios, y rutas de fotos/videos
├── assets/
│   └── logos.png       → logo combinado (BBPS + CCA Cinemacenter)
├── vercel.json         → evita que Vercel cachee config.json de forma agresiva
└── .gitignore
```

## 1) Subir esto a GitHub

Desde esta misma carpeta:

```bash
git init
git add .
git commit -m "Landing BBPS - versión inicial"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git push -u origin main
```

(Creá antes el repositorio vacío en GitHub, sin README ni licencia, para que el push no choque con nada.)

## 2) Desplegar en Vercel

1. Entrá a [vercel.com](https://vercel.com) → **Add New → Project**.
2. Importá el repositorio de GitHub que acabás de crear.
3. Framework Preset: **Other** (o "Static"). No hace falta Build Command ni Output Directory — dejalos vacíos, Vercel sirve los archivos tal cual están.
4. Deploy. En un minuto vas a tener una URL tipo `tu-proyecto.vercel.app`.
5. (Opcional) En **Settings → Domains** podés conectar un dominio propio.

Cada vez que hagas `git push` a `main`, Vercel vuelve a desplegar solo.

## 3) Cómo editar el contenido (panel de administración)

En la esquina inferior derecha de la página hay un botón ⚙. Al tocarlo pide una clave (por defecto **`bbps2026`** — cambiala desde la pestaña "Ajustes" del panel apenas la uses).

Desde el panel podés editar, por sección: títulos, precios, características de los planes, emails de contacto, fotos (subiendo un archivo) y videos (pegando una URL de un `.mp4`).

**Importante:** lo que edites y guardes ahí se ve solo en tu propio navegador — funciona como una vista previa en vivo. Como el sitio es estático (sin base de datos), para que el cambio se vea igual para cualquier visitante hay que publicarlo en el repo:

1. Editá lo que necesites en el panel y tocá **Guardar cambios** (para previsualizar).
2. Tocá **Exportar JSON** (pestaña Ajustes). Se descarga un archivo `bbps-landing-config.json`.
3. Reemplazá el archivo `config.json` del repo por ese archivo descargado (mismo nombre: `config.json`).
4. `git add config.json && git commit -m "Actualizo contenido" && git push`.
5. Vercel redespliega solo en ~1 minuto y el cambio ya es visible para todos.

Si en algún momento el panel muestra algo raro (por ejemplo, después de importar un JSON viejo), **Restaurar contenido original** lo vuelve a dejar como el `config.json` publicado.

## 4) Fotos, logo y videos

Las fotos, el logo y los videos se cargan todos de la misma forma: **subís el archivo a la carpeta `/assets` del repo y pegás la ruta** (ej. `assets/foto-1.jpg`, `assets/campana-1.mp4`) en el campo correspondiente del panel. También podés pegar una URL completa a un archivo alojado en otro lado (ej. Cloudflare R2, Bunny.net).

El panel **ya no sube archivos como imagen incrustada** (esto se probó al principio y generaba errores de "almacenamiento lleno" apenas se cargaban un par de fotos de buena calidad, además de perderse al recargar la página). El campo de texto es la forma confiable de referenciar cualquier imagen o video, sin límite de tamaño en el navegador.

Para subir archivos nuevos a `/assets`, la forma más simple es con **GitHub Desktop**: cloná el repo a tu PC, copiá los archivos dentro de la carpeta `assets`, y hacé commit + push desde la app. (Ver sección 1 para más detalle).

Recomendación de peso: para que la página cargue rápido, especialmente en celular, comprimí las fotos (JPG, idealmente bajo 500 KB–1 MB) y los videos antes de subirlos. Para video, por ejemplo:
```bash
ffmpeg -i original.mov -vf scale=1080:-2 -c:v libx264 -crf 26 -preset slow -c:a aac -b:a 128k salida.mp4
```
- **Videos**: se referencian por URL a un archivo `.mp4` (no YouTube/Vimeo, que no se pueden insertar como `<video>` directo). Lo más simple es subir los `.mp4` a la carpeta `/assets` del repo y usar una ruta relativa, por ejemplo `assets/campana-1.mp4`.

## 5) Funcionalidades incluidas

- Scroll por bloques: cada sección ocupa una pantalla completa y el scroll salta de bloque en bloque (rueda del mouse, touch, flechas del teclado o los puntos de navegación de la derecha).
- Videos con controles propios de play/pausa y mute, tanto en el hero como en el carrusel de campañas.
- Carrusel de campañas: clic en los videos laterales para rotar, o con las flechas.
- Animación de conteo (motion graphics) en los números de la sección "Datos", disparada al entrar en pantalla.
- Animación de entrada escalonada en las tarjetas de la sección "Planes".
- Respeta `prefers-reduced-motion` y tiene foco de teclado visible para accesibilidad.
- Responsive (mobile, tablet, desktop).

## Nota sobre la clave del panel

La clave del panel es solo un freno para evitar ediciones accidentales — no es un sistema de autenticación seguro (cualquiera que mire el código fuente podría encontrarla). No la uses para proteger información sensible.
