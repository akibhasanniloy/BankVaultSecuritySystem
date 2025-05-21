# 🖥️ ESP32 OLED Display Example

This project demonstrates how to interface an **I2C OLED display** (SSD1306 128x64) with an **ESP32**, and print a simple message: `"Hello"`.

---

## 🛒 Purchase Link

- [Buy SSD1306 OLED Display (128x64)](https://www.amazon.com/s?k=ssd1306+oled+display)

---

## 📌 OLED Pin Configuration

| OLED Pin | Description      |
|----------|------------------|
| VCC      | Power (3.3V or 5V) |
| GND      | Ground           |
| SDA      | I2C Data         |
| SCL      | I2C Clock        |

---

## 🔌 ESP32 to OLED Connection

| OLED Pin | ESP32 Pin |
|----------|-----------|
| VCC      | 3.3V      |
| GND      | GND       |
| SDA      | GPIO 21   |
| SCL      | GPIO 22   |

---

## 🖼️ Circuit Image

![OLED with ESP32 pinout](./assets/OLED%20Display%20with%20esp32%20Connection.png)

---

## 🧪 Test Code

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET     -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  Serial.begin(115200);
  
  // Initialize the OLED display
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }
  
  // Clear the buffer
  display.clearDisplay();
  
  // Display "Hello"
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE); 
  display.setCursor(30, 20);          
  display.println(F("Hello!"));
  
  display.display();
  delay(2000);
}

void loop() {
}
```

---
🖼️ OLED Display Sample "Hello"
![OLED with ESP32](./assets/OLED%20test%20snapshot.jpg)
