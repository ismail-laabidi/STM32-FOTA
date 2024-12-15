# Diagramme de Flux - ESP32 FOTA

```mermaid
flowchart TD
    A[Initialisation ESP32] --> B{Connexion WiFi réussie ?}
    B -->|Oui| C[Connecté au WiFi]
    B -->|Non| D[Réessayer connexion]
    D --> B

    C --> E{Téléchargement du fichier réussi ?}
    E -->|Oui| F[Stockage du fichier dans SPIFFS]
    E -->|Non| G[Erreur de téléchargement]

    F --> H{Réception 'g' de la STM32 ?}
    H -->|Oui| I[Transfert du fichier à la STM32]
    H -->|Non| J[Attente du signal 'g']
    J --> H

    I --> K[Transfert terminé]
    K --> L[Fin]
