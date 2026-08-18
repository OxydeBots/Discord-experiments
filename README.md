Voici une explication sur comment Discord distribue leur fonctionnalité de manière aléatoire
                              
                              DISCORD
                                 │
                                 ▼
                     ┌──────────────────────┐
                     │  NOUVELLE FEATURE    │
                     │  / nouveau système   │
                     └──────────┬───────────┘
                                │
                                ▼
                     ┌──────────────────────┐
                     │ Création d'un        │
                     │ EXPERIMENT           │
                     └──────────┬───────────┘
                                │
                                ▼
              ┌──────────────────────────────────┐
              │ Discord définit COMMENT tester   │
              │ la feature                       │
              └────────────────┬─────────────────┘
                               │
              ┌────────────────┼──────────────────┐
              │                │                  │
              ▼                ▼                  ▼
        USER EXPERIMENT   GUILD EXPERIMENT    APEX EXPERIMENT
              │                │                  │
              │                │                  │
              ▼                ▼                  ▼
          PERSONNE         SERVEUR             UNITÉ CIBLÉE
              │                │             ┌────┼─────┐
              │                │             │    │     │
              │                │            USER GUILD INSTALLATION
              │                │
              └────────────────┴──────────────────────┐
                                                       │
                                                       ▼
                                      ┌──────────────────────────┐
                                      │ 1. DÉTERMINATION DE      │
                                      │    L'ÉLIGIBILITÉ         │
                                      └────────────┬─────────────┘
                                                   │
                              ┌────────────────────┼────────────────────┐
                              │                    │                    │
                              ▼                    ▼                    ▼
                         ID utilisateur       ID serveur          Installation
                              │                    │                    │
                              └────────────────────┼────────────────────┘
                                                   │
                                                   ▼
                                      ┌──────────────────────────┐
                                      │ 2. FILTRES               │
                                      │                          │
                                      │ • User                   │
                                      │ • Guild                  │
                                      │ • Guild features         │
                                      │ • Taille du serveur      │
                                      │ • Âge du serveur         │
                                      │ • ID spécifique          │
                                      │ • Région / environnement │
                                      │ • Client / installation  │
                                      │ • Autres critères        │
                                      └────────────┬─────────────┘
                                                   │
                                          ┌────────┴────────┐
                                          │                 │
                                      ÉLIGIBLE          PAS ÉLIGIBLE
                                          │                 │
                                          ▼                 ▼
                               CONTINUE LE TEST        ┌───────────┐
                                          │            │ FEATURE   │
                                          │            │ NON       │
                                          │            │ DISPONIBLE│
                                          │            └───────────┘
                                          ▼
                              ┌──────────────────────────┐
                              │ 3. CALCUL DE POSITION    │
                              │                          │
                              │ Experiment + Resource ID │
                              │            │             │
                              │            ▼             │
                              │        HASH / BUCKET     │
                              │            │             │
                              │            ▼             │
                              │       0 ───────── 9999   │
                              └────────────┬─────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │ 4. ROLLOUT               │
                              │                          │
                              │ Discord découpe les      │
                              │ positions en plages      │
                              └────────────┬─────────────┘
                                           │
                     ┌─────────────────────┼─────────────────────┐
                     │                     │                     │
                     ▼                     ▼                     ▼
                  0–499                 500–2499             2500–9999
                   ~5 %                   ~20 %                 ~75 %
                     │                     │                     │
                     ▼                     ▼                     ▼
                TREATMENT A            TREATMENT B             CONTROL
               Feature ON             Variante 2              Feature OFF
                     │                     │                     │
                     └─────────────────────┼─────────────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │ 5. OVERRIDE ?            │
                              │                          │
                              │ Discord peut définir     │
                              │ manuellement certains    │
                              │ utilisateurs/serveurs    │
                              └────────────┬─────────────┘
                                           │
                              ┌────────────┴────────────┐
                              │                         │
                             OUI                       NON
                              │                         │
                              ▼                         ▼
                     ┌─────────────────┐       Utilise le résultat
                     │ BUCKET FORCÉ    │       du rollout
                     │ / accès forcé   │              │
                     └────────┬────────┘              │
                              │                       │
                              └───────────┬───────────┘
                                          │
                                          ▼
                              ┌──────────────────────────┐
                              │ 6. ASSIGNATION FINALE    │
                              │                          │
                              │     ┌──────────────┐     │
                              │     │              │     │
                              │     ▼              ▼     │
                              │   FEATURE ON    FEATURE OFF
                              │     │              │     │
                              └─────┬──────────────┬─────┘
                                    │              │
                                    ▼              ▼
                              UTILISATEUR       UTILISATEUR
                              SÉLECTIONNÉ       NON SÉLECTIONNÉ
                                    │
                                    ▼
                         ┌────────────────────────┐
                         │ 7. DISCORD LUI ENVOIE  │
                         │    / expose la feature │
                         └───────────┬────────────┘
                                     │
                                     ▼
                              ┌───────────────┐
                              │ TEST EN       │
                              │ CONDITIONS    │
                              │ RÉELLES       │
                              └───────┬───────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │ ANALYSE DES   │
                              │ RÉSULTATS     │
                              └───────┬───────┘
                                      │
                         ┌────────────┼────────────┐
                         │            │            │
                         ▼            ▼            ▼
                      PROBLÈMES    MITIGÉ       SUCCÈS
                         │            │            │
                         ▼            ▼            ▼
                       STOP       AJUSTEMENT    ROLLOUT +
                                    │            │
                                    └─────┬──────┘
                                          │
                                          ▼
                               5 % → 10 % → 25 %
                                          │
                                          ▼
                                      50 % → 75 %
                                          │
                                          ▼
                                        100 %
                                          │
                                          ▼
                              ┌────────────────────┐
                              │ FEATURE PUBLIQUE   │
                              │ POUR TOUT LE MONDE │
                              └────────────────────┘
