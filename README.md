# 🌡️ Sistema IoT de Monitoreo de Temperatura con ESP32 + ThingSpeak  
![Status](https://img.shields.io/badge/status-prototype-yellow)  
![License](https://img.shields.io/badge/license-MIT-blue)

## 🧭 Descripción del Proyecto  
Este proyecto implementa un **sistema IoT completo** que mide temperatura ambiente utilizando un **sensor de temperatura conectado a una placa ESP32**, mostrando los valores por monitor serie y enviando los datos a la nube mediante **ThingSpeak** para su almacenamiento, análisis y visualización gráfica.

El desarrollo se basó en el **tutorial de TodoMaker: “Envío de datos a ThingSpeak usando ESP32”**, y se adapta a los requisitos de la **Actividad 4 del curso de Computación en la Nube**, demostrando integración de hardware, servicios cloud y APIs públicas.

## 🎯 Objetivos del Proyecto  
### Objetivo General  
Desarrollar un sistema IoT con ESP32 que mida temperatura, se conecte a la nube y permita el registro y monitoreo remoto de los datos.

### Objetivos Específicos  
1. Configurar el ESP32 con un sensor de temperatura (DHT22 o DS18B20).  
2. Programar el envío de datos a **ThingSpeak** mediante WiFi.  
3. Visualizar los datos en tiempo real y almacenarlos en la nube.  
4. Integrar una **API pública gratuita** (por ejemplo, OpenWeatherMap) para mostrar la temperatura externa y compararla con la medida local.  
5. Documentar todo el proceso de desarrollo y pruebas.

## 🧩 Componentes y Conexiones  
### Hardware  
- ESP32 DevKit  
- Sensor de temperatura (DHT11, DHT22 o DS18B20)  
- (Opcional) Pantalla OLED SSD1306 (I2C)  
- Cables Dupont y protoboard  
- Fuente de alimentación 5 V  

### Conexión básica (ejemplo DHT22)  
| Componente | Pin ESP32 | Descripción |
|-------------|-----------|-------------|
| DHT22 VCC   | 3.3 V     | Alimentación |
| DHT22 DATA  | GPIO 23   | Señal |
| DHT22 GND   | GND       | Tierra |

## ☁️ Servicios en la Nube Utilizados  
- **ThingSpeak** – Recolección y visualización de datos en tiempo real.  
- (Opcional en ampliación) **Firebase** o **Heroku** – Backend REST para almacenamiento adicional o control remoto.

## 🌐 API Externa Integrada  
- **OpenWeatherMap API**: permite consultar la temperatura y condiciones meteorológicas de una ciudad para compararlas con las lecturas del sensor local.  
  ```
  https://api.openweathermap.org/data/2.5/weather?q=Bogota&appid=TU_API_KEY&units=metric
  ```

## 🛠️ Requisitos de Software y Librerías  
| Librería | Función |
|-----------|---------|
| `WiFi.h` | Conexión WiFi |
| `HTTPClient.h` | Consultas HTTP |
| `ArduinoJson.h` | Parseo de respuestas JSON |
| `DHT.h` o `DallasTemperature.h` | Lectura del sensor |
| `ThingSpeak.h` | Envío de datos a la nube |
| `Adafruit_SSD1306.h` (opcional) | Mostrar datos en pantalla OLED |

## ⚙️ Instalación y Configuración  
1. Clona el repositorio  
   ```bash
   git clone https://github.com/tu-usuario/iot-temperature-esp32.git
   ```
2. Abre el proyecto en Arduino IDE.  
3. Crea el archivo `secrets.h` con tus credenciales:  
   ```cpp
   #define SECRET_WIFI_SSID "TuSSID"
   #define SECRET_WIFI_PASS "TuPassword"
   #define SECRET_CHANNEL_ID 123456
   #define SECRET_WRITE_APIKEY "ABCDEFGHIJKL"
   #define SECRET_API_KEY_OPENWEATHER "tu_api_key_openweather"
   ```
4. Compila y sube el código al ESP32.  
5. Abre el monitor serie (115200 baud).  
6. Accede a ThingSpeak para visualizar el gráfico de temperatura.

## 🧠 Funcionamiento del Sistema  
1. El ESP32 se conecta automáticamente a la red WiFi.  
2. Lee la temperatura del sensor local.  
3. Consulta la temperatura externa desde la API de OpenWeatherMap.  
4. Envía ambas lecturas (local y externa) a **ThingSpeak** cada 60 segundos.  
5. ThingSpeak almacena y grafica los valores.  
6. (Opcional) El display OLED muestra la temperatura y el estado de conexión.  

## 🧩 Estructura del Repositorio  
```
iot-cloud-project/
├── README.md
├── firmware/
│   ├── main.ino
│   ├── config.h
│   └── libraries/
├── cloud-backend/
│   ├── api.py
│   ├── requirements.txt
│   └── deploy/
├── docs/
│   ├── circuit-diagram.png
│   ├── api-documentation.md
│   └── demo-video/
└── tests/
    └── unit-tests/
```

## 🧰 Guía de Uso  
- Enciende el ESP32 y verifica en el monitor serie que se conecte correctamente.  
- Observa el valor de temperatura local y externa.  
- Revisa los gráficos en tu canal de ThingSpeak.  

## 📊 Diagrama de Arquitectura  
[Incluir imagen en `docs/circuit-diagram.png`]

**Flujo:**  
1. Sensor → ESP32  
2. ESP32 → WiFi  
3. WiFi → ThingSpeak  
4. ThingSpeak → Visualización Web  

## 🧩 Problemas Encontrados y Soluciones  
| Problema | Solución |
|-----------|-----------|
| Conexión inestable a WiFi | Implementar reconexión automática |
| Errores HTTP 401 | Revisar API Key |
| Sensor sin lectura | Verificar pines y resistencia |
| Límite API OpenWeather | Ajustar frecuencia de consultas |

## 🚀 Mejoras Futuras  
- Control remoto desde la nube.  
- Visualización en OLED.  
- Alertas automáticas.  
- Integración con Firebase o Telegram.  

## 🧪 Pruebas Realizadas  
- Conectividad WiFi estable.  
- Envío continuo por 24 horas.  
- Verificación de datos en ThingSpeak.  

## 📸 Evidencias  
- Captura del monitor serie.  
- Gráfica de ThingSpeak.  
- Video demostrativo (3-5 min).  

## 📈 Reflexión Técnica  
El proyecto demuestra integración entre **IoT y computación en la nube**, aplicando redes, APIs REST y servicios cloud. El ESP32 actúa como cliente que recolecta y publica datos, mientras **ThingSpeak** gestiona el almacenamiento remoto.

## 📜 Licencia  
Proyecto bajo licencia **MIT**.

## 👥 Autores  
- **[Tu nombre]**  
- **[Compañero/a]**  
Docente: [Nombre del profesor/a]  
Asignatura: *Computación en la Nube*  
Institución: [Nombre de la universidad]

## 🧾 Faltantes Detectados  
❌ Diagrama de conexiones (`docs/circuit-diagram.png`)  
❌ Capturas y video (`docs/demo-video/`)  
❌ Documentación API (`docs/api-documentation.md`)  
❌ Comentarios en `main.ino`  
