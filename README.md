# Code_Examples

## Control arduino format IP adress

```
#include <IPAddress.h>

void setup() {
  Serial.begin(9600);
  
  // Adresse IP à vérifier
  String ipString = "192.168.1.1";
  
  // Convertir la chaîne de caractères en adresse IP
  IPAddress ip;
  if (!ip.fromString(ipString)) {
    Serial.println("Adresse IP invalide !");
  } else {
    Serial.print("Adresse IP valide : ");
    Serial.println(ipString);
  }
}

void loop() {
  // Votre code ici
}
```

## Test presence chaine de caractére
```
void setup() {
  Serial.begin(9600);
  
  // Chaîne de caractères reçue
  String receivedString = "test toto";
  
  // Sous-chaîne à rechercher
  String searchString = "toto";
  
  // Vérifier si la sous-chaîne est présente dans la chaîne de caractères
  if (receivedString.indexOf(searchString) >= 0) {
    Serial.println("La sous-chaîne est présente dans la chaîne de caractères !");
  } else {
    Serial.println("La sous-chaîne n'est pas présente dans la chaîne de caractères.");
  }
}

void loop() {
  // Votre code ici
}
```

## load time NTP on RTC (PCF8563)

```
//time NTP RTC
const char *ntpServer = "pool.ntp.org";
const long  gmtOffset_sec = 3600 * 1;
const int   daylightOffset_sec = 3600 * 1;
RTC_PCF8563 rtc;
DateTime now;
struct tm timeinfo;

char daysOfTheWeek[7][12] = { "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" };

//get time ntp
// Init and get the time
configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);
printLocalTime();

//update time in RTC
//structure tm (int8_t wday, int16_t year, int8_t mon, int8_t mday, int8_t hour, int8_t min, int8_t sec)
// January 21, 2014 at 3am you would call:
// rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
Serial.print("Extract time : ");
Serial.print((timeinfo.tm_year+1900));
Serial.print("_");
Serial.print(timeinfo.tm_mon+1);
Serial.print("_");
Serial.print(timeinfo.tm_mday);
Serial.print("_");
Serial.print(timeinfo.tm_hour);
Serial.print("_");
Serial.print(timeinfo.tm_min);
Serial.print("_");
Serial.println(timeinfo.tm_sec);

rtc.adjust(DateTime(timeinfo.tm_year+1900, timeinfo.tm_mon+1, timeinfo.tm_mday, timeinfo.tm_hour, timeinfo.tm_min, timeinfo.tm_sec));
rtc.start();

//RTC PCF8563
now = rtc.now();

Serial.print(now.year(), DEC);
Serial.print('/');
Serial.print(now.month(), DEC);
Serial.print('/');
Serial.print(now.day(), DEC);
Serial.print(" (");
Serial.print(daysOfTheWeek[now.dayOfTheWeek()]);
Serial.print(") ");
Serial.print(now.hour(), DEC);
Serial.print(':');
Serial.print(now.minute(), DEC);
Serial.print(':');
Serial.print(now.second(), DEC);
Serial.println();
```
