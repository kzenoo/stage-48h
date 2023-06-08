# stage-48h

# Description
Mon projet a été de réaliser un boitier permettant a l'aide d'un badge RFID l'ouverture d'une porte.

Pour ce projet je me suis servi de trois outils principaux tel qu'un capteur RFID, suivi d'un afficheur lcd et pour finir d'une carte arduino uno.

# Notice

Pour fonctionner ce boitier on a besoin d'un badge RFID afin de le scanner et si le badge est détecter comme valide la porte s'ouvrira sinon au bout de 3 éssais le badge n'est pas bon une alerte se lance

Pour la réparation ou l'installation c'est très simple il faut retirer la plaque arrière ainsi que tirer la plaque supérieur 

# Réalisation

https://a360.co/42tZnlB

```arduino
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 10
#define RST_PIN 9
    
#define AccesFlag_PIN 2
#define Gate_PIN 3
#define Max_Acces 3

byte Count_acces=0; 
byte CodeVerif=0; 
byte Code_Acces[4]={0x20, 0x12, 0x23, 0x2B}; 

MFRC522 rfid(SS_PIN, RST_PIN); // Instance of the class

// Init array that will store new NUID 
byte nuidPICC[4];

void setup() 
{ 
  // Init RS232
  Serial.begin(9600);

  // Init SPI bus
  SPI.begin(); 

  // Init MFRC522 
  rfid.PCD_Init(); 

  // Init LEDs 
  pinMode(AccesFlag_PIN, OUTPUT);
  pinMode(Gate_PIN, OUTPUT);
  
  digitalWrite(AccesFlag_PIN, LOW);
  digitalWrite(Gate_PIN, LOW);
}
 
void loop() 
{
  // Initialisé la boucle si aucun badge n'est présent 
  if ( !rfid.PICC_IsNewCardPresent())
    return;

  // Vérifier la présence d'un nouveau badge 
  if ( !rfid.PICC_ReadCardSerial())
    return;

  // Afffichage 
  Serial.println(F("Un badge est détecté"));

  // Enregistrer l’ID du badge (4 octets) 
  for (byte i = 0; i < 4; i++) {
    nuidPICC[i] = rfid.uid.uidByte[i];
  }

  // Vérification du code 
  CodeVerif= GetAccesState(Code_Acces,nuidPICC); 
  if (CodeVerif!=1)
  {
    Count_acces+=1;
    if(Count_acces==Max_Acces)
    {
     // Dépassement des tentatives (clignotement infinie) 
     while(1)
     {
      digitalWrite(AccesFlag_PIN, HIGH);
      delay(200); 
      digitalWrite(AccesFlag_PIN, LOW);
      delay(200); 
      // Affichage 
      Serial.println("Anomalie!");
     }
    }
    else
    {
      // Affichage 
      Serial.println("Acces denied");
    
      // Un seul clignotement: Code erroné 
      digitalWrite(AccesFlag_PIN, HIGH);
      delay(1000); 
      digitalWrite(AccesFlag_PIN, LOW);
    }
  }
  else
  {
    // Affichage 
    Serial.println("Ouverture de la porte");
    
    // Ouverture de la porte & Initialisation 
    digitalWrite(Gate_PIN, HIGH);
    delay(3000); 
    digitalWrite(Gate_PIN, LOW);
    Count_acces=0; 
  }

  // Re-Init RFID
  rfid.PICC_HaltA(); // Halt PICC
  rfid.PCD_StopCrypto1(); // Stop encryption on PCD
}

byte GetAccesState(byte *CodeAcces,byte *NewCode) 
{
  byte StateAcces=0; 
  if ((CodeAcces[0]==NewCode[0])&&(CodeAcces[1]==NewCode[1])&&
  (CodeAcces[2]==NewCode[2])&& (CodeAcces[3]==NewCode[3]))
    return StateAcces=1; 
  else
    return StateAcces=0; 
}
```

# Amélioration

Comme amélioration j'aurai pu ajouter:

- La batterie
- Augmenter les marges entre les crops
- Ajouter un bloc écrou pour pouvoir visser et fermer la plque arrière
- Diminuer l'épaisseur de la plaque supérieur afin que le lcd et celle si ne se dérange plus
