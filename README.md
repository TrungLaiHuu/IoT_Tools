#include <DHT.h>
#include <ESP8266WiFi.h>

//DHT config
#define DHTPIN  D7     // what digital pin we're connected to
#define DHTTYPE DHT11   // DHT 11

DHT dht(DHTPIN, DHTTYPE);

// Wi-Fi Settings
const char* ssid = "Mcnex_edu"; //Wi-Fi network name
const char* password = "mcnex.edu@2019"; //Wi-Fi network password

WiFiClient client;
// ThingSpeak Settings
String writeAPIKey = "API-KEY"; /* write API key for your ThingSpeak Channel */
const char* server = "api.thingspeak.com";
const int postingInterval = 5 * 1000; //post data every 2 seconds

void setup() {
  Serial.begin(115200);
  dht.begin();
  Serial.print("Dang ket noi WiFi");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(100);
  }
  Serial.println("\r\nWiFi ket noi thanh cong!");
}

void loop() {
  // wait and then post again
  delay(postingInterval);

  float temp = dht.readTemperature();
  float humi = dht.readHumidity();
  if (isnan(temp) || isnan(humi)) {
    Serial.println("Khong doc duoc du lieu tu cam bien DHT11!");
    return;
  }
  if (client.connect(server, 80)) {
    // Construct API request body
           /* Thay đổi số lượng trường của kênh thì thay đổi tại dòng lệnh bên dưới này */
    String body = "field1=" + String(temp, 1) + "&field2=" + String(humi, 1);

           // Đây là phần dữ liệu không thay đổi
    client.print("POST /update HTTP/1.1\n");
    client.print("Host: api.thingspeak.com\n");
    client.print("Connection: close\n");
    client.print("X-THINGSPEAKAPIKEY: " + writeAPIKey + "\n");
    client.print("Content-Type: application/x-www-form-urlencoded\n");
    client.print("Content-Length: ");
    client.print(body.length());
    client.print("\n\n");
    client.print(body);
    client.print("\n\n");

           // Construct API request body
           // Gửi dữ liệu lên cổng COM
    Serial.printf("Nhiet do: %s - Do am: %s\r\n", String(temp, 1).c_str(), String(humi, 1).c_str());
  }
  client.stop();
}
