Voici une explication sur comment Discord distribue leur fonctionnalité sur les plateformes
                                      
                                     DISCORD BACKEND
                                            │
                                            ▼
                              ┌──────────────────────────┐
                              │   FEATURE / EXPERIMENT   │
                              │                          │
                              │  Exemple : nouvelle UI   │
                              │  nouveau bouton, etc.    │
                              └────────────┬─────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │ CONFIGURATION DU TEST    │
                              │                          │
                              │ • Experiment             │
                              │ • Revision               │
                              │ • Treatments / Variants  │
                              │ • Populations            │
                              │ • Filters                │
                              │ • Rollout                │
                              │ • Overrides              │
                              │ • Holdout                │
                              └────────────┬─────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │      QUI EST TESTÉ ?     │
                              └────────────┬─────────────┘
                                           │
                 ┌─────────────────────────┼─────────────────────────┐
                 │                         │                         │
                 ▼                         ▼                         ▼
              USER ID                  GUILD ID              INSTALLATION ID
                 │                         │                         │
                 │                         │                         │
                 │                         │                PC / Mobile /
                 │                         │                Web installation
                 │                         │
                 └─────────────────────────┼─────────────────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │       FILTRAGE           │
                              │                          │
                              │ • compte                 │
                              │ • serveur                │
                              │ • installation           │
                              │ • plateforme             │
                              │ • version client         │
                              │ • build                  │
                              │ • propriétés client      │
                              │ • autres critères        │
                              └────────────┬─────────────┘
                                           │
                              ┌────────────┴────────────┐
                              │                         │
                              ▼                         ▼
                          ÉLIGIBLE                  NON ÉLIGIBLE
                              │                         │
                              │                         ▼
                              │                    FEATURE OFF
                              │
                              ▼
                  ┌─────────────────────────────┐
                  │ CALCUL / ATTRIBUTION        │
                  │                             │
                  │ Legacy :                    │
                  │ Experiment + Resource ID    │
                  │          │                  │
                  │          ▼                  │
                  │        HASH                 │
                  │          │                  │
                  │          ▼                  │
                  │     ROLLOUT POSITION        │
                  │                             │
                  │ Apex : système plus récent  │
                  │ avec attribution par        │
                  │ installation / contexte     │
                  └──────────────┬──────────────┘
                                 │
                                 ▼
                    ┌────────────────────────┐
                    │   BUCKET / VARIANT      │
                    └────────────┬───────────┘
                                 │
                 ┌───────────────┼────────────────┐
                 │               │                │
                 ▼               ▼                ▼
              NONE          TREATMENT 1      TREATMENT 2
                 │               │                │
                 │               │                │
                 │               └──────┬─────────┘
                 │                      │
                 ▼                      ▼
           FEATURE OFF            FEATURE ON
                                        │
                                        ▼
══════════════════════════════════════════════════════════════════════
                         DISTRIBUTION CLIENT
══════════════════════════════════════════════════════════════════════
                                        │
                         ┌──────────────┼──────────────┐
                         │              │              │
                         ▼              ▼              ▼
                      DESKTOP         MOBILE           WEB
                         │              │              │
                         │              │              │
                    Windows/macOS   iOS/Android     Navigateur
                         │              │              │
                         └──────────────┼──────────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ CLIENT → API      │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ GET EXPERIMENTS   │
                              │                   │
                              │ /experiments      │
                              │ /apex/experiments │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ DISCORD API       │
                              │                   │
                              │ regarde :         │
                              │ • utilisateur     │
                              │ • fingerprint     │
                              │ • installation    │
                              │ • plateforme      │
                              │ • propriétés      │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ RESPONSE          │
                              │                   │
                              │ assignments       │
                              │ + éventuellement  │
                              │ guild experiments │
                              │ + Apex data       │
                              └─────────┬─────────┘
                                        │
                                        ▼
══════════════════════════════════════════════════════════════════════
                              GATEWAY
══════════════════════════════════════════════════════════════════════
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ IDENTIFY          │
                              │                   │
                              │ client properties │
                              │ platform          │
                              │ version           │
                              │ build             │
                              │ installation_id   │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ DISCORD GATEWAY   │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │ READY             │
                              │                   │
                              │ Apex experiments  │
                              │ installation      │
                              │ assignments       │
                              └─────────┬─────────┘
                                        │
                                        ▼
══════════════════════════════════════════════════════════════════════
                              CLIENT
══════════════════════════════════════════════════════════════════════
                                        │
                                        ▼
                         ┌──────────────────────────┐
                         │ EXPERIMENT STORE         │
                         │                          │
                         │ "Cette installation      │
                         │  possède Treatment 1"    │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │ FEATURE FLAG / CONDITION │
                         │                          │
                         │ if (experiment === X)    │
                         │     afficher feature     │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │    UI / API   │
                              │    activée    │
                              └───────────────┘
