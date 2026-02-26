# Pourquoi le choix de l'hôte GitLab est important

Lorsqu'un utilisateur fournit une URL de dépôt comme :

    https://un.serveur/p1/p2/p3

nous ne pouvons pas toujours déterminer le chemin du dépôt directement à partir de l'URL.

## Le problème

GitLab peut être installé de deux façons.

### 1) À la racine du domaine

    https://un.serveur/

Exemple de dépôt :

    https://un.serveur/p1/p2/p3

Chemin du projet :

    p1/p2/p3

------------------------------------------------------------------------

### 2) Sous un préfixe de chemin (URL racine relative)

    https://un.serveur/gitlab/

Exemple de dépôt :

    - https://un.serveur/gitlab/p2/p3
    - https://un.serveur/gitlab/p2/p4/p5

Chemin du projet :

    - p2/p3
    - p2/p4/p5

------------------------------------------------------------------------

Si un utilisateur colle :

    https://un.serveur/p1/p2/p3

nous ne pouvons pas savoir si :

-   `p1` est un groupe, ou\
-   `p1` est le chemin d'installation de GitLab.

Sans connaître l'URL de base de GitLab, le chemin du dépôt est ambigu.

------------------------------------------------------------------------

## Pourquoi la sélection de l'hôte est nécessaire

Pour résoudre correctement le projet :

-   Nous devons connaître l'URL de base de GitLab.
-   Le chemin du projet est tout ce qui suit cette URL de base.

Autrement dit :

Chemin du projet = Chemin complet de l'URL − Chemin de base GitLab

------------------------------------------------------------------------

## Approche UX recommandée

### Option 1 — L'utilisateur sélectionne l'hôte GitLab (recommandé)

-   L'utilisateur colle l'URL complète du dépôt.
-   L'utilisateur sélectionne l'hôte GitLab dans un menu déroulant.
-   Le système supprime automatiquement l'hôte + le chemin de base.
-   Le chemin restant devient le chemin du projet.

C'est la solution la plus fiable et prévisible.

------------------------------------------------------------------------

### Option 2 — Détection automatique (au mieux)

-   Essayer de détecter `/api/v4/version`. /!\ MAIS NÉCESSITE UN JETON
-   Si non trouvé, essayer avec le premier segment du chemin comme préfixe.
-   Suggérer l'URL de base détectée à l'utilisateur.
-   Laisser l'utilisateur confirmer ou corriger.

La détection peut aider, mais la confirmation de l'utilisateur reste importante.

------------------------------------------------------------------------

## Conclusion

Parce que GitLab peut être installé sous un préfixe de chemin,\
la même structure d'URL peut signifier des choses différentes.

Pour éviter une résolution incorrecte du dépôt,\
l'hôte GitLab (y compris son chemin de base) doit être connu ou sélectionné
explicitement.
