# Gira y Detecta: especificación detallada

**Stand de ciberseguridad CIIECOM UP: «¿Estás seguro en Internet?»**
*Piensa antes de hacer clic. Protege antes de lamentar.*

Este documento describe **qué hace la página, cómo se ve y qué muestra cada pantalla** dentro de cada dinámica de la ruleta. Está escrito para que un desarrollador (o un agente de programación) pueda construirla sin tener que adivinar nada.

---

## Índice

1. Resumen de la idea
2. Condiciones del stand y restricciones
3. Decisiones técnicas
4. Sistema visual (colores, tipografía, formas)
5. Estructura general de la pantalla
6. Flujo y estados de la aplicación
7. La ruleta (diseño y lógica)
8. Las 6 dinámicas: qué muestra cada pantalla
   - 8.1 ¿Real o fraude?
   - 8.2 Encuentra la alerta
   - 8.3 Contraseña fuerte
   - 8.4 ¿Qué ataque es?
   - 8.5 Verdadero o falso
   - 8.6 ¡Hackeado!
9. Pantalla de premio
10. Componentes comunes (cronómetro, resultado, botones)
11. Estructura de datos de los retos
12. Controles del operador y robustez
13. Accesibilidad y legibilidad a distancia
14. Lista de pruebas antes del evento
15. Prompt listo para el agente de programación

---

## 1. Resumen de la idea

Una ruleta digital en pantalla grande. El visitante:

1. **Gira** la ruleta con un clic.
2. Cae en una de **6 casillas** (cada una es una dinámica distinta).
3. Resuelve **un solo reto** solo con el mouse, con cronómetro.
4. Ve la **explicación** de la respuesta correcta (acierte o falle).
5. Recibe su **premio** (boquita y tarjeta de consejos, entregados por el equipo).

**Duración objetivo por persona: alrededor de 1 minuto.**

| Etapa | Qué pasa | Tiempo |
|---|---|---|
| Girar | Clic y animación de la ruleta | 5 s |
| Reto | Se resuelve con clics, cronómetro de 15 a 20 s | 20 s |
| Explicación | Respuesta correcta y por qué | 15 s |
| Premio | Pantalla de cierre y entrega | 10 s |

Principios:

- **Un giro, un reto, un premio.** Sin puntos por persona ni rondas.
- **Todo con clics.** Sin teclado. Nadie escribe contraseñas.
- **Todos aprenden**, incluso si fallan o se acaba el tiempo.
- **Todo es ficticio.** No se piden ni se guardan datos personales.
- **La pantalla es el espectáculo:** el público que espera también puede leer y opinar.

---

## 2. Condiciones del stand y restricciones

| Condición | Consecuencia |
|---|---|
| Laptop conectada a un monitor de **24"** | Resolución objetivo **1920×1080**, navegador en **pantalla completa** (F11) |
| Solo **mouse** | Botones enormes, zonas de clic grandes, sin interacciones finas |
| Se mira desde **1 a 3 metros** | Texto grande, contraste alto, pocas palabras por pantalla |
| Hay fila y público | Retos de menos de 20 s; explicaciones de 2 a 3 líneas |
| Red del evento poco confiable | La página debe funcionar **sin internet** |
| Puede haber ruido | Sonido apagado por defecto |
| Puede haber clics accidentales | Bloqueo breve de clics entre pantallas |

---

## 3. Decisiones técnicas

**¿Hace falta JavaScript? Sí, un poco.** HTML y CSS solos no pueden:

- Elegir una casilla al azar y detener la ruleta en ella.
- Contar el tiempo del reto.
- Elegir un reto al azar y no repetirlo.
- Evaluar la respuesta y mostrar el resultado.
- Reiniciar solos tras inactividad.

**Recomendación:**

- **Un solo archivo `index.html`** con CSS y JavaScript incluidos.
- **JavaScript sin librerías** (vanilla). Sin CDN, sin Google Fonts, sin imágenes remotas.
- Íconos como **SVG en línea** o formas CSS.
- Fuentes del sistema: `"Segoe UI", Arial, Helvetica, sans-serif`. Monoespaciada para detalles "hacker": `Consolas, "Courier New", monospace`.
- Los retos van en **un arreglo de objetos al inicio del script**, para que el equipo los edite sin tocar la lógica.
- `localStorage` solo para el contador del día (dentro de try/catch; si falla, la página sigue funcionando).

---

## 4. Sistema visual

### 4.1 Paleta de colores

Fondo oscuro, que combina con el stand y se ve bien en pantalla grande.

| Uso | Color | Variable CSS sugerida |
|---|---|---|
| Fondo principal | `#0A1428` | `--bg` |
| Paneles y tarjetas | `#12213F` | `--panel` |
| Paneles elevados / hover | `#1A2D54` | `--panel-2` |
| Azul principal (botones, bordes) | `#2D8CFF` | `--blue` |
| Celeste de acento (brillos, títulos) | `#6FD3FF` | `--cyan` |
| Texto principal | `#FFFFFF` | `--text` |
| Texto secundario | `#B8C7E0` | `--text-2` |
| Acierto | `#2ECC71` | `--ok` |
| Error / alerta / hackeado | `#FF4D4D` | `--bad` |
| Advertencia / poco tiempo | `#FFC83D` | `--warn` |

Reglas de color:

- Texto **blanco sobre fondo oscuro** siempre. Nunca gris claro sobre azul claro.
- **Rojo solo para alertas, errores y la casilla «¡Hackeado!»**; no se usa como decoración.
- **No depender solo del color:** acierto y error también llevan **ícono y palabra** («¡Correcto!» / «Casi»), por si alguien no distingue verde de rojo.

### 4.2 Tipografía

| Elemento | Tamaño (a 1920×1080) | Peso |
|---|---|---|
| Título del stand | 80 a 100 px | 800, MAYÚSCULAS |
| Nombre de casilla revelada | 96 a 120 px | 800 |
| Enunciado del reto | 52 a 64 px | 600 |
| Contenido de mensajes ficticios | 36 a 44 px | 400 a 500 |
| Botones de respuesta | 44 a 56 px | 700 |
| Explicación | 40 a 48 px | 500 |
| Textos secundarios (contador, cronómetro) | 32 px mínimo | 500 |

