# Aula AR – Prototipo WebAR (Bernardo O'Higgins)

Tipo de AR: **Image Tracking** (MindAR + A-Frame). El retrato de O'Higgins es el "marcador": al detectarlo, aparece encima un video que el usuario puede reproducir.

## Estructura
```
index.html        → App: Asignaturas → Cultura (O'Higgins) → Pregunta (#quiz)
ar.html           → Experiencia AR (cámara + video + pregunta 3D sobre el retrato)
compiler.html     → Genera assets/targets.mind a partir del retrato
assets/
  ohiggins.jpg        → retrato (UI + imagen a reconocer)
  video-ohiggins.mp4  → video que se reproduce en AR
  poster.jpg          → miniatura del video en la pantalla de pregunta
  targets.mind        → (lo generas tú, ver paso 1)
```

## 1. Generar el marcador (una sola vez, recomendado)
1. Abre `compiler.html` en el navegador (con Live Server) → **Compilar**.
2. Mueve el `targets.mind` descargado a `assets/`.

> Si no existe `assets/targets.mind`, `ar.html` compila el retrato automáticamente la primera vez (10-30 s en móvil) y lo guarda en caché.

## 2. Probar en el computador
- VS Code → extensión **Live Server** → clic derecho en `index.html` → *Open with Live Server*.
- `localhost` cuenta como contexto seguro, así que la webcam funciona.

## 3. Probar en el celular (requiere HTTPS)
- **Rápido:** `ngrok http 5500` y abre la URL `https://…ngrok…` en el teléfono.
- **Permanente:** arrastra la carpeta a https://app.netlify.com/drop o `npx vercel`.

Para probar: abre `assets/ohiggins.jpg` en la pantalla del computador (o imprímelo) y apunta el celular.

## Pregunta en AR
Al terminar el video (o con *Responder la pregunta*) aparece sobre el retrato un panel 3D con la pregunta y dos respuestas que se tocan en la cámara. Correcta → verde + confeti + vibración; incorrecta → rojo y se puede reintentar. Los textos se dibujan en `<canvas>` (función `drawPanel`) porque la fuente de `<a-text>` no trae tildes. Para cambiar la pregunta edita `renderQuestion()` y `answer()` en `ar.html`.

## Problemas comunes (ya prevenidos en el código)
| Síntoma | Causa / solución |
|---|---|
| La cámara no abre | Falta HTTPS o permiso de cámara. Revisa el candado del navegador. |
| El video se ve negro/oscuro | Se usa `material="shader: flat"` (no depende de luces). |
| El video no suena en iPhone | Hace falta un toque del usuario: el botón *Iniciar cámara* desbloquea el audio. |
| Se abre a pantalla completa en iOS | `playsinline webkit-playsinline` en el `<video>`. |
| No detecta el retrato | Buena luz, sin reflejos, que el retrato ocupe ~60 % de la pantalla. Si cambias la imagen, vuelve a compilar `targets.mind`. |
| Video muy grande/pequeño | En `ar.html`, el ancho del retrato = 1 unidad. Ajusta `width/height` del `<a-video>` (16:9 → 1 × 0.5625). |
