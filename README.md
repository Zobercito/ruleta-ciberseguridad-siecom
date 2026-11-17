# Gira y Detecta — stand de ciberseguridad CIIECOM UP

Ruleta de escritorio para el stand «¿Estás seguro en Internet?»: el visitante
gira, cae en una casilla, resuelve **tres retos** de esa dinámica con el mouse y
ve su puntuación antes de recoger el premio. La idea original está en
`Gira_y_Detecta_Especificacion_Detallada.md`; esta versión incorpora los cambios
posteriores (ver *Diferencias con el documento*).

## Cómo usarlo el día del evento

1. Abre `index.html` con doble clic (Chrome o Edge).
2. Pulsa `F11` para pantalla completa en el monitor de 24".
3. El visitante hace clic en **GIRAR** y resuelve la ronda con el mouse.

No necesita internet, servidor ni instalación: todo (CSS, JavaScript, iconos y
favicon) está dentro del archivo.

## Las seis casillas

Cada una propone un ejercicio distinto, no la misma pregunta con otro texto:

| Casilla | Qué hace el visitante | Ronda |
|---|---|---|
| ¿Real o fraude? | Juzga un mensaje y ve el fragmento exacto que lo delata | 3 mensajes |
| Encuentra la alerta | Busca y pulsa la parte sospechosa del correo | 3 correos |
| Contraseña fuerte | Compara tres contraseñas parecidas y ve cuánto tardarían en romperse | 3 tandas |
| ¿Qué ataque es? | Identifica el ataque entre tres opciones | 3 situaciones |
| ¿Qué harías? | Elige la acción segura ante una situación real | 3 situaciones |
| ¡Hackeado! | Decide bajo presión en una pantalla de ataque simulada | 1 decisión |

No hay cronómetro en las dinámicas de conocimiento: el visitante lee y decide a
su ritmo. La única cuenta atrás es la de «¡Hackeado!» (8 s), porque ahí la prisa
es justamente lo que se está enseñando.

Retos disponibles: 6 de «¿Real o fraude?», 4 de «Encuentra la alerta», 4 de
«Contraseña fuerte», 6 de «¿Qué ataque es?», 7 de «¿Qué harías?» y 3 variantes
de «¡Hackeado!». Se eligen al azar sin repetirse hasta agotar la lista. Todo el
contenido es ficticio: dominios `.example`, «Banco Ejemplo» y cifras inventadas.

## Flujo

Girar (3,8 s) → casilla revelada (1,6 s) → **tres retos sin límite de tiempo**
(cada uno: reto → resultado con la explicación) → premio con la puntuación
(«Acertaste 2 de 3», «¡PLENO!» si son tres) → vuelve al inicio. Si el visitante
no toca nada en un resultado, avanza solo a los 20 s; tras 60 s de inactividad
la página vuelve al inicio.

## Controles del operador

| Tecla | Acción |
|---|---|
| `F11` | Pantalla completa del navegador |
| `R` o `Esc` | Reiniciar a la pantalla de inicio |
| `M` | Activar o silenciar sonido (apagado por defecto) |
| `N` | Saltar al siguiente estado |
| `O` | Mostrar u ocultar el panel de contadores |

El panel muestra participantes, respuestas correctas y permite reiniciar el
contador del día. Se guarda en `localStorage` y no identifica a nadie.

## Estructura

- `index.html` — aplicación completa (HTML + CSS + JavaScript vanilla).
- `CASILLAS` — nombre, color e icono de cada casilla.
- `RETOS` — texto de los retos, separado de la lógica.
- `HACKS` — variantes de «¡Hackeado!» con sus tres decisiones.
- `TIPS` — consejos de la pantalla de premio.
- `RETOS_POR_RONDA` — cuántos retos tiene cada ronda (3 por defecto).

## Diferencias con el documento

El documento es la especificación original. La aplicación cambió estas cosas a
propósito:

- **«Verdadero o falso»** se sustituyó por **«¿Qué harías?»**: las frases
  binarias se acertaban por azar y no hacían pensar.
- **«¡Hackeado!»** ya no es una secuencia automática: ahora plantea una decisión
  con ocho segundos y cuenta como acierto, en lugar de dejar al visitante
  mirando diecisiete segundos sin hacer nada.
- **Sin cronómetro en las dinámicas de conocimiento**: el cronómetro castigaba a
  quien lee despacio y contradecía el mensaje del stand («piensa antes de hacer
  clic»). Solo se mantiene la cuenta atrás del ataque simulado.
- **Ronda de tres retos con puntuación** en lugar de un único reto: una sola
  pregunta sabía a poco y no dejaba nada que medir al final. «¡Hackeado!» sigue
  siendo una decisión única, porque repetirla rompe la sorpresa.

## Pruebas sugeridas antes del evento

- Abrir el archivo sin internet y comprobar que la ruleta cae en las 6 casillas.
- Completar una ronda de tres retos y ver la puntuación en el premio.
- Comprobar la legibilidad a 2 metros con el monitor real.
- Revisar `R`/`Esc` durante «¡Hackeado!» y el reinicio por inactividad.