**Ningún texto por debajo de 28 px.** Ante la duda, más grande. Cada pantalla debe leerse desde 2 metros.

### 4.3 Formas y estilo

- **Estilo:** tecnológico, limpio, con brillo suave. No saturar.
- **Esquinas redondeadas:** 20 a 28 px en tarjetas y botones. La ruleta es un círculo.
- **Bordes:** 3 px en celeste o azul, con `box-shadow` suave tipo glow (`0 0 24px rgba(45,140,255,.35)`).
- **Fondo:** cuadrícula tenue o líneas de circuito al 5 a 8% de opacidad, y un **escudo grande semitransparente** detrás del contenido. No debe competir con el reto.
- **Íconos (SVG simple):** escudo, candado, anzuelo (phishing), bandera de alerta, llave, calavera o pantalla rota, reloj.
- **Animaciones:** suaves y breves. Solo «¡Hackeado!» puede tener parpadeo, y limitado a 3 s.
- **Botones:** altos (mínimo 120 px de alto), con estado `hover` claro (más brillo) y `active` (se hunden un poco).

### 4.4 Colores por casilla (sectores de la ruleta)

Mantener la paleta; variar el tono de azul y usar rojo solo para «¡Hackeado!».

| Casilla | Color del sector | Ícono |
|---|---|---|
| ¿Real o fraude? | `#2D8CFF` | Anzuelo |
| Encuentra la alerta | `#1F6FD1` | Bandera |
| Contraseña fuerte | `#3FB8E8` | Llave |
| ¿Qué ataque es? | `#5B6BFF` | Lupa |
| Verdadero o falso | `#2AA6A0` | Check y X |
| ¡Hackeado! | `#FF4D4D` | Calavera |

---

## 5. Estructura general de la pantalla

Toda la aplicación es **una sola página fija de 100vw × 100vh, sin scroll**. Siempre hay tres franjas:

```
┌────────────────────────────────────────────────────────────┐
│  CABECERA (alto ~110 px)                                   │
│  Escudo + «CIBERSEGURIDAD»            Contador del día     │
├────────────────────────────────────────────────────────────┤
│                                                            │
│                 ZONA PRINCIPAL (~800 px)                   │
│        Cambia según el estado: ruleta, reto, etc.          │
│                                                            │
├────────────────────────────────────────────────────────────┤
│  PIE (alto ~90 px)                                         │
│  «Piensa antes de hacer clic. Protege antes de lamentar.»  │
└────────────────────────────────────────────────────────────┘
```

- **Cabecera:** logo o escudo a la izquierda, nombre corto del stand, y a la derecha el contador «Hoy han participado: N».
- **Pie:** el lema, siempre visible, en celeste y 32 px.
- En la pantalla de **¡Hackeado!** la cabecera y el pie se ocultan o se oscurecen para dar pantalla completa al efecto.

---

## 6. Flujo y estados de la aplicación

```
 S0 INICIO ──clic──► S1 GIRANDO ──se detiene──► S2 CASILLA REVELADA
                                                      │ (1.5 s)
                                                      ▼
 S0 ◄──8 a 10 s o clic── S5 PREMIO ◄── S4 RESULTADO ◄── S3 RETO
                                          ▲                 │
                                          └─ tiempo agotado ┘
```

| Estado | Nombre | Qué ocurre | Sale cuando |
|---|---|---|---|
| S0 | Inicio | Ruleta quieta e invitación a girar | Clic en la ruleta o en GIRAR |
| S1 | Girando | Animación de 3.5 a 4 s | Termina la animación |
| S2 | Casilla revelada | Se ilumina el sector y aparece su nombre en grande | 1.5 s |
| S3 | Reto | Depende de la casilla (sección 8) | Respuesta o fin del tiempo |
| S4 | Resultado | Correcto o no, más explicación | Clic en SIGUIENTE o 15 s |
| S5 | Premio | Mensaje, consejo y recordatorio de recoger premio | 8 a 10 s o clic |
| — | Inactividad | Cualquier estado sin clics durante 45 s | Vuelve a S0 |

**Regla del bloqueo:** después de cada cambio de estado, ignorar clics durante **400 ms** para evitar dobles clics accidentales.

**Regla del contador del día:** al llegar a S5 se suma 1 a «participantes» (y 1 a «aciertos» si acertó). Es un detalle de ambiente: **no guarda quién jugó**.

---

## 7. La ruleta

### 7.1 Diseño

- Círculo de **720 a 800 px** de diámetro en 1080p, centrado verticalmente en la zona principal.
- **6 sectores iguales de 60°**, cada uno con color, ícono y nombre corto.
- **Puntero** fijo en la parte superior (triángulo blanco o rojo con sombra), siempre visible y por encima de la rueda.
- **Centro:** botón circular grande con la palabra **GIRAR** (o un escudo).
- **Borde exterior** con puntos tipo luces LED; pueden pulsar suavemente en S0.
- En S0, el texto **«¡Gira y descúbrelo!»** a un lado o debajo, en grande.

### 7.2 Distribución en la pantalla de inicio (S0)

```
┌──────────────────────────────────────────────────────────┐
│ CABECERA                                                 │
├───────────────────────────────┬──────────────────────────┤
│                               │  CIBERSEGURIDAD:         │
│        RULETA (780 px)        │  ¿ESTÁS SEGURO           │
│        con puntero arriba     │  EN INTERNET?            │
│                               │                          │
│                               │  ¡Gira y descúbrelo!     │
│                               │  [   GIRAR   ] (botón)   │
├───────────────────────────────┴──────────────────────────┤
│ Piensa antes de hacer clic. Protege antes de lamentar.   │
└──────────────────────────────────────────────────────────┘
```

### 7.3 Implementación sugerida

