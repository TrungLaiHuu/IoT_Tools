// Include the correct display library
// For a connection via I2C using Wire include
#include <Wire.h>  // Only needed for Arduino 1.6.5 and earlier
#include "SSD1306Wire.h" // legacy include: `#include "SSD1306.h"`
#include "DHT.h"

#define DHTPIN D7     // what digital pin we're connected to

// Uncomment whatever type you're using!
#define DHTTYPE DHT11   // DHT 11
// Initialize the OLED display using Wire library
SSD1306Wire  oled(0x3c, D15, D14);

DHT dht(DHTPIN, DHTTYPE);

void setup()
{
  Serial.begin(115200);
  delay(10);
  printf("\nDHT11\n");
  oled.init();
  // display.flipScreenVertically();
  oled.setContrast(255);
  oled.clear();

  delay(3000);

  dht.begin();
}

//float old_h = 0, old_t = 0;
int x = 0;
bool delPixel = false;

void loop()
{
  delay(2000);
  float h = dht.readHumidity();
  // Read temperature as Celsius (the default)
  float t = dht.readTemperature();

  if (delPixel == true)
  {
    oled.setColor(BLACK);//màu nền
    if (x == 127)
      oled.drawVerticalLine(0, 10, 54);
    else
      oled.drawVerticalLine(x + 1, 10, 54);
  }
  int y = map(h, 0, 100, 10, 63);
  oled.setColor(WHITE);//màu pixel
  oled.setPixel(x++, 63 - y);
  if (x > 127)
  {
    x = 0;
    delPixel = true;
  }


  printf("\nDo am: %s %", String(h).c_str());
  printf("\nNhiet do: %s *C", String(t).c_str());

  oled.display();

}
