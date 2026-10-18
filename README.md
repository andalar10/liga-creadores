# Liga de Creadores

App de un curso de programación con Scratch para un niño de 10 años, pensada para hacer misiones cortas (10-15 min) cuando haya un rato.

## Cómo está hecha

- Un único fichero, `index.html`, sin paso de compilación. Carga Google Fonts y `supabase-js` (2.45.4, desde jsDelivr).
- Publicada en Vercel: equipo `andalarsoft`, proyecto `liga-creadores`, https://liga-creadores.vercel.app (sin Vercel Auth y con `noindex`).
- Datos en Supabase, proyecto `andalar-projects`: tabla `public.liga_progress` (`user_id`, `data` jsonb, `updated_at`), una fila por usuario, con RLS para que cada cuenta solo vea y cambie su fila. `anon` no tiene acceso.
- La clave que va en el HTML es la `anon` (pública por diseño). Si Supabase la desactiva, hay que sustituir `SB_KEY` por una clave `sb_publishable_…` y actualizar `supabase-js`.
- Sin cuenta, o sin conexión, funciona con `localStorage`. Con cuenta, se fusiona con la nube al entrar y cada vez que la pestaña vuelve a estar visible.
- Scratch no está integrado: cada misión tiene un botón que abre el editor en otra pestaña.

## Cuenta del niño

Las cuentas se crean desde la propia app (botón «Entrar para guardarlo en la nube» y luego «Crear cuenta»). Según la configuración de Supabase Auth, puede pedir confirmar el correo. El enlace de confirmación vuelve a la URL del sitio configurada en Supabase Auth, que conviene que sea la de esta web.

## Añadir una misión

Añade un objeto al array `MISSIONS` en `index.html` con `id`, `n`, `title`, `time`, `xp`, `concept`, `goal`, `steps`, `fail` y `reto`. En los textos, `{categoria|texto}` pinta un bloque de Scratch con su color (`motion`, `looks`, `sound`, `events`, `control`, `sensing`, `operators`, `variables`). Si cambian los pasos de una misión, el progreso guardado se normaliza solo.

## Pendiente

- Niveles 2 a 5 (Ping pong, Mates, Ajedrez, Python).
- Dominio propio bajo el proyecto de dominio común que estás preparando.

Histórico de funcionalidades y lista de comprobaciones: `CHANGELOG.md`.
