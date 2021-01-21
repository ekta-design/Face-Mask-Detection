#include<string.h>
String ssid     = "Simulator Wifi";  // SSID to connect to
String password = ""; // Our virtual wifi has no password (so dont do your banking stuff on this network)
String host     = "api.thingspeak.com"; // Open Weather Map API
const int httpPort   = 80;
String uri = "/channels/1127986/fields/1.json?api_key=A3L7BJE4MM8DX6HM&results=1";
int motor = 10;
int buz = 9;
#include <LiquidCrystal.h>


// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

int setupESP8266(void) {
  // Start our ESP8266 Serial Communication
  Serial.begin(115200);   // Serial connection over USB to computer
  Serial.println("AT");   // Serial connection on Tx / Rx port to ESP8266
  delay(10);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 1;
    
  // Connect to 123D Circuits Simulator Wifi
  Serial.println("AT+CWJAP=\"" + ssid + "\",\"" + password + "\"");
  delay(10);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 2;
  
  // Open TCP connection to the host:
  Serial.println("AT+CIPSTART=\"TCP\",\"" + host + "\"," + httpPort);
  delay(50);        // Wait a little for the ESP to respond
  if (!Serial.find("OK")) return 3;
  
  return 0;
}


void face_detection(void) {

  String httpPacket = "GET " + uri + " HTTP/1.1\r\nHost: " + host + "\r\n\r\n";
  int length = httpPacket.length();
  
  // Send our message length
  Serial.print("AT+CIPSEND=");
  Serial.println(length);
  delay(10); // Wait a little for the ESP to respond if (!Serial.find(">")) return -1;

  // Send our http request
  Serial.print(httpPacket);
  delay(10); // Wait a little for the ESP to respond
  String ch;
  ch = Serial.readString();
  Serial.println(ch);
  int point = 0;
  for(int i = 0; i<500 ; i++)
  {
    if(ch[i] == 'x')
    {
      
      point = i+1;
      break;
      
    }
  }
  Serial.println(ch[point]);
  if(ch[point] == 'N')
  {
    lcd.print("No mask detected");
    Serial.println("No mask detected");
    lcd.setCursor(0,1);
    lcd.print("Access Denied..!");
    digitalWrite(buz,HIGH);
    delay(2000);
    digitalWrite(buz,LOW);
  }
  else
  {
    lcd.print("Mask detected");
    Serial.println("Mask detected");
    lcd.setCursor(0,1);
    lcd.print("Welcome..!");
    digitalWrite(motor,HIGH);
    delay(2000);
    digitalWrite(motor,LOW);
  }
  
  if (!Serial.find("SEND OK\r\n")) return;
  
  
}

void setup() {
  
  setupESP8266();
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
  // Print a message to the LCD.
}

void loop()
{
  face_detection();
  
  delay(1000);
}
