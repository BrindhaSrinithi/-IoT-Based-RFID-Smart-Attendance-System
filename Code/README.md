
```cpp
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClientSecure.h>

#include <SPI.h>
#include <MFRC522.h>

#include <Wire.h>
#include <LiquidCrystal_I2C.h>


#define SS_PIN  D8
#define RST_PIN D3

MFRC522 rfid(SS_PIN, RST_PIN);

LiquidCrystal_I2C lcd(0x27, 16, 2);

const char* ssid     = "vivo Y16";
const char* password = "brindha07";

String scriptURL = "https://script.google.com/macros/s/AKfycbyrUxf-SDre3K92W-vzuR0g7nfJzUsankofmLpNvkTSR7uKQNuuzrNncxUQhf6txf_g/exec";
void setup() {

  Serial.begin(115200);
  Wire.begin(D2, D1);      
  lcd.init();
  lcd.backlight();

  SPI.begin();             
  rfid.PCD_Init();
  delay(100);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(300);
  }

  readyScreen();
}


void loop() {

  if (!rfid.PICC_IsNewCardPresent()) {
    return;
  }

  if (!rfid.PICC_ReadCardSerial()) {
    return;
  }


  String uid = "";

  for (byte i = 0; i < rfid.uid.size; i++) {

    if (rfid.uid.uidByte[i] < 0x10) {
      uid += "0";
    }

    uid += String(rfid.uid.uidByte[i], HEX);
  }

  uid.toUpperCase();

  String studentName = "";

  if (uid == "3AA71C31") {

    studentName = "Brindha Srinithi";

  }

  else if (uid == "C06EA55F") {

    studentName = "Pranav";  

  }

  else if (uid == "1A583906") {

    studentName = "Harshini"; 

  }

  else {

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Unknown Card");

    lcd.setCursor(0, 1);
    lcd.print("Try Again");

    delay(2000);

    finishRFID();
    readyScreen();

    return;
  }


  String result = sendAttendance(studentName, uid);


  if (result.indexOf("TIME OUT RECORDED") >= 0) {

    showTimeOut(studentName);

  }


  else if (result.indexOf("TIME IN RECORDED") >= 0) {

    showTimeIn(studentName);

  }


  else {

    lcd.clear();

    lcd.setCursor(0, 0);
    lcd.print("Please Try Again");

    lcd.setCursor(0, 1);
    lcd.print("Scan Card");

    delay(2000);
  }


  finishRFID();

  delay(1000);

  readyScreen();
}


String sendAttendance(String name, String uid) {



  if (WiFi.status() != WL_CONNECTED) {

    WiFi.disconnect();
    WiFi.begin(ssid, password);

    unsigned long start = millis();

    while (
      WiFi.status() != WL_CONNECTED &&
      millis() - start < 10000
    ) {
      delay(200);
    }

    if (WiFi.status() != WL_CONNECTED) {
      return "ERROR";
    }
  }


  WiFiClientSecure client;
  client.setInsecure();

  HTTPClient https;


  String url =
    scriptURL +
    "?name=" + urlEncode(name) +
    "&uid=" + uid;


  https.setFollowRedirects(
    HTTPC_STRICT_FOLLOW_REDIRECTS
  );


  if (!https.begin(client, url)) {
    return "ERROR";
  }


  int httpCode = https.GET();

  String response = "";


  if (httpCode > 0) {

    response = https.getString();
    response.trim();
  }


  https.end();

  return response;
}


void showTimeIn(String name) {

  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Welcome");

  lcd.setCursor(0, 1);
  lcd.print(name.substring(0, 16));

  delay(2000);

  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Your Attendance");

  lcd.setCursor(0, 1);
  lcd.print("Is Recorded");

  delay(2000);


  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Have A Nice Day!");

  lcd.setCursor(0, 1);
  lcd.print("Enjoy Your Class");

  delay(2000);
}


void showTimeOut(String name) {

  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Thank You");

  lcd.setCursor(0, 1);
  lcd.print(name.substring(0, 16));

  delay(2000);


  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Your Time Out");

  lcd.setCursor(0, 1);
  lcd.print("Is Recorded");

  delay(2000);


  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Hope You Enjoyed");

  lcd.setCursor(0, 1);
  lcd.print("Your Classes!");

  delay(2000);



  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("See You Again!");

  lcd.setCursor(0, 1);
  lcd.print("Have A Nice Day");

  delay(2000);
}


void readyScreen() {

  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("RFID Attendance");

  lcd.setCursor(0, 1);
  lcd.print("Tap Your Card");
}

void finishRFID() {

  rfid.PICC_HaltA();
  rfid.PCD_StopCrypto1();
}



String urlEncode(String text) {

  String encoded = "";

  char hex[] = "0123456789ABCDEF";


  for (unsigned int i = 0; i < text.length(); i++) {

    unsigned char c = text.charAt(i);


    if (
      (c >= 'a' && c <= 'z') ||
      (c >= 'A' && c <= 'Z') ||
      (c >= '0' && c <= '9') ||
      c == '-' ||
      c == '_' ||
      c == '.' ||
      c == '~'
    ) {

      encoded += (char)c;

    }

    else {
```
      encoded += '%';
      encoded += hex[(c >> 4) & 0x0F];
      encoded += hex[c & 0x0F];
    }
  }

  return encoded;
}
