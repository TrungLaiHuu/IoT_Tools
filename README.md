#define PinR  D6
#define PinG  D7
#define PinB  D8

char red, green, blue;

void setRGBColor(int red, int green, int blue);

void setup()
{
  Serial.begin(115200);
  pinMode(PinR, OUTPUT);
  pinMode(PinG, OUTPUT);
  pinMode(PinB, OUTPUT);
  analogWriteRange(255);
  analogWriteFreq(1000);

  setRGBColor(50, 50, 50);
}


void loop()
{
  if (Serial.available())
  {
    String str = Serial.readString();
    printf("Nhan dươc string: %s\n", str.c_str());
  }
}

void setRGBColor(int red, int green, int blue)
{
  red = red;
  green = green;
  blue = blue;
  analogWrite(PinR, red);
  analogWrite(PinG, green);
  analogWrite(PinB, blue);
}
