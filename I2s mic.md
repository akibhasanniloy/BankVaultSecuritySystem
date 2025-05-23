# 🎤 I2S Microphone (INMP441)  

The **INMP441** is a high-performance, low-power digital microphone used for **audio input** in many Arduino or ESP32 projects by many students or even professionals.   

---

## 🛒 Purchase Link  

💰 *Buy the INMP441 microphone module on Amazon:*  
[**Purchase Here**](https://www.amazon.com/s?k=INMP441)  

---

## 🔌 INMP441 Pin Configuration  

The **INMP441** uses **I2S communication**, requiring the following connections:  

✔ **VDD** - Power Supply (3.3V - DO NOT USE 5V!)  
✔ **GND** - Ground  
✔ **L/R** - Left/Right channel select (tie to GND for left channel)  
✔ **SCK** - Serial Clock  
✔ **WS** - Word Select (Left/Right clock)  
✔ **SD** - Serial Data  

---

### 📊 INMP441 Pins Connected to ESP32  

| **INMP441 Pins** | **ESP32 Pins** |
|-----------------|--------------|
| **VDD** | 3.3V |
| **GND** | GND |
| **L/R** | GND |
| **SCK** | GPIO 26 |
| **WS** | GPIO 22 |
| **SD** | GPIO 21 |

---

# 📜 Test Code for INMP441

```cpp
#include <driver/i2s.h>

#define SAMPLE_BUFFER_SIZE 512
#define SAMPLE_RATE 8000
#define I2S_MIC_CHANNEL I2S_CHANNEL_FMT_ONLY_LEFT
#define I2S_MIC_SERIAL_CLOCK GPIO_NUM_26
#define I2S_MIC_LEFT_RIGHT_CLOCK GPIO_NUM_22
#define I2S_MIC_SERIAL_DATA GPIO_NUM_21

i2s_config_t i2s_config = {
    .mode = (i2s_mode_t)(I2S_MODE_MASTER | I2S_MODE_RX),
    .sample_rate = SAMPLE_RATE,
    .bits_per_sample = I2S_BITS_PER_SAMPLE_32BIT,
    .channel_format = I2S_CHANNEL_FMT_ONLY_LEFT,
    .communication_format = I2S_COMM_FORMAT_I2S,
    .intr_alloc_flags = ESP_INTR_FLAG_LEVEL1,
    .dma_buf_count = 4,
    .dma_buf_len = 1024,
    .use_apll = false,
    .tx_desc_auto_clear = false,
    .fixed_mclk = 0};

i2s_pin_config_t i2s_mic_pins = {
    .bck_io_num = I2S_MIC_SERIAL_CLOCK,
    .ws_io_num = I2S_MIC_LEFT_RIGHT_CLOCK,
    .data_out_num = I2S_PIN_NO_CHANGE,
    .data_in_num = I2S_MIC_SERIAL_DATA};

void setup() {
  Serial.begin(115200);
  i2s_driver_install(I2S_NUM_0, &i2s_config, 0, NULL);
  i2s_set_pin(I2S_NUM_0, &i2s_mic_pins);
}

int32_t raw_samples[SAMPLE_BUFFER_SIZE];
void loop() {
  size_t bytes_read = 0;
  i2s_read(I2S_NUM_0, raw_samples, sizeof(int32_t)*SAMPLE_BUFFER_SIZE, &bytes_read, portMAX_DELAY);
  int samples_read = bytes_read / sizeof(int32_t);
  for (int i = 0; i < samples_read; i++) {
    Serial.printf("%ld\n", raw_samples[i]);
  }
}
```

---

Open this sketch up using the Arduino IDE and hit run. Now go to "Tools->Serial Plotter".

Changes this according to your wiring
```c++
#define I2S_MIC_SERIAL_CLOCK GPIO_NUM_26
#define I2S_MIC_LEFT_RIGHT_CLOCK GPIO_NUM_22
#define I2S_MIC_SERIAL_DATA GPIO_NUM_21
```

---

## 🖼️ INMP441 Microphone Sample "Hello"  

![Microphone Test Snapshot](./assets/Microphone%20test%20snapshot.png)  
