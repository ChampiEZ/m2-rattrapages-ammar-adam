# Gestion de tâches — Application Flutter

Application mobile Flutter réalisée dans le cadre du module d'architecture
mobile. Sujet fonctionnel : gestion de tâches.

> Ce README sera complété au fur et à mesure de l'avancement du projet
> (fonctionnalités, données, état, tests, justification finale).

## Développement et tests

Le projet a été développé et testé uniquement via **Chrome** (`flutter run -d
chrome`). Xcode a été essayé pour tester sur simulateur iOS, mais la machine
utilisée ne supportait pas bien la charge (compilation très lente, plantages
système). Les dossiers `android/` et `ios/` ont donc été retirés du projet.

## Architecture

Le projet suit une **Clean Architecture**, organisée par feature. Chaque
feature est décomposée en 3 couches indépendantes :

- **`presentation`** : écrans, widgets et gestion d'état. Ne contient
  aucune logique métier ni appel réseau direct.
- **`domain`** : entités et cas d'usage (règles métier), ainsi que les
  interfaces de repository. Ne dépend ni de Flutter, ni d'une source de
  données concrète.
- **`data`** : implémentation des repositories, sources de données
  (API REST, stockage local) et modèles de sérialisation.

La couche `domain` ne dépend jamais de `data` : c'est `data` qui implémente
les interfaces définies par `domain` (inversion de dépendance). La couche
`presentation` ne dépend que de `domain`.

```
lib/
├── core/                        # code transverse, partagé entre features
│   ├── error/                   # erreurs métier normalisées (Failure)
│   └── usecases/                # contrat de base des cas d'usage
├── features/
│   └── tasks/
│       ├── data/
│       │   ├── datasources/     # accès API REST / stockage local
│       │   ├── models/          # DTO / sérialisation JSON
│       │   └── repositories/    # implémentation de TaskRepository
│       ├── domain/
│       │   ├── entities/        # Task (entité métier pure)
│       │   ├── repositories/    # TaskRepository (interface)
│       │   └── usecases/        # règles métier (GetTasks, CreateTask...)
│       └── presentation/
│           ├── pages/           # écrans
│           ├── widgets/         # composants UI réutilisables
│           └── state/           # gestion d'état de la feature
└── main.dart
```

## Getting Started

```bash
flutter pub get
flutter run -d chrome
```
