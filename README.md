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

## 4) Fotos y videos

- **Fotos y logo**: se suben directo desde el panel. El propio navegador las redimensiona y comprime antes de guardarlas (fotos: hasta 1600px de lado, JPG calidad 85%; logo: hasta 1000px, PNG para conservar la transparencia), así una foto de cámara de varios MB queda en un tamaño manejable automáticamente. Aun así, `localStorage` tiene un límite de unos 5 MB por sitio — en la pestaña **Ajustes** hay una barra que muestra cuánto espacio se está usando. Si se llena, exportá el JSON como respaldo y bajá el peso de alguna foto.
- Para producción, si `config.json` termina pesando mucho por tener varias fotos en base64, conviene reemplazar esas imágenes por archivos reales dentro de `/assets` (ej. `assets/foto-1.jpg`) y poner esa ruta en el campo correspondiente en vez de subir el archivo — el resultado es un sitio más liviano y rápido.
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
