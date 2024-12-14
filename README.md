# FOTA (Firmware Over-The-Air) for STM32

## Description
Ce projet met en œuvre une solution de mise à jour de firmware Over-The-Air (FOTA) pour les microcontrôleurs STM32 à l'aide d'un ESP32 comme intermédiaire. Il permet de télécharger un firmware depuis un serveur distant (hébergé sur GitHub) et de le transférer au STM32 via une connexion UART pour une mise à jour en toute simplicité.

Ce projet a été réalisé comme un **projet de fin de semestre** dans le cadre de notre formation, sous la supervision de **Mr. Belkouche**.

## Fonctionnalités principales
- **Téléchargement OTA** : Télécharge un firmware binaire à partir d'un lien HTTPS sur GitHub.
- **Communication UART** : Transfert sécurisé du fichier binaire de l'ESP32 vers le STM32.
- **Bootloader STM32** : 
  - Gère la réception du firmware via UART.
  - Écrit le firmware dans la mémoire Flash.
  - Lance l'application utilisateur après une mise à jour réussie.

## Architecture du projet
Le projet est divisé en deux composants principaux :
1. **ESP32** : 
   - Télécharge le fichier firmware via HTTPS à partir d'un serveur distant.
   - Transfère le fichier au STM32 via UART, en utilisant un protocole simple (synchronisation avec des caractères de contrôle).
   
2. **STM32** :
   - Contient un bootloader qui :
     - Réceptionne le fichier.
     - Écrit le firmware dans la Flash à l’adresse utilisateur (`0x08004400`).
     - Saute vers l’application utilisateur après validation.

## Dépendances
- **ESP32** :
  - [Arduino Core for ESP32](https://github.com/espressif/arduino-esp32)
  - Bibliothèques utilisées :
    - `WiFi.h`
    - `WiFiClientSecure.h` (pour gérer les connexions HTTPS)
    - `SPIFFS.h` (pour stocker temporairement le firmware sur l'ESP32)
  
- **STM32** :
  - STM32 HAL (Hardware Abstraction Layer)
  - Outils utilisés :
    - STM32CubeIDE pour le développement.
    - Bootloader écrit en C utilisant les bibliothèques HAL.

## Comment utiliser ce projet
### 1. Préparer le fichier firmware
- Compilez votre application STM32 utilisateur et générez un fichier `.bin`.
- Hébergez ce fichier sur un serveur accessible via HTTPS (par exemple, sur GitHub Releases).

### 2. Configurer l’ESP32
- Dans le fichier de configuration de l'ESP32 (`ESP32_FOTA.ino`), mettez à jour :
  - Les identifiants WiFi (`WIFI_SSID` et `WIFI_PASSWORD`).
  - L'URL du fichier binaire (`FILE_URL`) pointant vers le lien HTTPS du firmware.

### 3. Charger les firmwares
- **ESP32** : Flashez le firmware de l’ESP32 en utilisant l’Arduino IDE.
- **STM32** : Flashez le bootloader sur le STM32 avec STM32CubeProgrammer.

### 4. Tester la mise à jour
1. Lancez l'ESP32 et le STM32.
2. Observez les logs sur les moniteurs série :
   - ESP32 : Téléchargement du firmware et transfert UART.
   - STM32 : Réception, écriture dans la Flash, et exécution de l'application mise à jour.

## Diagramme de flux
```mermaid
graph TD
    A[ESP32 démarre] --> B[Connexion au WiFi]
    B --> C[Téléchargement du fichier firmware]
    C --> D[Transfert UART au STM32]
    D --> E[STM32 Bootloader démarre]
    E --> F[Réception du fichier firmware]
    F --> G[Écriture dans la Flash STM32]
    G --> H[Lancement de l'application utilisateur]
