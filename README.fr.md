# 🦊 GitLab

Référence rapide pour gérer les jetons GitLab, les releases et les assets hébergés via le registre de paquets.

## Sommaire
1. [Créer un jeton d'accès personnel (PAT)](#créer-un-jeton-daccès-personnel-pat)
2. [Créer une release](#créer-une-release)
3. [Assets de release dans GitLab (différence importante avec GitHub)](#assets-de-release-dans-gitlab-différence-importante-avec-github)
4. [Comment associer des fichiers à une release (approche recommandée)](#comment-associer-des-fichiers-à-une-release-approche-recommandée)
5. [Uploader des fichiers via le registre de paquets](#uploader-des-fichiers-via-le-registre-de-paquets)
6. [GitHub vs GitLab : ce que « dernière release » signifie vraiment](#github-vs-gitlab--ce-que--dernière-release--signifie-vraiment)
7. [Autres pages](#autres)
8. [CLI pour GitLab](#cli)

---

## Créer un jeton d'accès personnel (PAT)

### Documentation officielle
- Documentation GitLab : [Jetons d'accès personnels](https://docs.gitlab.com/user/profile/personal_access_tokens/)
- Guide des jetons : [The ultimate guide to token management at GitLab](https://about.gitlab.com/blog/the-ultimate-guide-to-token-management-at-gitlab/)

### Liens directs vers la page de configuration des jetons

- **GitLab.com** [Jetons d'accès personnels](https://gitlab.com/-/user_settings/personal_access_tokens)
- **Instance GitLab auto-hébergée / privée** `https://<votre-domaine-gitlab>/-/user_settings/personal_access_tokens`

### Étapes

1. Connectez-vous à GitLab.
2. Cliquez sur votre **avatar** (en haut à droite).
3. Allez dans **Modifier le profil**.
4. Ouvrez **Jetons d'accès personnels**.
5. Cliquez sur **Ajouter un nouveau jeton**.
6. Remplissez :
 - **Nom du jeton**
 - **Date d'expiration** (obligatoire sur les versions récentes de GitLab)
 - **Portées** (par exemple : `api`, `read_repository`)
7. Cliquez sur **Créer le jeton d'accès personnel**.
8. **Copiez le jeton immédiatement** — il ne sera plus affiché ensuite.

### Alternative

- [API OAuth2 de GitLab](https://docs.gitlab.com/api/oauth2/)

---

## Créer une release

### Documentation officielle
- [Releases de projet GitLab](https://docs.gitlab.com/user/project/releases/)

### Qu'est-ce qu'une release dans GitLab ?

Une release est :
- Basée sur un **tag Git**
- Inclut des **notes de version** optionnelles
- Inclut des **assets** optionnels (**LIENS**)
- Fournit automatiquement les archives du code source (`.zip`, `.tar.gz`, etc.)

### Créer une release depuis l'interface web

1. Ouvrez votre **Projet**.
2. Allez dans **Déployer → Releases**.
3. Cliquez sur **Nouvelle release**.
4. Sélectionnez ou créez un **Tag**.
5. Remplissez éventuellement :
 - **Titre de la release**
 - **Notes de version**
 - **Assets (liens)**
6. Cliquez sur **Créer la release**.

---

## Assets de release dans GitLab (différence importante avec GitHub)

### Différence clé

**⚠️ GitLab ne permet PAS d'uploader des fichiers dans une release.**

À la place :
- Les assets d'une release sont des **liens (URLs)**.
- GitLab stocke uniquement le **nom + l'URL**.
- Le fichier doit déjà exister quelque part.

C'est différent de GitHub, où les fichiers sont uploadés directement dans la release.

### Ce que GitLab fournit par défaut

Chaque release inclut automatiquement les archives du **code source** : `.zip`, `.tar.gz`, `.tar.bz2` générées à partir du tag Git.

## Comment associer des fichiers à une release (approche recommandée)

### Concept

Pour associer un fichier (binaire, installateur, archive) à une release :

1. **Uploader le fichier quelque part**
2. **Ajouter un lien vers ce fichier** en tant qu'asset de la release

Le **nom de l'asset** est généralement le **nom du projet** ou de l'artefact.

---

## Uploader des fichiers via le registre de paquets

Si vous souhaitez que GitLab héberge vos fichiers, utilisez le **registre de paquets**.

### Pourquoi utiliser le registre de paquets ?

- Les fichiers sont stockés dans GitLab
- L'accès est authentifié
- Les URLs sont stables et versionnées
- Parfait pour associer des binaires à un tag de release
- Non limité comme GitHub à certains langages

### Documentation officielle

- [Registre de paquets GitLab](https://docs.gitlab.com/user/packages/package_registry/)
- [Registre de paquets génériques GitLab](https://docs.gitlab.com/user/packages/generic_packages/)

### Flux de travail typique

1. **Construisez votre fichier** (par exemple : `MyComponent.zip`)
2. **Uploadez-le dans le registre de paquets génériques PAR code/commande**
  - consultez la documentation pour la commande cURL
  - ou [exemple 4D](https://gist.github.com/e-marchand/b218af0cca0d23f9b0399f42f282221f)
4. Déployer > Registre de paquets pour voir le résultat
5. **Utilisez l'URL du paquet** comme lien d'asset de la release
6. **Associez-le au même tag Git**
Exemple d'URL de paquet pour télécharger via l'API : `[https://<host>/api/v4/projects/<chemin-projet-encodé>/packages/generic/MyComponent/1.0.0/MyComponent.zip](https://<host>/api/v4/projects/<chemin-projet-encodé>/packages/generic/MyComponent/1.0.0/MyComponent.zip)`

----

## GitHub vs GitLab : ce que « dernière release » signifie vraiment

### 🔗 Documentation officielle

- API Releases GitHub
  - « Obtenir la dernière release » : [GitHub REST Releases](https://docs.github.com/en/rest/releases/releases#get-the-latest-release)
  - [Liens vers les releases](https://docs.github.com/en/repositories/releasing-projects-on-github/linking-to-releases)
  - [Gestion des releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
- [API Releases](https://docs.gitlab.com/ee/api/releases/)

### 🗝️ Différence clé

La notion de **« dernière release »** ne peut pas être transposée directement de GitHub à GitLab.

- **GitHub** : « latest » est un concept défini par la plateforme
- **GitLab** : « latest » est une décision côté client

### 🐙 GitHub

GitHub définit et expose explicitement le concept de « dernière release » :

- Distingue les releases stables des pré-releases
- « Dernière release » désigne toujours la version stable la plus récente (ni pré-release, ni brouillon)
- Ce concept est natif et peut être directement consommé par des outils externes (ex : gestionnaires de paquets)

Les consommateurs peuvent se fier à GitHub pour déterminer quelle release est considérée comme « la dernière » sans implémenter de logique supplémentaire.

### 🦊 GitLab

GitLab n'a pas le même modèle conceptuel :

- Ne distingue pas les releases stables des pré-releases
- Toutes les releases sont considérées comme finales
- Aucune notion sémantique de « dernière release stable »

« Dernière » signifie simplement la release la plus récente par date. Le lien permanent vers la dernière release est purement chronologique et ne tient pas compte du versionnage sémantique ni de la stabilité de la release.

### 🛠️ Implication pour l'outillage

Lorsque vous travaillez avec GitLab :

1. Récupérez toutes les releases.
2. Décidez comment déterminer la « dernière » :
   - La plus **récente** par date, ou
   - La **plus haute** version sémantique (logique personnalisée requise)

Cela a des conséquences directes pour les outils d'automatisation et les gestionnaires de paquets, qui doivent implémenter une logique supplémentaire de tri ou de filtrage.

## Autres

- [Pourquoi le choix de l'hôte GitLab est important](gitlab_host_path_explanation.fr.md)
- [GitLab CI/CD : Pipelines et Runners](gitlab_ci_explanation.fr.md)

## CLI

Comme la commande `gh` pour GitHub, GitLab fournit le CLI `glab` :
- https://docs.gitlab.com/cli/

Pour s'authentifier :

```bash
glab auth login
```

Après vous être connecté, vous pouvez utiliser glab pour diverses tâches.

Par exemple, vous pouvez demander à un LLM de générer du code ou des scripts qui utilisent glab pour créer une release, gérer des issues, interagir avec des merge requests, et plus encore.
