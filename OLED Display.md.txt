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

![OLED with ESP32](assets/OLED Display with Esp32.png)

---

## 🧪 Test Code

See [`src/main.ino`](src/main.ino) for the complete Arduino sketch.

---

## 🔧 #define Section

```cpp
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1  // No reset pin used
#define I2C_ADDRESS   0x3C