- **Sectores:** `background: conic-gradient(...)` en un `div` circular, con los 6 colores. Los íconos y textos se colocan con `transform: rotate(...) translateY(...)` desde el centro.
- **Giro:** `transform: rotate(Xdeg)` con `transition: transform 3.8s cubic-bezier(.12,.7,.15,1)` (arranque rápido y frenado largo).
- **Cálculo del ángulo final** (puntero arriba, sectores numerados 0 a 5 en sentido horario desde el tope):

```js
const SECTOR = 60;                    // grados por sector
const idx = elegirCasilla();          // 0..5, al azar
const jitter = (Math.random() - 0.5) * 40;   // ±20°, para que no caiga siempre al centro
const vueltas = 360 * 5;              // 5 vueltas completas
const destino = vueltas + (360 - (idx * SECTOR + SECTOR / 2)) + jitter;
rueda.style.transform = `rotate(${destino}deg)`;
// Al terminar la transición (evento transitionend) se pasa a S2 con la casilla idx
```

- **Guardar la rotación acumulada** para que el siguiente giro empiece desde donde quedó (o reiniciar con `transition: none` a `rotate(0)` al volver a S0).
- **Elección de casilla:** al azar, pero **evitar repetir la misma casilla dos veces seguidas** para que el público vea variedad.
- **Ruido de matraca** (opcional): sonido corto que se repite mientras gira, solo si el sonido está activado.

---

## 8. Las 6 dinámicas: qué muestra cada pantalla

Convenciones para todas las dinámicas:

- **Pantalla de reto (S3):** cabecera de casilla (ícono + nombre + cronómetro), contenido del reto y botones de respuesta.
- **Pantalla de resultado (S4):** el mismo contenido del reto, pero con **las señales marcadas**, un banner de acierto o fallo, y la explicación.
- **Cronómetro:** barra horizontal que se vacía, más el número de segundos; azul, luego amarillo (últimos 7 s) y rojo (últimos 3 s). Duración de **20 s** salvo indicación contraria.
- **Todo el contenido es ficticio.** Los dominios deben ser falsos y evidentemente de prueba (no usar marcas bancarias reales; usar nombres genéricos como «Banco Ejemplo»).

---

### 8.1 ¿Real o fraude?

**Idea:** el visitante ve un mensaje que parece cotidiano y decide si es real o un intento de fraude.

**Tiempo:** 15 s.

#### Pantalla de reto (S3)

```
┌──────────────────────────────────────────────────────────┐
│ [anzuelo] ¿REAL O FRAUDE?                  ⏱ 15  ▓▓▓▓▓▓▓ │
├──────────────────────────────────────────────────────────┤
│                                                          │
│   ┌────────────────────────────────────────────────┐     │
│   │  MENSAJE FICTICIO (correo, SMS o WhatsApp)     │     │
│   │  estilo ventana de aplicación, texto 36-44 px  │     │
│   └────────────────────────────────────────────────┘     │
│                                                          │
│   ┌──────────────────┐       ┌──────────────────┐        │
│   │   ✓  REAL        │       │   ⚠  FRAUDE      │        │
│   │  (botón azul)    │       │  (botón rojo)    │        │
│   └──────────────────┘       └──────────────────┘        │
└──────────────────────────────────────────────────────────┘
```

- **Tarjeta del mensaje:** imita el formato (barra de correo con «De / Asunto», burbuja de WhatsApp o SMS). Ocupa el 55 a 60% de la zona principal.
- **Dos botones enormes** (mínimo 480×140 px), lado a lado, con ícono y palabra.

#### Pantalla de resultado (S4)

- El **mismo mensaje** se queda en pantalla, pero las partes sospechosas se **resaltan** con subrayado rojo y **números** (①, ②, ③).
- Banner arriba: **«¡Correcto!»** (verde) o **«Casi. Mira por qué:»** (rojo suave).
- A la derecha o debajo, **lista corta** de las señales numeradas (máximo 3).
- Botón **SIGUIENTE**.

#### Retos (mínimo 5, uno aparece por giro)

| # | Mensaje ficticio | Respuesta | Señales y explicación |
|---|---|---|---|
| 1 | Correo. **De:** soporte@banc0-seguro.example · **Asunto:** URGENTE: tu cuenta será suspendida en 24 horas · «Ingresa aquí para verificar tus datos» | **Fraude** | ① Dominio mal escrito (cero en lugar de «o»). ② Presión de tiempo. ③ Pide datos por un enlace. |
| 2 | WhatsApp de número desconocido: «Hola mamá, cambié de número. ¿Me depositas $200 ahorita? Es urgente» | **Fraude** | ① Número nuevo. ② Urgencia y dinero. ③ Evita la llamada. Confirma llamando al número de siempre. |
| 3 | Notificación de app: «Se inició sesión en tu cuenta desde tu teléfono. Si fuiste tú, no necesitas hacer nada.» Sin enlaces. | **Real** | ① No pide datos. ② No trae enlaces. ③ Coincide con algo que acabas de hacer. |
| 4 | SMS: «¡Ganaste un teléfono nuevo! Paga $9.99 de envío para reclamarlo: ofertas-ya.example/premio» | **Fraude** | ① Premio de un concurso en el que no participaste. ② Pide un pago. ③ Enlace desconocido. |
| 5 | Correo de la universidad: «Tu horario del próximo semestre ya está disponible. Entra al portal escribiendo la dirección oficial en tu navegador.» Sin enlace. | **Real** | ① Sin presión. ② Sin datos pedidos. ③ Te indica entrar por tu cuenta, no por un enlace. |
| 6 | Correo: «Hemos detectado un acceso extraño. Descarga el archivo adjunto «Factura_urgente.exe» para revisar tu cuenta.» | **Fraude** | ① Archivo ejecutable adjunto. ② Mensaje alarmista. ③ Una empresa seria no pide instalar nada por correo. |

---

### 8.2 Encuentra la alerta

**Idea:** un correo falso en pantalla; el visitante hace clic en la parte que le parece sospechosa. Es la dinámica más visual y la que más «detective» se siente.

