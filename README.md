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
