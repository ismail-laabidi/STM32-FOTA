flowchart TD
    A[Initialisation du système] --> B{Configuration des périphériques}
    B -->|GPIO Init| C[Initialisation des GPIOs]
    B -->|UART Init| D[Initialisation de l'UART1]
    C --> E[Affichage de la version de l'application]
    D --> E

    E --> F[Début de la boucle principale]

    F --> G[Clignotement de la LED PC13]
    G --> H[Attente de 100ms]
    H --> F

    E --> I{Erreur lors de l'initialisation ?}
    I -->|Oui| J[Appel de Error_Handler]
    I -->|Non| F

    J --> K[Blocage du système]

    %% Styles personnalisés
    classDef init fill:#f9f,stroke:#333,stroke-width:2px;
    classDef loop fill:#bbf,stroke:#333,stroke-width:2px;
    classDef error fill:#faa,stroke:#333,stroke-width:2px;

    class A,B,C,D,E init;
    class F,G,H loop;
    class I,J,K error;