**Tiempo:** 20 s.

#### Pantalla de reto (S3)

```
┌──────────────────────────────────────────────────────────┐
│ [bandera] ENCUENTRA LA ALERTA              ⏱ 20  ▓▓▓▓▓▓▓ │
│ Haz clic en lo que te parece sospechoso                  │
├──────────────────────────────────────────────────────────┤
│  ┌────────────────────────────────────────────────────┐  │
│  │ [ZONA A]  De: …………………………                         │  │
│  │ [ZONA B]  Asunto: ……………………                       │  │
│  │ ──────────────────────────────────────────────     │  │
│  │ [ZONA C]  Cuerpo del mensaje (texto)               │  │
│  │ [ZONA D]  [ BOTÓN / ENLACE FALSO ]                 │  │
│  │ [ZONA E]  Adjunto o firma                          │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

- El correo ocupa casi toda la pantalla (unos 1500×680 px).
- Hay **4 o 5 zonas clicables**: remitente, asunto, frase del cuerpo, enlace o botón, adjunto o firma.
- **Cada zona mide mínimo 80 px de alto** y se ilumina con borde celeste al pasar el mouse. Nunca zonas diminutas.
- **Una sola selección.** Algunas zonas son señales reales y otras son normales (distractores).
- **No hay botones de respuesta**: la respuesta es el clic sobre el correo.

#### Pantalla de resultado (S4)

- Si hizo clic en una **señal real**: banner verde «¡Buen ojo!».
- Si hizo clic en una **zona normal**: banner rojo suave «Esa parte es normal. Fíjate en esto:».
- En ambos casos, **todas las señales reales quedan marcadas en rojo con número**, y a un lado aparecen 2 o 3 líneas que explican cada una.
- Botón SIGUIENTE.

#### Retos (mínimo 4)

| # | Correo ficticio | Señales reales (cuentan como acierto) | Zonas normales (distractores) |
|---|---|---|---|
| 1 | De: `seguridad@cuenta-verifica.example` · Asunto: «Acción inmediata requerida» · «Haz clic en el botón antes de que tu cuenta sea cerrada.» · Botón «VERIFICAR AHORA» → `http://192.0.2.15/login` | Remitente que no es de la empresa · Lenguaje de urgencia · Enlace con números en lugar de nombre | Saludo «Estimado usuario» (genérico, pero común) · Firma con logotipo |
| 2 | Correo de «Recursos Humanos»: «Adjuntamos la nueva política salarial» · Adjunto: `Politica_Salarial.pdf.exe` | Doble extensión del archivo (.pdf.exe) · Remitente externo con nombre de la empresa | Asunto con fecha · Texto corto |
| 3 | Correo de «tu banco»: «Tu tarjeta fue bloqueada. Responde este correo con tu número de tarjeta y tu clave para desbloquearla.» | Pide clave y número de tarjeta por correo · Tono alarmista | Hora de envío · Despedida cordial |
| 4 | Correo de «paquetería»: «Tu paquete no pudo entregarse. Paga $1.50 en el enlace para reprogramar» · Enlace `entregas-rapidas.example/pago` | Cobro inesperado · Enlace con dominio desconocido · Paquete que no esperabas | Número de guía · Logo |

---

### 8.3 Contraseña fuerte

**Idea:** tres contraseñas ficticias. El visitante hace clic en la más segura y la pantalla muestra, con un efecto visual, cuánto tardaría en romperse cada una. Es el «efecto wow» de seguridad.

**Tiempo:** 15 s.

#### Pantalla de reto (S3)

```
┌──────────────────────────────────────────────────────────┐
│ [llave] CONTRASEÑA FUERTE                  ⏱ 15  ▓▓▓▓▓▓▓ │
│ ¿Cuál es la más segura?                                  │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐ │
│  │      A        │  │      B        │  │      C        │ │
│  │  [candado]    │  │  [candado]    │  │  [candado]    │ │
│  │  123456       │  │  Sofia2005    │  │  Cielo!Verde  │ │
│  │               │  │               │  │  -Tren#58     │ │
│  └───────────────┘  └───────────────┘  └───────────────┘ │
│        (tres tarjetas enormes, 520×460 px cada una)      │
└──────────────────────────────────────────────────────────┘
```

- **Tres tarjetas** del mismo tamaño, con la contraseña en fuente monoespaciada de unos 52 px.
- Todo el tarjetón es clicable.
- El orden de la correcta **cambia al azar** en cada reto.

#### Pantalla de resultado (S4)

- Las tres tarjetas se quedan en su lugar. Debajo de cada una aparece una **barra de fortaleza** que se llena (roja, amarilla, verde) y el texto **«Tiempo estimado para descifrarla»** con un valor ilustrativo.
- La elegida queda con borde verde si acertó, o rojo si no; la correcta se marca con **«✓ La más segura»**.
- Explicación de 2 líneas: por qué la correcta resiste más.
- Aviso pequeño pero legible (28 px): **«Tiempos ilustrativos»**.

#### Retos (mínimo 4 conjuntos)

| # | Opción A | Opción B | Opción C | Correcta | Explicación |
|---|---|---|---|---|---|
| 1 | `123456` | `Sofia2005` | `Cielo!Verde-Tren#58` | C | Larga, mezcla palabras, símbolos y números, y no usa datos personales. |
| 2 | `qwerty` | `Tigre-Luna-Bote-91!` | `password1` | B | Una frase larga y rara resiste mucho más que palabras comunes. |
| 3 | `MiPerro2010` | `Zq7!mP2#vR9` | `contraseña` | B | Es aleatoria, mezcla tipos de caracteres y no es una palabra. (Nota: un gestor de contraseñas ayuda a recordarlas.) |
| 4 | `Mar-Roja-Pan-Tren-47` | `admin123` | `Juan1990` | A | Frase larga con varias palabras. Nombre y año de nacimiento son lo primero que prueba un atacante. |

**Valores ilustrativos para la pantalla de resultado** (para mostrar, no son cálculos exactos):

