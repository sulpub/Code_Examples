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
