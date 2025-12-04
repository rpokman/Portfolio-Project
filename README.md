# Stage 1 Report: Portfolio Project

### 1. Team Formation & Roles Definition (Task 0)

*   **Équipe :** Projet solo.
*   **Membre :** rpokman
    *   **Description :** Développeur en formation, passionné par l'écosystème de League of Legends et le développement full-stack. L'objectif de ce projet est de monter en compétences sur la création d'une application web de A à Z, de la base de données à l'interface utilisateur, en passant par l'intégration d'une API externe.
*   **Rôles Assumés :**
    *   **Project Manager :** Responsable de la planification, de la définition du périmètre (scope) et du respect des objectifs du MVP.
    *   **Full-Stack Developer :** En charge du développement du backend (serveur, base de données, logique métier, authentification) et du frontend (interface utilisateur, expérience utilisateur).
    *   **API Integrator :** Gère la connexion, les appels et le traitement des données provenant de l'API externe de Riot Games.
    *   **Documentation Owner :** Responsable de la rédaction et de la maintenance de la documentation du projet.
*   **Outils de Collaboration & Documentation :**
    *   **Gestion de projet et de version :** GitHub (incluant les README pour la documentation et GitHub Projects pour le suivi des tâches).

### 2. Idea Selection and Justification (Task 1)

L'idée de ce projet était claire dès le départ, motivée par un intérêt personnel pour League of Legends et l'envie de créer un outil utile à la communauté. Plutôt qu'un brainstorming dispersé, la réflexion s'est concentrée sur la validation de cette unique idée en la comparant aux alternatives classiques pour un projet de portfolio.

L'idée d'un outil de theorycrafting a été retenue car elle présente un défi technique bien plus pertinent qu'un simple site vitrine et est plus originale et motivante qu'un clone d'application existante. Elle permet de démontrer un large éventail de compétences full-stack (backend, frontend, base de données, authentification) et l'intégration d'une API tierce, ce qui correspond parfaitement aux objectifs d'apprentissage.

### 3. Selected MVP: Decision and Refinement (Task 2)

#### **Nom Provisoire du Projet**

*   *LoL Theorycrafter*

#### **Problème, Solution et Cible**

*   **Le Problème :** De nombreux joueurs de League of Legends, qu'ils soient nouveaux ou expérimentés, peinent à visualiser l'impact réel de leurs choix d'items et de runes. Ils font souvent des erreurs dans leurs builds, ce qui diminue leur efficacité et leur taux de victoire. Il n'existe pas d'outil simple et centralisé pour tester rapidement des théories sans lancer une partie.
*   **La Solution :** Une application web centralisée où un utilisateur peut sélectionner un champion, lui assigner un build complet (items, runes, niveau) et obtenir des calculs de dégâts précis et des comparatifs clairs, le tout basé sur les données officielles de l'API Riot.
*   **La Cible Utilisateur :** Tout joueur de League of Legends souhaitant optimiser ses performances, du débutant cherchant à comprendre les mécaniques de base au joueur vétéran voulant tester des builds de niche.

#### **Périmètre du MVP (Scope)**

*   **Type d'application :** Application Web (site internet).
*   **Fonctionnalités In-Scope (ce que le MVP fera) :**
    1.  **Système d'Authentification :** Création de compte, connexion et déconnexion des utilisateurs.
    2.  **Constructeur de Build :** Interface pour sélectionner un champion, des items, des runes et définir le niveau.
    3.  **Sauvegarde de Builds :** Les utilisateurs connectés peuvent sauvegarder leurs builds sur leur compte.
    4.  **Calculateur de Dégâts par Sort :** Un tableau affichant les dégâts de chaque sort du champion en fonction du build et du niveau sélectionnés.
    5.  **Outil de Comparaison d'Items :** Un tableau comparatif pour visualiser l'impact de deux items différents sur un même slot de build, contre des cibles avec différents niveaux d'armure/résistance magique.
    6.  **Intégration API Riot :** Utilisation de l'API pour récupérer les données à jour des champions et des items.
*   **Fonctionnalités Out-of-Scope (ce que le MVP ne fera pas) :**
    *   Importation automatique de builds depuis l'historique de parties d'un joueur.
    *   Fonctionnalités sociales (partage, commentaires, votes).
    *   Guides de jeu complets ou articles de blog.
    *   Support pour d'autres jeux que League of Legends.

#### **Risques et Stratégies d'Atténuation**

*   **Risque 1 : Complexité de l'API Riot Games.**
    *   **Atténuation :** Commencer par intégrer un seul type de données (ex: data statique des champions via Data Dragon) avant de passer aux appels plus complexes. Gérer attentivement les limites de requêtes (rate limits) imposées par l'API.
*   **Risque 2 : Complexité des calculs de dégâts.**
    *   **Atténuation :** Commencer par implémenter les calculs pour un seul champion et des formules de base. Valider les calculs manuellement avant de généraliser la logique.
*   **Risque 3 : Dépassement du périmètre (Scope Creep).**
    *   **Atténuation :** Se tenir strictement aux fonctionnalités définies pour le MVP. Utiliser un board de gestion de projet (GitHub Projects) pour prioriser les tâches et ne pas ajouter de nouvelles fonctionnalités avant que le MVP soit complet.

