# Arduino

## Simple LED Blinking

```c
# define LED_PIN 8

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(300);
  digitalWrite(LED_PIN, LOW);
  delay(300);
}
```

## Scrolling Sign

```c
#include <Adafruit_NeoMatrix.h>
#ifndef PSTR
 #define PSTR 
#endif

#define PIN 6
#define mw 16
#define mh 7
#define increaseHourBtnPin 2
#define increaseMinuteBtnPin 4
#define LED_TEST_PIN 7

Adafruit_NeoMatrix matrix = Adafruit_NeoMatrix(mw, mh, PIN,
NEO_MATRIX_ZIGZAG, 
  NEO_GRB            + NEO_KHZ800);

const uint16_t colors[] = {
  matrix.Color(255, 0, 0), matrix.Color(0, 255, 0), matrix.Color(0, 0, 255) };

void setup() {
  pinMode(increaseHourBtnPin, INPUT);
  pinMode(increaseMinuteBtnPin, INPUT);
  pinMode(LED_TEST_PIN, OUTPUT);

  matrix.begin();
  // matrix.setTextWrap(false);
  matrix.setBrightness(200);
  matrix.setTextColor(colors[0]);
}

int x    = matrix.width();
int pass = 0;

// Time
int h = 12;
int m = 59;
int s = 30;
int timeDelay = 100;
int tickCounter = 0;
int cooldown = 0;

void loop() {
  int increaseHourState = digitalRead(increaseHourBtnPin);
  int increaseMinuteState = digitalRead(increaseMinuteBtnPin);

  if (increaseHourState == HIGH or increaseMinuteState == HIGH) {
    digitalWrite(LED_TEST_PIN, HIGH);
  }

  if (increaseHourState == LOW and increaseMinuteState == LOW) {
    digitalWrite(LED_TEST_PIN, LOW);
  }

  if (increaseHourState == HIGH and cooldown == 0) {
    cooldown++;
    h++;  
  }

  if (increaseMinuteState == HIGH and cooldown == 0 ) {
    cooldown++;    
    m++;
  }

  
  // Check the time
  if (s == 60) {
    s = 0;
    m++;
  }
  if (m == 60) {
    m = 0;
    h++;
  }
  if (h == 24) {
    h = 0;
  }
  
  matrix.fillScreen(0);
  matrix.setCursor(x,0);
  String a = String(2);
  char buff[50];
  sprintf(buff, "Amin Ghasvari %d:%d:%d", h, m, s);
  matrix.print(buff);
  if(--x < -120) {
    x = matrix.width();
    if(++pass >= 3) pass = 0;
    matrix.setTextColor(colors[pass]);
  }
  matrix.show();

  if (timeDelay * tickCounter >= 1000) {
    tickCounter=0;
    s++;
  }
  delay(timeDelay);
  tickCounter++;

  // Handle cooldown
  if (cooldown >= 3) {
    cooldown = 0;
  } else if (cooldown != 0) {
    cooldown++;
  }
}
```

# See More

- [Arduino-CLI](Arduino-CLI.md)