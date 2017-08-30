# led
int sensorPin = 0; 
#include <Adafruit_NeoPixel.h>

#define PIN 6
int soundSensor = 2;

bool poweron = LOW;


Adafruit_NeoPixel strip = Adafruit_NeoPixel(24, PIN, NEO_GRB + NEO_KHZ800);

void setup()
{
  pinMode (soundSensor, INPUT);
  strip.begin();
  strip.show();

  for (int i = 0; i < strip.numPixels(); i++) {
    strip.setPixelColor(i,  strip.Color(200, 0, 0));
  }

  strip.show();


  Serial.begin(9600);  //Start the serial connection with the computer
  //to view the result open the serial monitor
}

void loop()                     // run over and over again
{
  int statusSensor = digitalRead (soundSensor);
  int reading = analogRead(sensorPin);
  Serial.println(reading);

  if (statusSensor == 0) {
    poweron = !poweron;
  }

  if (poweron) {
    if (reading > 70) {
      for (int i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i,  strip.Color(200, 0, 0));
      }
    } else if (reading < 30) {
      for (int i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i,  strip.Color(0, 0, 200));
      }

    } else {
      int red;
      int blue;+
      int green;

      if (reading > 50) {
        blue = 0;
        red = map (reading, 51, 70, 0, 200);
        green = map (reading, 51, 70, 200, 0);

      } else {
        red = 0;
        blue = map (reading, 30, 50, 200, 0);
        green = map (reading, 30, 50, 0, 200);

      }



      for (int i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i,  strip.Color(red, green, blue));

      }



    }

    strip.show();
    delay(5);
  } else {
    for (int i = 0; i < strip.numPixels(); i++) {
      strip.setPixelColor(i,  strip.Color(0, 0, 0));

    }
    strip.show();
    delay(5);

  }
  //waiting a second
}
