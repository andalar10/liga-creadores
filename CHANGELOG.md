# Liga de Creadores: histórico de funcionalidades

Curso de programación para un niño de 10 años. App en un único `index.html`; el contenido son misiones cortas con Scratch.

## 2026-10-12 · v0.2 (cuenta y sincronización)

- La app está publicada en Vercel (equipo andalarsoft, proyecto `liga-creadores`) en https://liga-creadores.vercel.app, sin login de Vercel y marcada como no indexable.
- Cuenta con correo y contraseña (Supabase Auth del proyecto `andalar-projects`). Se puede entrar o crear la cuenta desde la propia app.
- El progreso se guarda en la nube (tabla `liga_progress`, una fila por usuario, protegida con RLS: cada cuenta solo ve y cambia lo suyo) y se abre igual desde cualquier equipo.
- Al entrar en una cuenta se fusiona lo que hay en el navegador con lo que hay en la nube: se suma todo lo hecho en ambos lados.
- «Empezar de cero» también borra el progreso de la nube y gana sobre lo que tengan otros dispositivos.
- Si se entra con otra cuenta en el mismo navegador, no se mezclan los progresos.
- Sin cuenta o sin conexión la app sigue funcionando con el guardado local, y se sincroniza al volver la conexión.
- Indicador de estado de sincronización y botón de salir.
- Corregido: en la primera visita (navegador vacío) abrir una misión daba error porque faltaban los pasos en el estado inicial.

## 2026-10-12 · v0.1 (Nivel 1: Fútbol)

- Mapa de temporada con las 5 misiones del Nivel 1. Cada misión se abre al completar la anterior.
- Misiones: Mueve el balón, ¡Gol!, El portero, El marcador, Tu primer partido completo.
- Cada misión tiene: objetivo, tiempo estimado, concepto que aprende, pasos que se marcan uno a uno, pista «Si no funciona» y reto extra opcional.
- Los nombres de bloques de Scratch salen como etiquetas de colores (movimiento, apariencia, eventos, control, sensores, operadores, variables).
- Botón «Abrir Scratch» que abre el editor en otra pestaña.
- XP por misión (10 / 15 / 15 / 10 / 30), +5 por reto extra y +2 por reto del día.
- Rangos: Cantera, Suplente, Titular, Capitán, Leyenda, con barra de progreso al siguiente.
- Semanas jugando: cuenta semanas (lunes a domingo), no días, y perdona una semana saltada.
- Reto del día de 5 minutos: pregunta de lógica con respuestas cerradas, rota cada día, se puede reintentar.
- Medalla «Delantero Creador» al terminar el Nivel 1, y huecos bloqueados para los siguientes niveles.
- Niveles futuros visibles en el mapa como «Próximamente»: Ping pong, Mates, Ajedrez, Python.
- Galería: se pega el enlace del juego compartido en Scratch (solo se aceptan enlaces de scratch.mit.edu) y sale en «Mi galería».
- Nombre del jugador en la cabecera.
- Progreso guardado en el navegador, con copia de seguridad por código (generar y cargar) y botón de empezar de cero con doble confirmación.
- Tema claro y oscuro, adaptado a móvil, celebración con confeti (desactivada si el sistema pide menos movimiento).

## Comprobaciones para detectar regresiones

Antes de dar por cerrado un cambio, revisar que siguen existiendo (por ejemplo con `grep` en `index.html`):

| Funcionalidad | Qué buscar |
| --- | --- |
| 5 misiones del Nivel 1 | `id: 'm1'` … `id: 'm5'` en `MISSIONS` |
| Chips de bloques de Scratch | `function fmt`, clases `.b-motion`, `.b-events`, `.b-control` |
| Desbloqueo secuencial | `function status` |
| XP y rangos | `function xp`, `RANKS`, `RETO_XP`, `DAILY_XP` |
| Semanas jugando | `function streakOf`, `function weekIdx` |
| Reto del día | `DAILY`, `data-act="daily"` |
| Medalla del Nivel 1 | `function medal`, `Delantero Creador` |
| Galería con enlaces validados | `LINK_RE`, `Mi galería` |
| Guardado local | `localStorage`, `function normalize`, `KEY` |
| Estado inicial con pasos de todas las misiones | `normalize(null)` devuelve `steps` de las 5 misiones |
| Copia de seguridad | `bk-show`, `bk-load` |
| Reinicio con doble confirmación | `resetArm`, `state.epoch = Date.now()` |
| Login y cuenta | `doAuth`, `loginHTML`, `acctHTML`, `signInWithPassword`, `signUp` |
| Sincronización | `mergeStates`, `pull`, `push`, tabla `liga_progress` |
| Tema claro y oscuro | `prefers-color-scheme`, `data-theme` |

Comprobación manual rápida:

1. Sin cuenta: completar la misión 1, ver que se abre la 2, que el XP sube a 10 y que la semana queda marcada.
2. La galería acepta un enlace válido de Scratch y rechaza uno que no lo sea.
3. Con cuenta: entrar, completar algo, abrir la web en otro navegador con la misma cuenta y ver el mismo progreso.
4. «Empezar de cero» en un equipo deja a cero también el otro tras recargar.
5. Primera visita en un navegador vacío: abrir una misión no da error.
