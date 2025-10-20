# Proyecto: Monitor de Temperatura y Humedad con ESP32 y ThingSpeak
*(Versión 1.0)*

## Descripción
Este proyecto implementa un sistema de monitoreo de ambiente utilizando una placa ESP32, sensor DHT22 y una pantalla OLED.  
El dispositivo se conecta vía WiFi, lee temperatura y humedad a intervalos definidos, lo muestra en la pantalla OLED y envía los datos a la plataforma ThingSpeak para almacenamiento y visualización en la nube.  
Ideal para aplicaciones IoT (Internet de las Cosas) domésticas o de laboratorio.

## Características principales
- Lectura de temperatura (°C) y humedad (%) mediante DHT22  
- Pantalla OLED (128×64 píxeles) para mostrar estado y valores en tiempo real  
- Conectividad WiFi para envío de datos a ThingSpeak  
- Envío automático de datos cada 30 segundos (configurable)  
- Manejo de errores: detección de lectura inválida del sensor, estado de conexión WiFi, entrega a la nube  
- Código totalmente basado en Arduino IDE (o compatible)

## Componentes necesarios
### Hardware
- Placa ESP32 (por ejemplo, modelo DevKit)
- Sensor DHT22 (digital temperatura + humedad)
- Pantalla OLED 128×64 (controlada vía SPI)
- Módulo protoboard + cables de conexión
- Alimentación USB (5 V) para la ESP32

### Software / Librerías
```cpp
#include <WiFi.h>
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include "DHT22.h"
#include "ThingSpeak.h"
```

## Conexiones de hardware
| Componente | Pin en ESP32 | Notas |
|-------------|--------------|-------|
| DHT22 sensor de datos | GPIO 21 | Data del DHT22 |
| DHT22 alimentación | 3.3 V / 5 V | Alimentación |
| DHT22 GND | GND | Tierra |
| OLED MOSI | GPIO 23 | SPI MOSI |
| OLED CLK | GPIO 18 | SPI CLK |
| OLED DC | GPIO 16 | Data/Command |
| OLED RESET | GPIO 17 | Reset |
| OLED CS | GPIO 5 | Chip Select |
| OLED alimentación | 3.3 V / 5 V | Alimentación |
| OLED GND | GND | Tierra |

## Procedimiento
1. Conecta los componentes según la tabla anterior.
2. Abre Arduino IDE y selecciona la placa ESP32 adecuada.
3. Instala las librerías necesarias desde el Gestor de Librerías.
4. Copia el código del proyecto y modifica tus credenciales WiFi y API Key.
5. Carga el código a la ESP32.
6. Observa la conexión en el monitor serie (115200 bps).
7. Verifica los datos en tu canal de ThingSpeak.

## Parámetros configurables
- `ssid` y `password`: red WiFi local.
- `myChannelNumber` y `myWriteAPIKey`: credenciales de ThingSpeak.
- `timerDelay`: intervalo de actualización (por defecto 30 s).

## Integración con ThingSpeak
1. Crea cuenta en [ThingSpeak](https://thingspeak.com).
2. Crea un canal con dos campos: **Temperatura** y **Humedad**.
3. Copia el número del canal y la API Key de escritura.
4. Configura estos valores en el código.
5. Verifica los gráficos de datos en la web.

## Solución de problemas comunes
| Problema | Causa probable | Solución |
|-----------|----------------|-----------|
| No conecta WiFi | Credenciales incorrectas | Revisar SSID y contraseña |
| OLED sin imagen | Conexiones SPI incorrectas | Revisar pines MOSI, CLK, DC, RESET, CS |
| Lecturas NAN | Sensor sin iniciar o desconectado | Revisar cableado y esperar 2 s antes de lectura |
| Sin datos en ThingSpeak | Canal o API incorrecta | Verificar credenciales y conexión |

## Próximas mejoras
- Promedio de lecturas.
- Alarma por umbrales.
- Servidor web local con ESP32.
- Energía solar o batería.

## Licencia
Proyecto bajo licencia **MIT**.

## Autor
Yuliana Díaz  
Docente: [Nombre del profesor/a]  
Asignatura: *Computación en la Nube*  
Institución: [Nombre de la universidad]
