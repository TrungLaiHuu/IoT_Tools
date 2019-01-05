#include <Wire.h>
#include <Eeprom24C32_64.h>
#define EEPROM_ADDRESS 0x50
static Eeprom24C32_64 eeprom(EEPROM_ADDRESS);
void setup()
{
  /* Initialize serial communication. */
  Serial.begin(115200);
  Serial.println();

  /* Initiliaze EEPROM library. */
  eeprom.initialize();

  /* Giá trị lưu vào AT24C32 */
  double dValue = 245.3456;
  /* Mảng chứa giá trị cần lưu */
  byte wArray[sizeof(double)] = {0};
  /* Mảng đọc giá trị từ AT24C32 */
  byte rArray[sizeof(double)] = {0};
  /* Giá trị đọc kiểu double */
  double dDestination = 0;
  

  /* Copy vùng nhớ của biến cần lưu vào mảng wArray */
  memcpy(wArray , &dValue , sizeof( double ) );
  Serial.printf("Write %f to eeprom\n", dValue);
  /* Ghi mảng đó vào AT24C32 */
  eeprom.writeBytes(0, sizeof( double ), wArray);

  /* Đọc một mảng độ dài là double tại địa chỉ 0 */
  eeprom.readBytes(0, sizeof(double), rArray);
  /* Chuyển ngược lại mảng thành giá trị kiểu double */
  memcpy(&dDestination, rArray, sizeof(double));

  // Print read bytes.
  Serial.printf("Gia tri doc duoc: %f\n", dDestination);
}
void loop()
{

}
