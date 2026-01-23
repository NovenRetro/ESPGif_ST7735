# ESPGif 🎞️ (ESP32 + ST7735 + SD/SPIFFS) — by NovenRetro

**ESPGif** es un firmware para **ESP32** que reproduce **GIFs animados** en una pantalla **ST7735 1.8" (128x160)**, permitiendo administrar los GIFs desde una **web** alojada en el propio ESP32 (subir, listar, reproducir y borrar).  
Soporta almacenamiento en **microSD (recomendado)** y fallback automático a **SPIFFS** si la SD no monta.
Incluye además un **Modo Avanzado** para **calibrar la pantalla** (offset X/Y, rotación, swap de colores R/B y perfil TAB), guardado en memoria **NVS (Preferences)**.

## ✅ Features

- Reproducción de **GIFs animados** en ST7735 usando **AnimatedGIF**
- Render optimizado con **“runs opacos”** (mejor rendimiento con transparencias)
- **Almacenamiento externo (microSD)** y **fallback** a interno (**SPIFFS**)
- Web UI mobile-friendly (estilo NovenRetro):
  - Subir GIF (multipart)
  - Listar GIFs
  - Reproducir un GIF
  - Eliminar un GIF
  - “Reproduciendo ahora” con refresco automático
- **mDNS**: acceso por `http://espgif.local`
- **Improv Wi-Fi Serial** (compatible con **ESP Web Tools**) para configurar Wi-Fi sin recompilar
- **Modo Avanzado de Pantalla** (`/advanced`):
  - Offset X/Y (setColRowStart)
  - Rotación (0..3)
  - Swap R/B (corrección de color)
  - Perfil TAB (Green/Black/Red) con persistencia
  - Guardar configuración en **NVS** y reiniciar si hace falta

## 🧩 Hardware soportado

- **ESP32 DevKit** (u otro ESP32 compatible)
- Pantalla **ST7735 1.8" 128x160** (módulo rojo común)
- Lector **microSD** (en tu caso, integrado en la misma placa con la pantalla)

> Nota: muchos módulos ST7735 “combo” comparten SPI entre pantalla y SD. Este firmware contempla el bus compartido controlando CS.

## 🔌 Conexiones (pinout)
Este firmware asume **SPI compartido** para TFT y SD:

### SPI (compartido)
- `SCLK` → GPIO **18**
- `MOSI` → GPIO **23**
- `MISO` → GPIO **19** (solo SD)

### TFT ST7735
- `TFT_CS`  → GPIO **5**
- `TFT_DC`  → GPIO **16**
- `TFT_RST` → GPIO **17**
- `VCC`     → **3.3V**
- `GND`     → GND
- Backlight/LED → **3.3V** (directo)

### microSD
- `SD_CS`   → GPIO **4**
- `VCC`     → **3.3V**
- `GND`     → GND

📌 Pines definidos en el código:
#define SD_CS    4
#define SD_MOSI  23
#define SD_MISO  19
#define SD_SCLK  18

#define TFT_CS   5
#define TFT_DC   16
#define TFT_RST  17
#define TFT_SCLK 18
#define TFT_MOSI 23

📁 Estructura de archivos en el almacenamiento
Todos los GIFs se guardan en:
/gifs/
El firmware intenta reproducir:

/gifs/idle.gif como “idle” inicial
Si no existe, muestra una pantalla fallback con texto.

🌐 Interfaz Web
Home

Acceder por:
http://espgif.local (si mDNS funciona)
o por IP local mostrada en pantalla

Permite:
Subir GIF
Ver lista de GIFs
Reproducir uno
Eliminar
Ver el GIF “reproduciendo ahora”

Modo Avanzado (Pantalla)
http://espgif.local/advanced

Permite:
PROBAR (sin guardar): aplica offset/rot/swap en caliente
GUARDAR: persiste en NVS
GUARDAR Y REINICIAR: recomendado si cambiás el perfil TAB
RESTABLECER: borra calibración y reinicia

🔧 Endpoints HTTP (API)
GET / → UI principal
GET /advanced → UI modo avanzado
GET /hello → info del firmware y estado
GET /status → JSON con playing y uptime
GET /list → JSON con lista de archivos en /gifs
POST /play?name=<archivo.gif> → reproduce un GIF
POST /idle → reproduce idle (/gifs/idle.gif)
POST /delete?name=<archivo.gif> → borra un GIF
POST /upload → sube GIF por multipart (FormData)
POST /wifi/reset → borra credenciales Wi-Fi y reinicia

Pantalla (Modo Avanzado)
GET /display/config
POST /display/apply (aplica sin guardar)
POST /display/save (guarda en NVS)

opcional ?reboot=1 para reiniciar
POST /display/reset (borra calibración y reinicia)

🧠 Persistencia (NVS / Preferences)
Namespace: "ESPGif"
Wi-Fi
wifi_ssid
wifi_pass

Pantalla
d_offx (char)
d_offy (char)
d_rot (uchar)
d_tab (uchar)
d_swap (uchar)

📶 Wi-Fi (Improv / ESP Web Tools)
El firmware soporta Improv Wi-Fi Serial:
Si no existen credenciales guardadas, el firmware queda esperando configuración por Improv.
Cuando se recibe SSID/PASS, se guarda en NVS y se conecta.


🎨 Calibración de pantalla (TAB / Offset / Swap)
Dependiendo del módulo ST7735, puede variar:
perfil TAB correcto (GREENTAB, BLACKTAB, REDTAB)
offset de inicio (col/row start)
swap R/B si los colores se ven invertidos
Este firmware resuelve eso desde el Modo Avanzado sin recompilar:
Ajustás visualmente
Probás en vivo
Guardás en NVS

🧯 Troubleshooting
“En SPIFFS anda perfecto pero en SD se cuelga”
Probá otra microSD (muchas SD “raras” fallan en SPI aunque funcionen en PC)
Formateá en FAT32
Evitá SDs muy grandes o exFAT
En módulos combo, una SD mala puede provocar cuelgues/reinicios raros

✅ Caso real: cambiar la SD solucionó el problema completamente.
“No veo espgif.local”
Depende del soporte mDNS del dispositivo/red.
Usá la IP que muestra la pantalla.
“Upload ok pero no aparece en la lista”
Verificá que exista /gifs/
Verificá que el archivo sea .gif (se sanitiza el nombre)
Reintentá con un GIF liviano

📌 Créditos / Librerías
AnimatedGIF (decodificación de GIF)
Adafruit_GFX + Adafruit_ST7735
ESP32 Arduino Core
Improv Wi-Fi (ESP Web Tools friendly)
