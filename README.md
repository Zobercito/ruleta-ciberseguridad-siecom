# Gira y Detecta — stand de ciberseguridad CIIECOM UP

Ruleta de escritorio para el stand «¿Estás seguro en Internet?»: el visitante
gira, cae en una casilla, resuelve un reto con el mouse y se lleva su premio.
La idea original está en `Gira_y_Detecta_Especificacion_Detallada.md`; esta
versión incorpora los cambios de la última iteración (ver *Diferencias con el
documento*).

## Cómo usarlo el día del evento

1. Abre `index.html` con doble clic (Chrome o Edge).
2. Pulsa `F11` para pantalla completa en el monitor de 24".
3. El visitante hace clic en **GIRAR** y resuelve el reto con el mouse.

No necesita internet, servidor ni instalación: todo (CSS, JavaScript, iconos y
favicon) está dentro del archivo.

## Las seis casillas

Cada una propone un ejercicio distinto, no la misma pregunta con otro texto:

| Casilla | Qué hace el visitante | Tiempo |
|---|---|---|
| ¿Real o fraude? | Juzga un mensaje y ve el fragmento exacto que lo delata | 15 s |
| Encuentra la alerta | Busca y pulsa la parte sospechosa del correo | 20 s |
| Contraseña fuerte | Compara tres contraseñas parecidas y ve cuánto tardarían en romperse | 15 s |
| ¿Qué ataque es? | Identifica el ataque entre tres opciones | 20 s |
| ¿Qué harías? | Elige la acción segura ante una situación real | 15 s |
| ¡Hackeado! | Decide bajo presión en una pantalla de ataque simulada | 6 s |

Retos disponibles: 6 de «¿Real o fraude?», 4 de «Encuentra la alerta», 4 de
«Contraseña fuerte», 6 de «¿Qué ataque es?», 7 de «¿Qué harías?» y 3 variantes
de «¡Hackeado!». Se eligen al azar sin repetirse hasta agotar la lista. Todo el
contenido es ficticio: dominios `.example`, «Banco Ejemplo» y cifras inventadas.

## Flujo

Girar (3,8 s) → casilla revelada (1,6 s) → reto con cronómetro → resultado con
la explicación → premio (12 s) → vuelve al inicio. El resultado avanza solo a
los 15 s si nadie pulsa SIGUIENTE. Tras 45 s sin clics, la página vuelve al
inicio.

## Controles del operador

| Tecla | Acción |
|---|---|
| `F11` | Pantalla completa del navegador |
| `R` o `Esc` | Reiniciar a la pantalla de inicio |
| `M` | Activar o silenciar sonido (apagado por defecto) |
| `N` | Saltar al siguiente estado |
| `O` | Mostrar u ocultar el panel de contadores |

El panel permite reiniciar el contador del día (participantes y aciertos). Se
guarda en `localStorage` y no identifica a nadie.

## Estructura

- `index.html` — aplicación completa (HTML + CSS + JavaScript vanilla).
- `CASILLAS` — nombre, color, icono y tiempo de cada casilla.
- `RETOS` — texto de los retos, separado de la lógica.
- `HACKS` — variantes de «¡Hackeado!» con sus tres decisiones.
- `TIPS` — consejos de la pantalla de premio.

## Diferencias con el documento

El documento es la especificación original. La aplicación cambió dos cosas a
propósito:

- **«Verdadero o falso»** se sustituyó por **«¿Qué harías?»**: las frases
  binarias se acertaban por azar y no hacían pensar.
- **«¡Hackeado!»** ya no es una secuencia automática: ahora plantea una decisión
  con seis segundos y cuenta como acierto, en lugar de dejar al visitante
  mirando diecisiete segundos sin hacer nada.

## Pruebas sugeridas antes del evento

- Abrir el archivo sin internet y comprobar que la ruleta cae en las 6 casillas.
- Verificar el cronómetro, el tiempo agotado y el paso al resultado.
- Comprobar la legibilidad a 2 metros con el monitor real.
- Revisar `R`/`Esc` durante «¡Hackeado!» y el reinicio por inactividad.
