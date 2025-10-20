# 🌡️ Proyecto IoT: Monitor de Temperatura y Humedad con ESP32 + OLED + ThingSpeak

![Estado](https://img.shields.io/badge/Estado-Completado-green)
![Plataforma](https://img.shields.io/badge/Arduino-IDE-blue)
![Licencia](https://img.shields.io/badge/Licencia-MIT-yellow)

---

## 🧭 Descripción del Proyecto
Este proyecto implementa un **sistema IoT de monitoreo ambiental** usando una placa **ESP32**, un sensor **DHT22** y una pantalla **OLED (SPI)**.  
El sistema mide temperatura y humedad en tiempo real, las muestra en el display OLED y las envía automáticamente a la nube mediante **ThingSpeak**, donde se visualizan en gráficos interactivos.  


---

## 🎯 Objetivos

- 📶 Conectar el ESP32 a WiFi y enviar lecturas del sensor DHT22 a la nube.
- 💾 Visualizar los datos localmente en una pantalla OLED.
- ☁️ Almacenar y graficar los valores en **ThingSpeak**.
- 🧠 Documentar paso a paso el código y conexiones de hardware.

---

## 🧩 Componentes Utilizados

| Componente | Descripción |
|-------------|--------------|
| 🧠 **ESP32 DevKit v1** | Microcontrolador principal |
| 🌡️ **Sensor DHT22** | Sensor digital de temperatura y humedad |
| 🖥️ **Pantalla OLED 0.96” SPI (SSD1306)** | Visualización local de lecturas |
| 🔌 **Protoboard y cables Dupont** | Conexiones eléctricas |
| ⚡ **Cable USB** | Alimentación y programación |

---

## 🔌 Conexiones de Hardware

| Componente | Pin ESP32 | Función |
|-------------|-----------|----------|
| DHT22 VCC | 3.3V | Alimentación |
| DHT22 GND | GND | Tierra |
| DHT22 DATA | GPIO 21 | Señal |
| OLED MOSI | GPIO 23 | Comunicación SPI |
| OLED CLK | GPIO 18 | Reloj SPI |
| OLED DC | GPIO 16 | Data/Command |
| OLED RESET | GPIO 17 | Reinicio de pantalla |
| OLED CS | GPIO 5 | Chip Select |
| OLED VCC | 3.3V | Alimentación |
| OLED GND | GND | Tierra |

---

## ⚙️ Instalación y Configuración

1. 🧩 Conecta todos los componentes según la tabla anterior.
2. 💻 Abre **Arduino IDE** y selecciona la placa **ESP32 Dev Module**.
3. 📦 Instala las siguientes librerías desde el Gestor de Librerías:
   - `WiFi.h`
   - `SPI.h`
   - `Adafruit_GFX.h`
   - `Adafruit_SSD1306.h`
   - `ThingSpeak.h`
   - `DHT22.h`
4. ⚙️ Configura tus credenciales de WiFi y tu canal de ThingSpeak en el código:
   ```cpp
   const char* ssid = "TuSSID";
   const char* password = "TuPassword";
   unsigned long myChannelNumber = TU_NUMERO_DE_CANAL;
   const char* myWriteAPIKey = "TU_API_KEY";
   ```
5. 🔼 Sube el código al ESP32.
6. 🖥️ Abre el **Monitor Serie** (115200 bps) y observa los mensajes de conexión y lecturas.
7. 📊 Ingresa a **ThingSpeak** y verifica que tus datos se visualizan correctamente.

---

## 💻 Código del Proyecto (con explicación detallada)

```cpp
#include <WiFi.h>                // 📶 Maneja la conexión WiFi
#include <SPI.h>                 // 🔄 Comunicación SPI
#include <Adafruit_GFX.h>        // 🎨 Librería gráfica
#include <Adafruit_SSD1306.h>    // 🖥️ Controlador OLED
#include <DHT22.h>               // 🌡️ Sensor DHT22
#include "ThingSpeak.h"          // ☁️ Envío de datos a la nube

// ===== CONFIGURACIÓN OLED SPI =====
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_MOSI   23
#define OLED_CLK    18
#define OLED_DC     16
#define OLED_RESET  17
#define OLED_CS     5

// Se crea el objeto pantalla OLED
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &SPI, OLED_DC, OLED_RESET, OLED_CS);

// ===== CONFIGURACIÓN DHT22 =====
#define DHTPIN 21
DHT22 dht(DHTPIN);

// ===== CONFIGURACIÓN WiFi =====
const char* ssid = "TuSSID";
const char* password = "TuPassword";

// ===== CONFIGURACIÓN ThingSpeak =====
WiFiClient client;
unsigned long myChannelNumber = TU_NUMERO_DE_CANAL;
const char* myWriteAPIKey = "TU_API_KEY";

// ===== VARIABLES =====
float temperatura = 0.0;
float humedad = 0.0;
unsigned long lastTime = 0;
unsigned long timerDelay = 30000; // ⏱️ Enviar datos cada 30 segundos
```

---

### 🔧 `setup()` — Inicialización del Sistema

```cpp
void setup() {
  Serial.begin(115200);
  Serial.println(F("Iniciando sistema..."));

  // --- Inicializar OLED ---
  if (!display.begin(SSD1306_SWITCHCAPVCC)) {
    Serial.println(F("No se encontró la pantalla OLED!"));
    for (;;);
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10, 25);
  display.println("Iniciando WiFi...");
  display.display();

  // --- Conectar a WiFi ---
  WiFi.begin(ssid, password);
  Serial.print("Conectando a WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nConectado a WiFi!");
  Serial.print("IP asignada: ");
  Serial.println(WiFi.localIP());

  // Mostrar IP en OLED
  display.clearDisplay();
  display.setCursor(10, 20);
  display.println("WiFi Conectado!");
  display.setCursor(10, 35);
  display.println(WiFi.localIP());
  display.display();
  delay(2000);

  // --- Inicializar ThingSpeak ---
  ThingSpeak.begin(client);

  // --- Inicializar DHT22 ---
  delay(2000);
}
```

🔍 **Explicación paso a paso:**
- Inicia comunicación serie para depuración.  
- Inicializa la pantalla OLED y muestra mensajes de estado.  
- Establece la conexión WiFi y muestra la IP asignada.  
- Inicia la librería de ThingSpeak.  
- Espera 2 s para estabilizar el sensor DHT22.  

---

### 🔁 `loop()` — Lectura y Envío Continuo

```cpp
void loop() {
  if ((millis() - lastTime) > timerDelay) {
    // 🌡️ Leer sensor
    temperatura = dht.getTemperature();
    humedad = dht.getHumidity();

    // ⚠️ Validar lectura
    if (isnan(temperatura) || isnan(humedad)) {
      Serial.println("Error al leer DHT22");
      display.clearDisplay();
      display.setCursor(10, 25);
      display.println("Error lectura DHT!");
      display.display();
      delay(2000);
      return;
    }

    // 📟 Mostrar en Serial
    Serial.print("Temperatura: ");
    Serial.print(temperatura);
    Serial.println(" °C");
    Serial.print("Humedad: ");
    Serial.print(humedad);
    Serial.println(" %");

    // 🖥️ Mostrar en OLED
    display.clearDisplay();
    display.setTextColor(SSD1306_WHITE);
    display.setTextSize(1);
    display.setCursor(0, 0);
    display.println("Enviando a nube...");
    display.setTextSize(2);
    display.setCursor(0, 25);
    display.print(temperatura, 1);
    display.print("C ");
    display.print(humedad, 1);
    display.println("%");
    display.display();

    // ☁️ Enviar datos a ThingSpeak
    ThingSpeak.setField(1, temperatura);
    ThingSpeak.setField(2, humedad);

    int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);

    if (x == 200) {
      Serial.println("Datos enviados correctamente a ThingSpeak!");
      display.setTextSize(1);
      display.setCursor(0, 55);
      display.println("Upload OK");
      display.display();
    } else {
      Serial.print("Error al enviar datos. Código HTTP: ");
      Serial.println(x);
      display.setTextSize(1);
      display.setCursor(0, 55);
      display.println("Error upload!");
      display.display();
    }

    lastTime = millis(); // Actualizar temporizador
  }
}
```

📘 **Qué hace este bloque:**
1. Verifica si han pasado 30 s desde la última lectura.  
2. Lee la temperatura y humedad del DHT22.  
3. Valida los datos y los muestra por Serial y OLED.  
4. Envía los valores al canal de ThingSpeak (campo 1 = temperatura, campo 2 = humedad).  
5. Muestra mensajes de éxito o error.  

---

## 🧠 Funcionamiento General

🔹 **Inicio:** el ESP32 se conecta a WiFi y prepara la pantalla.  
🔹 **Ciclo:** cada 30 s se leen los valores del sensor y se envían a ThingSpeak.  
🔹 **Visualización:** tanto en la pantalla OLED como en el dashboard web.  
🔹 **Supervisión:** el monitor serie permite ver el estado del proceso en tiempo real.  

---

## 📸 Evidencias del Proyecto

### 🔧 Montaje en Protoboard
![Montaje 1](WhatsApp%20Image%202025-10-20%20at%206.10.05%20PM.jpeg)
![Montaje 2](WhatsApp%20Image%202025-10-20%20at%206.10.13%20PM.jpeg)
![Montaje 3](WhatsApp%20Image%202025-10-20%20at%206.10.14%20PM.jpeg)

### 🌐 Resultados en ThingSpeak
![Resultados](Captura%20de%20pantalla%202025-10-20%20165716.png)

> Los gráficos muestran el comportamiento dinámico de la **temperatura** y **humedad** en tiempo real.  
> Los indicadores tipo “gauge” permiten visualizar de forma clara las variaciones de los valores medidos.

---

## 🧩 Problemas Comunes y Soluciones

| Problema | Causa | Solución |
|-----------|--------|-----------|
| ❌ No conecta a WiFi | Credenciales incorrectas | Revisar SSID y contraseña |
| ⚠️ Pantalla OLED no muestra nada | Pines mal conectados o voltaje inadecuado | Revisar conexiones SPI y nivel de voltaje |
| 🌡️ Lecturas “NaN” | Sensor desconectado o sin tiempo de estabilización | Revisar conexiones del DHT22 y agregar delay |
| ☁️ ThingSpeak no recibe datos | API Key o canal incorrecto | Revisar valores configurados en el código |

---

## 🚀 Posibles Mejoras

- 🕒 Promediar lecturas antes de enviarlas.  
- 🔔 Enviar alertas por Telegram o correo al superar umbrales.  
- 🖥️ Añadir servidor web local con ESP32.  
- 🔋 Implementar alimentación por batería o panel solar.  

---

## 📈 Conclusión
Este proyecto demuestra cómo **IoT y computación en la nube** pueden integrarse para monitorear variables ambientales en tiempo real.  
El **ESP32** actúa como cliente que mide, muestra y publica datos, mientras que **ThingSpeak** gestiona la visualización y almacenamiento en la nube.

---

## 👩‍💻 Autores

**Yuliana Díaz**  
Docente: [Nombre del profesor/a]  
Asignatura: *Computación en la Nube*  
Institución: [Nombre de la universidad]

---

## 📜 Licencia
Proyecto bajo licencia **MIT** — uso libre con atribución.
