# GitLab CI/CD

Référence rapide pour comprendre les pipelines et les runners GitLab CI/CD.

## Qu'est-ce que le CI/CD ?

**CI/CD** (Intégration Continue / Déploiement Continu) automatise la construction, les tests et le déploiement de votre code :

- **CI** (Intégration Continue) : Construire et tester automatiquement le code à chaque commit
- **CD** (Déploiement Continu) : Déployer automatiquement le code en production ou créer des releases

## Pipelines GitLab

### Qu'est-ce qu'un pipeline ?

Un **pipeline** est un flux de travail automatisé qui s'exécute lorsque vous poussez du code sur GitLab.

- Défini dans un fichier `.gitlab-ci.yml` à la racine de votre dépôt
- Composé de **jobs** organisés en **stages** (étapes)
- Chaque job s'exécute indépendamment et peut effectuer des tâches comme :
  - Construire votre application
  - Exécuter des tests
  - Créer des releases
  - Déployer sur des serveurs

### Documentation officielle

- [Pipelines GitLab CI/CD](https://docs.gitlab.com/ee/ci/pipelines/)
- [Référence .gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/)
- [Démarrer avec GitLab CI/CD](https://docs.gitlab.com/ee/ci/quick_start/)

### Stages du pipeline

Les pipelines sont organisés en **stages** qui s'exécutent séquentiellement :

1. **build** — Compiler votre application
2. **test** — Exécuter les tests automatisés
3. **deploy** — Déployer ou créer des releases

Les jobs d'un même stage s'exécutent en parallèle. Le stage suivant ne démarre que si tous les jobs du stage précédent réussissent.

## Runners GitLab

### Qu'est-ce qu'un runner ?

Un **runner** est un agent qui exécute les jobs de votre pipeline.

- S'exécute sur une machine (physique, virtuelle ou conteneur)
- Peut être partagé (fourni par GitLab.com) ou auto-hébergé
- Peut s'exécuter sur différents systèmes d'exploitation : Linux, macOS, Windows

### Documentation officielle

- [GitLab Runner](https://docs.gitlab.com/runner/)
- [Installer GitLab Runner](https://docs.gitlab.com/runner/install/)
- [Exécuteurs de runner](https://docs.gitlab.com/runner/executors/)

### Types de runners

| Type | Description |
|------|-------------|
| **Runners partagés** | Fournis par GitLab.com, disponibles pour tous les projets |
| **Runners de groupe** | Disponibles pour tous les projets d'un groupe |
| **Runners de projet** | Dédiés à un projet spécifique |

### Tags des runners

Les runners peuvent avoir des **tags** pour cibler des environnements spécifiques :

```yaml
build-macos:
  tags:
    - macos
  script:
    - make build
```

Ce job ne s'exécutera que sur les runners ayant le tag `macos`.

## Pourquoi utiliser le CI/CD ?

### Construire à chaque commit

**Problème** : Les builds manuels sont sujets aux erreurs et incohérents.

**Solution** : Construire automatiquement votre code à chaque commit :

- Détecter les erreurs de build immédiatement
- S'assurer que le code compile dans un environnement propre
- Vérifier que les dépendances sont correctement définies
- Obtenir un retour en quelques minutes, pas en heures

### Créer des releases sur un runner macOS

**Cas d'usage** : Créer des releases spécifiques à macOS automatiquement.

**Configuration** :

1. **Installer un runner macOS** sur une machine Mac
2. **Taguer le runner** avec `macos`
3. **Créer un job de release** dans `.gitlab-ci.yml` :

```yaml
release-macos:
  stage: deploy
  tags:
    - macos
  only:
    - tags
  script:
    - make build-release
    - ./upload-to-package-registry.sh
  artifacts:
    paths:
      - build/MyApp.dmg
```

**Avantages** :

- Les releases sont créées automatiquement lorsque vous poussez un tag Git
- Le build s'effectue dans un véritable environnement macOS
- Les binaires peuvent être uploadés dans le registre de paquets génériques
- Builds cohérents et reproductibles
- Aucune étape manuelle requise

## Exemple basique de `.gitlab-ci.yml`

```yaml
stages:
  - build
  - test
  - deploy

build-job:
  stage: build
  script:
    - echo "Construction de l'application..."
    - make build

test-job:
  stage: test
  script:
    - echo "Exécution des tests..."
    - make test

release-job:
  stage: deploy
  tags:
    - macos
  only:
    - tags
  script:
    - echo "Création de la release..."
    - make package
  artifacts:
    paths:
      - dist/
```

## Ressources supplémentaires

- [Exemples CI/CD](https://docs.gitlab.com/ee/ci/examples/)
- [Auto DevOps](https://docs.gitlab.com/ee/topics/autodevops/)
- [Variables CI/CD GitLab](https://docs.gitlab.com/ee/ci/variables/)
- [Artefacts de jobs](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)
