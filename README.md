# PR62: Procedimiento
Volvemos a añadir dos librerias, en este caso seran:
- SPI -> Para comuncaciones con dispositivos SPI
- MFRC522 -> Para leer y escribir en tarjetas RFID utilizando la interfaz ISO/IEC 14443A/MIFARE.
```
#include <SPI.h>
#include <MFRC522.h>
```
Para empezar especificamos los pines Reset y SDA(SS) del módulo. Ademas, creamos el objeto para el RC522. NOTA: Los otros pines son inecesarias sus especificacions ya que trabajan y se asignan junto con el arduino
```
#define RST_PIN 9
#define SS_PIN 10
MFRC522 mfrc522(SS_PIN, RST_PIN);
```
Iniciamos el setup() estableciendo una comunicacion serie de 9600 bauds y iniciando también el bus SPI. Seguidamente utilizamos la función mfrc522.PCD_Init() para iniciar y configurar el RC522. Por ultimo mostramos los datos por pantalla.
```
void setup() {
Serial.begin(9600);
SPI.begin();
mfrc522.PCD_Init();
Serial.println("Lectura del UID");
}
```
Finalizamos con el void loop() donde iniciamos con un if que tenga como funcion mfrc522.PICC.IsNewCardPresent() la cual devuelve un valor booleano dependiendo de si detecta una tarjeta cerca del modulo RC522 nos devuelve true, y sino es asi false. Dentro de este mismo if creamos otro if el cual tendra como funcion mfrc522.PICC.ReadCardSerial() la cual se utiliza para la comunicacion con esta misma tarjeta que puede ser detectada, recibimos true si se establece conexion con la tarjeta y sino false. Por ultimo creamos un bucle para imprimir el codigo de verificación con la funcion mfrc522.uid.uidByte[i] (la i es una variable que creamos dentro del bucle for)
Por ultimo utilizamos la funcion mfrc522.PICC.HaltA() para finalizar la conexion con la tarjeta.
```
void loop() {
if ( mfrc522.PICC_IsNewCardPresent())
{
if ( mfrc522.PICC_ReadCardSerial())
{
Serial.print("Card UID:");
for (byte i = 0; i < mfrc522.uid.size; i++) {
Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
Serial.print(mfrc522.uid.uidByte[i], HEX);
}
Serial.println();
mfrc522.PICC_HaltA();
}
}
```