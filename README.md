# Gira y Detecta — stand de ciberseguridad CIIECOM UP

Implementación de la especificación `Gira_y_Detecta_Especificacion_Detallada.md`
(la idea de la ruleta: gira, cae en una casilla, resuelve un reto y se lleva premio).

## Cómo usarlo el día del evento

1. Abre `index.html` con doble clic (Chrome o Edge).
2. Pulsa `F11` para pantalla completa en el monitor de 24".
3. El visitante hace clic en **GIRAR** y resuelve el reto con el mouse.

No necesita internet, servidor ni instalación: todo (CSS, JavaScript, iconos)
está dentro del archivo.

## Controles del operador

| Tecla | Acción |
|---|---|
| `F11` | Pantalla completa del navegador |
| `R` o `Esc` | Reiniciar a la pantalla de inicio |
| `M` | Activar o silenciar sonido (apagado por defecto) |
| `N` | Saltar al siguiente estado |
| `O` | Mostrar u ocultar el panel de contadores |

El panel del operador permite reiniciar el contador del día (participantes y
aciertos). El contador se guarda en `localStorage` y no identifica a nadie.
La página vuelve sola al inicio tras 45 s sin clics y tras 9,5 s en el premio.

## Contenido

- **6 casillas:** ¿Real o fraude?, Encuentra la alerta, Contraseña fuerte,
  ¿Qué ataque es?, Verdadero o falso y ¡Hackeado!
- 6 retos de «¿Real o fraude?», 4 de «Encuentra la alerta», 4 de «Contraseña
  fuerte», 6 de «¿Qué ataque es?», 7 de «Verdadero o falso» y 3 variantes de
  «¡Hackeado!». Se eligen al azar sin repetirse hasta agotar la lista.
- Todo el texto es ficticio: dominios `.example`, «Banco Ejemplo» y cifras
  inventadas. No se piden ni se guardan datos personales.

## Estructura

- `index.html` — aplicación completa (HTML + CSS + JavaScript vanilla).
- `RETOS` — datos de los retos al inicio del `<script>`, separados de la lógica
  para que el equipo pueda editar textos sin tocar el motor.
- `CASILLAS` — nombre, color, icono y tiempo de cada casilla.
- `HACKS` — variantes de la dinámica «¡Hackeado!».
- `TIPS` — consejos que aparecen en la pantalla de premio.

## Pruebas sugeridas antes del evento

- Abrir el archivo sin internet y comprobar que la ruleta cae en las 6 casillas.
- Verificar que el cronómetro termina y pasa al resultado.
- Comprobar la legibilidad a 2 metros con el monitor real.
- Revisar `R`/`Esc` durante «¡Hackeado!» y el reinicio por inactividad.
