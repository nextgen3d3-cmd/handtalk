# HandTalk

App que recibe por Bluetooth las palabras del guante traductor de lenguaje de señas,
las muestra en pantalla y las lee en voz alta.

- **Dirección pública:** https://handtalk.pages.dev (página de presentación) y https://handtalk.pages.dev/app/ (la app)
- **Publicación:** Cloudflare Pages conectado a un repositorio de GitHub. Carpeta publicada: `web`.

---

## Estructura

```
HandTalk/
├─ web/                        Lo que se publica en internet
│  ├─ index.html               Página de presentación (el enlace que se comparte)
│  ├─ capturas/                Capturas de la app que se ven en la página
│  └─ app/                     La app en sí
│     ├─ index.html            Todo: diseño, pantallas y lógica
│     ├─ manifest.webmanifest  Nombre, colores e ícono para instalarla
│     ├─ sw.js                 Guarda la app para que abra sin internet
│     ├─ icons/                Íconos de la app
│     └─ img/                  Logos (copias de IMAGENES)
├─ IMAGENES/                   Logos originales
└─ LEEME.md
```

Para cambiar algo de la app se edita **un solo archivo**: `web/app/index.html`.

Funciones y valores principales dentro de `web/app/index.html`:

| Nombre | Qué hace |
|---|---|
| `handleWord()` | Muestra una palabra recibida, la agrega a la frase y al historial, y la lee en voz alta |
| `feed()` | Junta el texto que llega del guante y lo separa por saltos de línea |
| `FLUSH_MS` | Si llega texto sin salto de línea, se muestra después de esta pausa (400 ms) |
| `connectSerial()` | Conexión por Bluetooth clásico (ESP32) o por cable USB |
| `connectBle()` | Conexión por Bluetooth LE (ESP32-C3), servicio Nordic UART |
| `speak()` | Lee un texto en voz alta en español |
| `DEMO_WORDS` | Palabras que se simulan en el modo demostración |
| `startDemo()` | Arranca el modo demostración (también con `app/?demo` en la dirección) |

## Lo que tiene que enviar el guante

Cada palabra terminada en salto de línea:

```
SerialBT.println("HOLA");
```

## Requisitos para conectarse

- Android con Google Chrome 138 o más reciente.
- El guante debe estar **vinculado** en Ajustes → Bluetooth antes de conectarlo desde la app.
- En iPhone la app abre pero no puede usar Bluetooth (limitación de Apple); solo sirve el modo demostración.

## Cómo se actualiza la página publicada

Después de cambiar cualquier archivo dentro de `web/`, desde la carpeta `HandTalk`:

```
git add .
```

```
git commit -m "Describe aquí el cambio"
```

```
git push
```

Cloudflare publica la versión nueva sola en uno o dos minutos.

Si se cambia algo de la app, conviene subir el número de versión en `web/app/sw.js`
(`handtalk-v2` → `handtalk-v3`) para que los teléfonos que la tienen instalada descarguen la nueva.