| Tipo de contraseña | Texto a mostrar |
|---|---|
| Muy débil (`123456`, `qwerty`, `password1`, `admin123`, `contraseña`) | «Al instante» |
| Débil (nombre + año) | «Unas horas» |
| Fuerte (frase larga con símbolos) | «Cientos de años» |

---

### 8.4 ¿Qué ataque es?

**Idea:** se lee una situación corta y el visitante identifica de qué ataque se trata. Enseña el vocabulario básico de la ciberseguridad.

**Tiempo:** 20 s.

#### Pantalla de reto (S3)

```
┌──────────────────────────────────────────────────────────┐
│ [lupa] ¿QUÉ ATAQUE ES?                     ⏱ 20  ▓▓▓▓▓▓▓ │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │  SITUACIÓN (texto de 2 a 3 líneas, 52 px)          │  │
│  │  con una ilustración simple a la izquierda         │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐   │
│  │  PHISHING     │ │  RANSOMWARE   │ │ FUERZA BRUTA  │   │
│  └───────────────┘ └───────────────┘ └───────────────┘   │
└──────────────────────────────────────────────────────────┘
```

- **Tres botones** de igual tamaño (unos 560×160 px) con el nombre del ataque.
- La **posición de la correcta cambia** en cada reto.
- La ilustración es un ícono grande (sobre, candado, ordenador bloqueado).

#### Pantalla de resultado (S4)

- La respuesta correcta se ilumina en verde; si eligió otra, esa queda en rojo.
- Aparece una **tarjeta de definición** con el nombre del ataque en grande y una explicación de una línea.
- Debajo, una línea **«Cómo protegerte»**.
- SIGUIENTE.

#### Retos (mínimo 6; cada uno muestra 3 opciones, una correcta)

| # | Situación | Opciones | Correcta | Definición y protección |
|---|---|---|---|---|
| 1 | Recibes un mensaje que parece de tu banco y te pide entrar a un enlace para «confirmar tus datos». | Phishing · Ransomware · Fuerza bruta | **Phishing** | Engaño para que entregues tu información. Verifica el remitente y entra por la página oficial. |
| 2 | Un programa intenta miles de contraseñas distintas hasta dar con la de una cuenta. | Fuerza bruta · Phishing · Suplantación | **Fuerza bruta** | Probar combinaciones hasta acertar. Usa contraseñas largas y activa la verificación en dos pasos. |
| 3 | Todos tus archivos aparecen bloqueados y te piden dinero para recuperarlos. | Ransomware · Phishing · Fuerza bruta | **Ransomware** | Secuestra tus archivos a cambio de dinero. Haz copias de seguridad y no abras archivos dudosos. |
| 4 | Alguien crea un perfil con tu nombre y tu foto y escribe a tus amigos pidiendo dinero. | Suplantación de identidad · Ransomware · Fuerza bruta | **Suplantación de identidad** | Alguien se hace pasar por ti o por otra persona. Avisa a tus contactos y reporta el perfil. |
| 5 | Descargas un juego «gratis» de una página rara y desde entonces tu computadora va lenta y muestra anuncios extraños. | Malware · Fuerza bruta · Phishing | **Malware** | Software malicioso escondido en programas. Descarga solo de tiendas oficiales y mantén un antivirus. |
| 6 | Conectas tu celular a un wifi público «gratis» y alguien intercepta lo que envías. | Ataque en red pública · Ransomware · Suplantación | **Ataque en red pública** | Las redes abiertas pueden ser vigiladas. Evita ingresar a cuentas importantes en ellas o usa datos móviles. |

---

### 8.5 Verdadero o falso

**Idea:** una afirmación corta y dos botones. Es la dinámica más rápida, perfecta para reponer ritmo.

**Tiempo:** 12 s.

#### Pantalla de reto (S3)

```
┌──────────────────────────────────────────────────────────┐
│ [check/x] VERDADERO O FALSO                ⏱ 12  ▓▓▓▓▓▓▓ │
├──────────────────────────────────────────────────────────┤
│                                                          │
│     «Si tengo antivirus, no necesito                     │
│      tener más cuidado.»                                 │
│           (frase enorme, 64 px, centrada)                │
│                                                          │
│   ┌────────────────────┐   ┌────────────────────┐        │
│   │  ✓  VERDADERO      │   │  ✗  FALSO          │        │
│   └────────────────────┘   └────────────────────┘        │
└──────────────────────────────────────────────────────────┘
```

- Frase muy grande y centrada, sin imágenes que distraigan.
- Dos botones enormes (azul para verdadero y rojo suave para falso, siempre con ícono y palabra).

#### Pantalla de resultado (S4)

- La frase queda arriba, con una etiqueta grande al lado: **«VERDADERO»** o **«FALSO»** (la respuesta correcta).
- Banner de acierto o fallo.
- Explicación de 2 líneas.
- Un **dato curioso o consejo** breve opcional en una línea.

#### Retos (mínimo 6)

| # | Afirmación | Respuesta | Explicación |
|---|---|---|---|
| 1 | «Si tengo antivirus, no necesito tener más cuidado.» | **Falso** | El antivirus ayuda, pero no detiene un engaño: el error humano sigue siendo el mayor riesgo. |
| 2 | «Puedo entrar a mi banco desde un wifi público sin problema.» | **Falso** | Las redes abiertas pueden ser vigiladas. Mejor usa tus datos móviles. |
| 3 | «La verificación en dos pasos agrega una capa extra de protección.» | **Verdadero** | Aunque alguien tenga tu clave, también necesitaría tu segundo paso. |
| 4 | «Puedo usar la misma contraseña en todas mis cuentas si es muy larga.» | **Falso** | Si se filtra en un sitio, quedan expuestas todas tus cuentas. |
| 5 | «Los mensajes con faltas de ortografía y mucha urgencia suelen ser sospechosos.» | **Verdadero** | Los estafadores buscan que actúes sin pensar. Detente y verifica. |
| 6 | «Un candado en la barra del navegador significa que la página es 100% confiable.» | **Falso** | Significa que la conexión está cifrada, pero una página falsa también puede tenerlo. |
| 7 | «Mantener mis aplicaciones actualizadas ayuda a cerrar puertas a los atacantes.» | **Verdadero** | Las actualizaciones corrigen fallos de seguridad conocidos. |

