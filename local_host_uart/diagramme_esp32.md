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


flowchart TD
    id1[Start]:::customStyle1 --> id2[Connect to WiFi]:::customStyle2
    id2 --> id3[Download File]:::customStyle3
    id3 --> id4[Send File via UART]:::customStyle4
    id4 --> id5[End]:::customStyle1

    %% Define custom styles
    classDef customStyle1 fill:#f9f,stroke:#333,stroke-width:2px;
    classDef customStyle2 fill:#ffcc00,stroke:#000,stroke-width:2px;
    classDef customStyle3 fill:#0f0,stroke:#000,stroke-width:2px;
    classDef customStyle4 fill:#00f,stroke:#fff,stroke-width:2px;
