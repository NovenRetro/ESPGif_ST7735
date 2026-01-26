ESPGif 🎞️ (ESP32 + ST7735 + SD/SPIFFS) — by NovenRetro

ESPGif es un firmware para ESP32 que reproduce GIFs animados y imágenes JPG/JPEG en una pantalla ST7735 1.8" (128x160), permitiendo administrar el contenido desde una web alojada en el propio ESP32 (subir, listar, reproducir y borrar).

Soporta almacenamiento en microSD (recomendado) y fallback automático a SPIFFS si la SD no monta. Incluye un Modo Avanzado para calibrar la pantalla (offset X/Y, rotación, swap de colores R/B y perfil TAB), con persistencia en NVS (Preferences).

✅ Features 🎞️ Multimedia

Reproducción de GIFs animados (AnimatedGIF)

Reproducción de imágenes JPG / JPEG

Autocentrado automático (GIF y JPG)

Re-escalado automático de JPG:

Mantiene relación de aspecto

Ajusta imágenes grandes a la resolución de la pantalla

Imágenes más pequeñas se centran sin estirarse

Render optimizado con “runs opacos” para GIFs (mejor rendimiento con transparencias)

💾 Almacenamiento

microSD (recomendado)

Fallback automático a SPIFFS si la SD no está presente

Estructura simple basada en /gifs/

🌐 Interfaz Web (mobile-friendly)

Subir GIF o JPG

Listar archivos

Reproducir cualquier media

Eliminar archivos

Indicador de “Reproduciendo ahora” con refresco automático

⚙️ Sistema

mDNS → acceso por http://espgif.local

Improv Wi-Fi Serial (compatible con ESP Web Tools)

Configuración Wi-Fi sin recompilar firmware

🖥️ Modo Avanzado de Pantalla (/advanced)

Offset X/Y (setColRowStart)

Rotación (0..3)

Swap R/B (corrección de color)

Perfil TAB:

Green TAB

Black TAB

Red TAB

Aplicar cambios en vivo

Guardar configuración en NVS

Reinicio opcional desde la UI

🧩 Hardware soportado

ESP32 DevKit (u otro ESP32 compatible)

Pantalla ST7735 1.8" 128×160 (módulo rojo común)

Lector microSD (integrado o externo)

💡 Muchos módulos ST7735 “combo” comparten el bus SPI con la SD. Este firmware maneja correctamente SPI compartido mediante control de CS.

🔌 Conexiones (pinout) SPI (compartido)

SCLK → GPIO 18

MOSI → GPIO 23

MISO → GPIO 19 (solo SD)

TFT ST7735

TFT_CS → GPIO 5

TFT_DC → GPIO 16

TFT_RST → GPIO 17

VCC → 3.3V

GND → GND

Backlight / LED → 3.3V directo

microSD

SD_CS → GPIO 4

VCC → 3.3V

GND → GND

📌 Pines definidos en el código:

#define SD_CS 4 #define SD_MOSI 23 #define SD_MISO 19 #define SD_SCLK 18

#define TFT_CS 5 #define TFT_DC 16 #define TFT_RST 17 #define TFT_SCLK 18 #define TFT_MOSI 23

📁 Estructura de archivos

Todos los archivos se guardan en:

/gifs/

Archivos soportados:

.gif

.jpg

.jpeg

Archivo especial:

/gifs/idle.gif

Se reproduce automáticamente como idle al iniciar el sistema o cuando no hay contenido activo.

Si no existe, se muestra una pantalla fallback con texto.

🌐 Interfaz Web Home

Acceso por:

http://espgif.local

o por la IP local mostrada en pantalla

Permite:

Subir GIF o JPG

Ver lista de archivos

Reproducir cualquier media

Eliminar archivos

Ver qué archivo está en reproducción

Modo Avanzado (Pantalla) http://espgif.local/advanced

Permite:

PROBAR: aplicar ajustes sin guardar

GUARDAR: persiste en NVS

GUARDAR Y REINICIAR: recomendado al cambiar TAB

RESTABLECER: borra calibración y reinicia

🔧 Endpoints HTTP (API) UI

GET / → UI principal

GET /advanced → UI modo avanzado

Estado

GET /hello

GET /status

GET /list

Reproducción

POST /play?name=<archivo.gif|jpg>

POST /idle

POST /delete?name=

POST /upload → subida multipart (GIF/JPG)

Wi-Fi

POST /wifi/reset → borra credenciales y reinicia

Pantalla (Modo Avanzado)

GET /display/config

POST /display/apply

POST /display/save

POST /display/reset

?reboot=1 puede usarse para forzar reinicio.

🧠 Persistencia (NVS / Preferences)

Namespace: ESPGif

Wi-Fi

wifi_ssid

wifi_pass

Pantalla

d_offx

d_offy

d_rot

d_tab

d_swap

🎨 Calibración de pantalla

Cada módulo ST7735 puede requerir:

Perfil TAB distinto

Offset de inicio diferente

Swap R/B

Este firmware permite:

Ajustar visualmente

Ver cambios en vivo

Guardar sin recompilar

Resolver colores incorrectos o desplazamientos

🧯 Troubleshooting

“En SPIFFS anda perfecto pero en SD se cuelga”

Probá otra microSD

Formateá en FAT32

Evitá exFAT o SDs muy grandes

Caso real: cambiar la SD solucionó cuelgues completamente.

“No veo espgif.local”

Depende del soporte mDNS del router/dispositivo

Usá la IP mostrada en pantalla

“JPG se ve mal de color”

Revisá Swap R/B en Modo Avanzado

Ajustá perfil TAB correcto

📌 Créditos / Librerías

AnimatedGIF

JPEGDEC

Adafruit_GFX

Adafruit_ST7735

ESP32 Arduino Core

Improv Wi-Fi / ESP Web Tools

🤝 Contribuir

¡Las contribuciones son bienvenidas!

Fork del proyecto

Rama: feature/tu-mejora

Pull Request con descripción clara

📄 Licencia

MIT License © NovenRetro 2025 — Todos los derechos reservados