---

### 8.6 ¡Hackeado!

**Idea:** la casilla sorpresa. Durante unos segundos, la pantalla simula un ataque (de forma inofensiva y evidentemente de broma) y luego se revela que era una simulación. Es lo que la gente comentará después.

**Tiempo:** 3 s de efecto + 12 s de explicación. **No hay respuesta que dar**, así que esta casilla es automática y siempre «pasa».

#### Reglas de seguridad de esta casilla

- **Es solo una pantalla.** No bloquea el navegador, no usa `alert()` en bucle, no impide cerrar la ventana, no toca archivos ni red.
- La tecla **`R`** y **`Esc`** siempre reinician (para el operador).
- Debe verse **claramente como simulación** después de 3 s.
- El texto no debe parecer una amenaza real contra una marca o entidad existente.

#### Secuencia de pantallas

**Pantalla A: alarma (0 a 3 s)**

```
┌──────────────────────────────────────────────────────────┐
│ (cabecera y pie ocultos; fondo casi negro con ruido)     │
│                                                          │
│     ⚠  TUS ARCHIVOS HAN SIDO CIFRADOS  ⚠                 │
│        (rojo, monoespaciada, 80 px, parpadea)            │
│                                                          │
│     Tienes 00:00:03 para pagar o se perderán             │
│                                                          │
│     ████████████░░░░░░░░ cifrando… 63%                   │
└──────────────────────────────────────────────────────────┘
```

- Fondo `#0A0A0A`, texto en rojo `#FF4D4D`, tipografía monoespaciada.
- **Cuenta regresiva** que llega a 0 en 3 s, barra de «cifrando» que avanza rápido.
- Efecto leve de temblor o glitch (CSS, máximo 3 s).
- Sin sonido, o un único pitido corto si el sonido está activado.

**Pantalla B: revelación (3 a 5 s)**

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│                  [escudo azul grande]                    │
│                                                          │
│            TRANQUILO. ES UNA SIMULACIÓN.                 │
│       Esto le pasa a miles de personas cada día.         │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

- La pantalla pasa de rojo a azul con una transición suave de 0.5 s.
- Título enorme (96 px) en blanco y subtítulo en celeste.

**Pantalla C: explicación (5 a 17 s)**

- Título: **«¿Qué acabas de ver?»**
- Nombre del ataque en grande (según la variante).
- **Tres consejos** de «qué hacer», con ícono, en tarjetas horizontales.
- Botón **SIGUIENTE**.

#### Variantes (mínimo 3, para que no se repita)

| # | Efecto de la pantalla A | Ataque que se explica | 3 consejos |
|---|---|---|---|
| 1 | «Tus archivos han sido cifrados» con cuenta regresiva | **Ransomware** | ① Haz copias de seguridad. ② No abras archivos ni enlaces dudosos. ③ No pagues: avisa a soporte. |
| 2 | Ventana falsa «¡Virus detectado! Llama ya al número…» con alerta roja | **Estafa de falso soporte técnico** | ① Cierra la ventana; no llames. ② Ningún navegador te pide llamar por un virus. ③ Si dudas, apaga y vuelve a abrir. |
| 3 | Pantalla de «Tu cuenta fue bloqueada. Ingresa tus datos para recuperarla» con formulario falso | **Phishing / robo de cuenta** | ① No escribas tus datos en páginas a las que llegaste por un aviso. ② Entra siempre por la dirección oficial. ③ Activa la verificación en dos pasos. |

---

## 9. Pantalla de premio

```
┌──────────────────────────────────────────────────────────┐
│ CABECERA                                                 │
├──────────────────────────────────────────────────────────┤
│                                                          │
│           [escudo con check, animación suave]            │
│                                                          │
│        ¡LISTO! RECOGE TU PREMIO                          │
│        Y TU TARJETA DE CONSEJOS                          │
│                                                          │
│   ┌────────────────────────────────────────────────┐     │
│   │  CONSEJO DEL DÍA (aleatorio, 44 px)            │     │
│   └────────────────────────────────────────────────┘     │
│                                                          │
│        [ VOLVER A GIRAR ]  (para el siguiente)           │
└──────────────────────────────────────────────────────────┘
```

- Mensaje principal enorme, con **doble opción de cierre:** un clic en el botón o **regreso automático a S0 en 8 a 10 s**.
- Si **acertó**, puede mostrarse una línea extra: **«¡Premio extra por acertar!»**. Si no, simplemente el mensaje normal: **nadie debe sentirse mal**.
- El **consejo** se elige al azar de la lista de las 5 recomendaciones de la tarjeta física.

**Lista de consejos para esta pantalla (los mismos de la tarjeta impresa):**

1. No compartas tus contraseñas.
2. Evita abrir enlaces o archivos sospechosos.
3. Activa la autenticación en dos pasos.
4. No entregues información personal a desconocidos ni en páginas que no hayas verificado.
5. Mantén tus dispositivos y aplicaciones actualizados.

---

## 10. Componentes comunes

### 10.1 Cronómetro

- Barra horizontal de ancho completo en la parte superior del reto, 18 px de alto, esquinas redondeadas.
- Número grande a la derecha (48 px).
- Colores: azul (más de 7 s) → amarillo (7 a 4 s) → rojo (3 a 0 s).
- **Al llegar a 0:** el reto se cuenta como fallo y se pasa a S4 con el mensaje **«Se acabó el tiempo. Mira la respuesta:»** (sin tono de regaño).
- **El cronómetro se detiene** al responder.
- En la casilla «¡Hackeado!» no hay cronómetro.

### 10.2 Banner de resultado

