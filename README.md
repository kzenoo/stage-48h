# stage-48h

Projet de _Raffanel_ Guilhem

I. [Description](#i-description-🔎)

II. [Notice](#ii-notice-📜)

III. [Réalisation](#iii-réalisation-💻)

IV. [Amélioration](#iv-amélioration-➕)

V. [Composant](#v-composant)


## I. Description: 🔎

Ce projet a eu pour but de réaliser un boitier permettant l'ouverture d'une porte à l'aide d'un badge RFID.

Pour ce projet trois outils principaux ont été utilisé tel qu'un capteur RFID, un afficheur lcd et une carte arduino uno.

## II. Notice: 📜

Pour faire fonctionner ce boitier un badge RFID est obligatoire afin de le scanner. Si le badge est détecter comme valide la porte s'ouvrira sinon au bout de 3 tentatives echouées le badge est considéré comme non valide et enverra une alerte.

Pour la réparation et/ou l'installation, il faut retirer la plaque arrière puis tirer la plaque supérieur. 

## III. Réalisation: 💻

Lien vers le boitier : https://a360.co/42tZnlB

[code du boitier](/Code.ino)

[le schéma électrique du tout](/shema%20elec%20rfid.kicad_sch)

## IV. Amélioration: ➕

Des améliorations qui auraient pu être ajoutées:

- Une batterie au lithium de 7.4V.
- Augmenter les marges entre les corps.
- Ajouter un bloc écrou pour pouvoir visser et fermer la plque arrière.
- Diminuer l'épaisseur de la plaque supérieur afin que le lcd et celle-ci ne s'entrecroise plus.

## V. Composant:

RFID RC-522: [Lien vers sa datasheet](http://www.handsontec.com/dataspecs/RC522.pdf)

LCD : [Lien vers sa datasheet](https://components101.com/sites/default/files/component_datasheet/16x2%20LCD%20Datasheet.pdf)

Arduino UNO : [Lien vers sa datasheet](https://docs.arduino.cc/resources/datasheets/A000066-datasheet.pdf)

Batterie RS PRO 7.4V Lithium-Ion : [Lien vers sa datasheet](https://docs.rs-online.com/c165/A700000007945034.pdf)