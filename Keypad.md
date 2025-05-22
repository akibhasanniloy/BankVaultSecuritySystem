```cpp
#include <Keypad.h>
#include <Password.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED display config
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Keypad config
const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};
byte rowPins[ROWS] = {14, 27, 26, 25};
byte colPins[COLS] = {33, 32, 18, 19};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);
Password password = Password("1234");

bool doorLocked = true;
int currentPasswordLength = 0;

void setup() {
  Serial.begin(115200);
  
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED not found!");
    while (true);
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("System Ready");
  display.display();
  delay(1000);
  display.clearDisplay();
}

void loop() {
  char key = keypad.getKey();
  if (key != NO_KEY) {
    delay(60);

    if (key == 'C') {
      resetPassword();
    } else {
      processKey(key);
    }
  }
}

void processKey(char key) {
  if (currentPasswordLength == 0) {
    display.clearDisplay();
    display.setCursor(0, 0);
    display.print("Enter Password:");
    display.setCursor(0, 10);
  }

  display.print(key);
  display.display();

  password.append(key);
  currentPasswordLength++;

  if (currentPasswordLength == 4) {
    display.clearDisplay();
    display.setCursor(0, 0);
    display.print("Thinking...");
    display.display();
    delay(1000);

    if (password.evaluate()) {
      doorLocked = !doorLocked;
      controlDoor(doorLocked, "keypad");
    } else {
      showError("keypad");
    }

    resetPassword();
  }
}

void resetPassword() {
  password.reset();
  currentPasswordLength = 0;
  display.clearDisplay();
  display.setCursor(0, 0);
  display.print("Reset");
  display.display();
  delay(500);
  display.clearDisplay();
}

void controlDoor(bool locked, String method) {
  display.clearDisplay();
  display.setCursor(0, 0);
  if (locked) {
    display.print("Door Locked");
  } else {
    display.print("Door Unlocked");
  }
  display.setCursor(0, 10);
  display.print("By: " + method);
  display.display();
}

void showError(String method) {
  display.clearDisplay();
  display.setCursor(0, 0);
  display.print("Wrong Password");
  display.setCursor(0, 10);
  display.print("via " + method);
  display.display();
  delay(1500);
  display.clearDisplay();
}

```