| Situación | Texto | Color | Ícono |
|---|---|---|---|
| Acierto | «¡Correcto!» | Verde | ✓ |
| Fallo | «Casi. Mira por qué:» | Rojo suave | ✗ |
| Tiempo agotado | «Se acabó el tiempo. Mira la respuesta:» | Amarillo | ⏱ |

Ocupa todo el ancho, 100 a 120 px de alto, con el texto a 56 px.

### 10.3 Botones

- Alto mínimo **120 px**, ancho mínimo **420 px**.
- Texto de 44 a 56 px en negrita, con ícono.
- Estados: normal, `hover` (más brillo y glow), `active` (se hunde 2 a 3 px), `disabled` (opaco 40%).
- Botón principal de avance (**SIGUIENTE**, **GIRAR**) en azul `#2D8CFF`; secundarios en contorno.

### 10.4 Transiciones

- Cambio de estado con **fundido de 250 a 350 ms**.
- Nada de animaciones largas ni que mareen.
- Respetar `prefers-reduced-motion` reduciendo efectos si está activado.

---

## 11. Estructura de datos de los retos

Todo el contenido vive en un objeto al inicio del script, separado de la lógica. Ejemplo:

```js
const CASILLAS = [
  { id: "real_fraude",  nombre: "¿Real o fraude?",      color: "#2D8CFF", tiempo: 15 },
  { id: "alerta",       nombre: "Encuentra la alerta",  color: "#1F6FD1", tiempo: 20 },
  { id: "password",     nombre: "Contraseña fuerte",    color: "#3FB8E8", tiempo: 15 },
  { id: "ataque",       nombre: "¿Qué ataque es?",      color: "#5B6BFF", tiempo: 20 },
  { id: "vf",           nombre: "Verdadero o falso",    color: "#2AA6A0", tiempo: 12 },
  { id: "hackeado",     nombre: "¡Hackeado!",           color: "#FF4D4D", tiempo: 0  }
];

const RETOS = {
  real_fraude: [
    {
      formato: "correo",                         // correo | whatsapp | sms | notificacion
      de: "soporte@banc0-seguro.example",
      asunto: "URGENTE: tu cuenta será suspendida en 24 horas",
      cuerpo: "Ingresa aquí para verificar tus datos.",
      correcta: "fraude",                        // "real" | "fraude"
      señales: [
        "Dominio mal escrito (cero en lugar de «o»)",
        "Presión de tiempo",
        "Pide datos por un enlace"
      ]
    }
    // ...más retos
  ],

  alerta: [
    {
      de: "seguridad@cuenta-verifica.example",
      asunto: "Acción inmediata requerida",
      cuerpo: "Haz clic en el botón antes de que tu cuenta sea cerrada.",
      boton: "VERIFICAR AHORA",
      zonas: [
        { id: "de",     texto: "seguridad@cuenta-verifica.example", esSenal: true,  motivo: "Remitente que no es de la empresa" },
        { id: "asunto", texto: "Acción inmediata requerida",        esSenal: true,  motivo: "Lenguaje de urgencia" },
        { id: "saludo", texto: "Estimado usuario",                  esSenal: false, motivo: "Genérico, pero común" },
        { id: "boton",  texto: "VERIFICAR AHORA",                   esSenal: true,  motivo: "Enlace con números en lugar de nombre" }
      ]
    }
  ],

  password: [
    {
      opciones: ["123456", "Sofia2005", "Cielo!Verde-Tren#58"],
      correcta: 2,
      tiempos: ["Al instante", "Unas horas", "Cientos de años"],
      explicacion: "Larga, mezcla palabras, símbolos y números, y no usa datos personales."
    }
  ],

  ataque: [
    {
      situacion: "Recibes un mensaje que parece de tu banco y te pide entrar a un enlace para «confirmar tus datos».",
      opciones: ["Phishing", "Ransomware", "Fuerza bruta"],
      correcta: 0,
      definicion: "Engaño para que entregues tu información.",
      proteccion: "Verifica el remitente y entra por la página oficial."
    }
  ],

  vf: [
    {
      frase: "Si tengo antivirus, no necesito tener más cuidado.",
      correcta: false,
      explicacion: "El antivirus ayuda, pero no detiene un engaño: el error humano sigue siendo el mayor riesgo."
    }
  ],

  hackeado: [
    { variante: "ransomware", ataque: "Ransomware", consejos: ["...", "...", "..."] }
  ]
};
```

**Reglas de selección:**

- Los retos de cada casilla salen **al azar sin repetirse** hasta agotar todos; luego se reinicia la lista.
- La casilla **no se repite dos veces seguidas** en la ruleta.
- Las **opciones** (contraseñas, ataques) se barajan para que la correcta no esté siempre en el mismo lugar.
- Todo el texto del contenido se escribe en el arreglo, **nunca dentro del HTML fijo**.

---

## 12. Controles del operador y robustez

| Tecla | Acción |
|---|---|
| `F11` | Pantalla completa del navegador |
| `R` o `Esc` | Reiniciar a la pantalla de inicio |
| `M` | Activar o silenciar sonido (apagado por defecto) |
| `N` | Saltar al siguiente estado (por si alguien se atasca) |
| `O` | Mostrar u ocultar un **panel del operador** con contadores (participantes, aciertos) y botón para reiniciar el contador del día |

**Robustez:**

- **Reinicio automático** a S0 tras **45 s sin clics** en cualquier estado.
- **Ocultar el cursor** tras 5 s sin movimiento y mostrarlo al mover el mouse.
- **Bloquear la selección de texto** y el menú contextual para evitar acciones accidentales.
- **Bloqueo de 400 ms** tras cada cambio de pantalla.
- **Manejo de errores:** si el script falla, mostrar un mensaje grande «Reiniciando…» y recargar la página a los 3 s.
- **Sin dependencias externas:** abrir el archivo localmente funciona. No hace falta servidor.
- **Contador del día** en `localStorage` con `try/catch`; si falla, la página sigue funcionando.
- **No guardar** nombres, respuestas individuales ni nada que identifique a un visitante.

---

## 13. Accesibilidad y legibilidad a distancia

