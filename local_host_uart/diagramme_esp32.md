# Diagramme de Flux - ESP32 FOTA

```mermaid
flowchart TD
    A[Initialisation ESP32]:::customStyle1 --> B{Connexion WiFi réussie ?}:::customStyle1
    B -->|Oui| C[Connecté au WiFi]:::customStyle1
    B -->|Non| D[Réessayer connexion]:::customStyle1
    D --> B:::customStyle1

    C --> E{Téléchargement du fichier réussi ?}:::customStyle1
    E -->|Oui| F[Stockage du fichier dans SPIFFS]:::customStyle1
    E -->|Non| G[Erreur de téléchargement]:::customStyle1

    F --> H{Réception 'g' de la STM32 ?}:::customStyle1
    H -->|Oui| I[Transfert du fichier à la STM32]:::customStyle1
    H -->|Non| J[Attente du signal 'g']:::customStyle1
    J --> H

    I --> K[Transfert terminé]:::customStyle1
    K --> L[Fin]:::customStyle1

    %% Define custom styles
    classDef customStyle1 fill:#2954,stroke:#111,stroke-width:2px;
    classDef customStyle2 fill:#ffcc00,stroke:#000,stroke-width:2px;
    classDef customStyle3 fill:#0f0,stroke:#000,stroke-width:2px;
    classDef customStyle4 fill:#00f,stroke:#fff,stroke-width:2px;
