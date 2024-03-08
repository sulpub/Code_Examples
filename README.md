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


## Code exemple monté en puissance moteur un seul fil

Ce code permet un demarrage en rampup d'une moteur sur l'alimentation d'un moteur DC.
Note : il faut un étage de puissance pour alimenter le moteur.

```
const int pwmPin = 9; // Broche PWM pour le contrôle du moteur
const int rampDelay = 10; // Durée entre chaque augmentation de puissance (en millisecondes)
const int maxPower = 255; // Puissance maximale du moteur

void setup() {
  // Initialisation de la broche PWM en sortie
  pinMode(pwmPin, OUTPUT);
}

void loop() {
  // Fading progressif de 0 à maxPower
  for (int power = 0; power <= maxPower; power++) {
    analogWrite(pwmPin, power); // Réglage de la puissance du moteur
    delay(rampDelay); // Attente pour le fading progressif
  }
  // Fading progressif de maxPower à 0
  for (int power = maxPower; power >= 0; power--) {
    analogWrite(pwmPin, power); // Réglage de la puissance du moteur
    delay(rampDelay); // Attente pour le fading progressif
  }
}
```


## Code exemple monté en puissance moteur pas à pas

Ce code permet un demarrage en rampup d'une moteur pas à pas sous arduino.

```
#include <Stepper.h>

const int stepsPerRevolution = 200; // Nombre de pas par révolution pour votre moteur
const int rampDelay = 10; // Durée entre chaque augmentation de puissance (en millisecondes)

// Initialisez le stepper avec les broches appropriées
Stepper myStepper(stepsPerRevolution, 8, 9, 10, 11);

void setup() {
  // Rien à faire dans la configuration
}

void loop() {
  // Démarrage du fading progressif
  for (int power = 0; power <= 255; power++) {
    myStepper.setSpeed(power); // Réglez la vitesse du moteur pas à pas
    myStepper.step(1); // Faites avancer le moteur d'un pas
    delay(rampDelay); // Attente pour le fading progressif
  }

  // Inversion de la direction du fading progressif
  for (int power = 255; power >= 0; power--) {
    myStepper.setSpeed(power); // Réglez la vitesse du moteur pas à pas
    myStepper.step(-1); // Faites reculer le moteur d'un pas
    delay(rampDelay); // Attente pour le fading progressif
  }
}


```