- **Contraste mínimo 4.5:1** en todo texto (el blanco sobre `#0A1428` y `#12213F` lo cumple con holgura).
- **Estado nunca solo por color:** acierto y fallo llevan palabra e ícono.
- **Elementos clicables reales** (`<button>`), no `div` con clic, para que el teclado y el foco funcionen.
- **Foco visible** (contorno celeste de 4 px) por si el operador usa el teclado.
- **`prefers-reduced-motion`:** reducir animaciones si está activado.
- **`lang="es"`** en el HTML.
- **Daltonismo:** los pares verde/rojo siempre acompañados de ✓ y ✗, y con diferente luminosidad.
- **Probar a 2 y 3 metros de distancia** con la pantalla real. Lo que se lee bien a 40 cm en la laptop puede ser ilegible en el monitor.

---

## 14. Lista de pruebas antes del evento

**En la pantalla real (24", pantalla completa):**

- [ ] Se ve completa, sin barras de scroll ni elementos cortados.
- [ ] Todos los textos se leen desde 2 m.
- [ ] El brillo y contraste se ven bien con la iluminación del lugar.

**Funcionamiento:**

- [ ] La ruleta cae en las 6 casillas (probar muchas veces) y nunca se queda trabada.
- [ ] La misma casilla no sale dos veces seguidas.
- [ ] Cada casilla muestra al menos 4 retos distintos sin repetirse seguido.
- [ ] El cronómetro llega a 0 y pasa al resultado correctamente.
- [ ] Un doble clic accidental no se salta pantallas.
- [ ] «¡Hackeado!» se ve bien y `R`/`Esc` lo interrumpe.
- [ ] El reinicio automático por inactividad funciona.
- [ ] Los contadores suben bien y se pueden reiniciar.

**Condiciones del stand:**

- [ ] Funciona **sin internet**.
- [ ] Funciona abriendo el archivo directamente, sin servidor.
- [ ] Funciona después de cerrar y abrir el navegador.
- [ ] El plan B (tarjetas impresas) está listo si la pantalla falla.

**Contenido:**

- [ ] Todos los correos, números, dominios y contraseñas son ficticios.
- [ ] No aparecen marcas bancarias reales ni datos de personas reales.
- [ ] Un integrante del equipo revisó la ortografía y que las respuestas sean correctas.

---

## 15. Prompt listo para el agente de programación

> Construye un único archivo `index.html` (HTML, CSS y JavaScript vanilla, sin librerías, sin CDN, sin fuentes externas y sin conexión a internet) para una pantalla de 24" a 1920×1080 en pantalla completa, usada solo con mouse en un stand de ciberseguridad. Es una **ruleta con 6 casillas**: «¿Real o fraude?», «Encuentra la alerta», «Contraseña fuerte», «¿Qué ataque es?», «Verdadero o falso» y «¡Hackeado!».
>
> **Flujo:** S0 inicio (ruleta + botón GIRAR) → S1 girando (3.8 s, `conic-gradient` + `transform: rotate` con `cubic-bezier`) → S2 casilla revelada (1.5 s) → S3 reto con cronómetro (12 a 20 s según casilla) → S4 resultado con banner (acierto/fallo/tiempo) y explicación → S5 premio (8 a 10 s) → vuelve a S0. Tras 45 s sin clics, reinicio automático. Bloqueo de clics de 400 ms entre estados.
>
> **Diseño:** fondo `#0A1428`, paneles `#12213F`, azul `#2D8CFF`, celeste `#6FD3FF`, acierto `#2ECC71`, alerta `#FF4D4D`, advertencia `#FFC83D`, texto blanco. Esquinas de 20 a 28 px, bordes celestes con glow, fondo con cuadrícula tenue y escudo semitransparente. Textos nunca menores de 28 px; enunciados de 52 a 64 px; botones de mínimo 120 px de alto con texto de 44 a 56 px. Cabecera con nombre del stand y contador del día, pie con el lema «Piensa antes de hacer clic. Protege antes de lamentar.»
>
> **Dinámicas:**
> 1. ¿Real o fraude?: tarjeta que imita correo, WhatsApp o SMS ficticio y dos botones REAL / FRAUDE. En el resultado, se resaltan y numeran las señales sospechosas.
> 2. Encuentra la alerta: correo grande con 4 o 5 zonas clicables (mínimo 80 px de alto). Acierta quien hace clic en una señal real. El resultado marca todas las señales con su motivo.
> 3. Contraseña fuerte: tres tarjetas grandes con contraseñas ficticias (fuente monoespaciada). El resultado muestra una barra de fortaleza y un «tiempo estimado ilustrativo» bajo cada una.
> 4. ¿Qué ataque es?: situación breve y tres botones de ataques. El resultado muestra definición y cómo protegerse.
> 5. Verdadero o falso: frase enorme y dos botones. El resultado muestra la respuesta y una explicación de 2 líneas.
> 6. ¡Hackeado!: pantalla negra con texto rojo de «archivos cifrados» y cuenta regresiva de 3 s (simulación inofensiva, sin bloquear el navegador); luego pantalla azul «Tranquilo, es una simulación» y una explicación con 3 consejos. Tiene 3 variantes (ransomware, falso soporte técnico, cuenta bloqueada).
>
> **Datos:** todos los retos están en un objeto `RETOS` al inicio del script (mínimo 5 por casilla), con selección al azar sin repetir hasta agotar, opciones barajadas y sin repetir la misma casilla dos veces seguidas. Todo el contenido es ficticio.
>
> **Operador:** `R`/`Esc` reinicia, `M` sonido (apagado por defecto), `N` salta de estado, `O` panel con contadores. Ocultar cursor tras 5 s sin movimiento. Contador del día en `localStorage` dentro de try/catch. Respetar `prefers-reduced-motion`. Usar `<button>` reales y `lang="es"`. Si ocurre un error de script, mostrar «Reiniciando…» y recargar.
>
> Usa los textos de retos del documento «Gira y Detecta: especificación detallada» como contenido inicial.
