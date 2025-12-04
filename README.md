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

L'idée d'un outil de theorycrafting complet a été retenue car elle présente un défi technique bien plus pertinent qu'un simple site vitrine et est plus originale et motivante qu'un clone d'application existante. Elle permet de démontrer un large éventail de compétences full-stack (backend, frontend, base de données, authentification) et l'intégration complexe d'une API tierce (Riot API), ce qui correspond parfaitement aux objectifs d'apprentissage.

### 3. Selected MVP: Decision and Refinement (Task 2)

#### **Nom Provisoire du Projet**

*   *SlotDiff*

#### **Problème, Solution et Cible**

*   **Le Problème :** De nombreux joueurs de League of Legends, qu'ils soient nouveaux ou expérimentés, peinent à visualiser l'impact réel de leurs choix d'items et de runes. Ils font souvent des erreurs dans leurs builds, ce qui diminue leur efficacité et leur taux de victoire. Il manque un outil centralisé qui combine théorie pure (calculs) et données réelles (matchs/tierlists) de manière simple.
*   **La Solution :** Une application web tout-en-un où un utilisateur peut non seulement théoriser des builds précis avec des comparatifs de dégâts, mais aussi consulter la "meta" actuelle via une tierlist et analyser l'historique de joueurs réels.
*   **La Cible Utilisateur :** Tout joueur de League of Legends souhaitant optimiser ses performances, du débutant cherchant les meilleurs champions du moment au joueur vétéran voulant affiner ses calculs de dégâts.

#### **Périmètre du MVP (Scope)**

*   **Type d'application :** Application Web (site internet).
*   **Fonctionnalités In-Scope (ce que le MVP fera) :**
    1.  **Système d'Authentification :** Création de compte, connexion et déconnexion des utilisateurs.
    2.  **Constructeur de Build (Theorycraft) :** Interface pour sélectionner un champion, des items, des runes et définir le niveau.
    3.  **Sauvegarde de Builds :** Les utilisateurs connectés peuvent sauvegarder leurs builds sur leur compte.
    4.  **Outil de Comparaison "SlotDiff" :** Un tableau comparatif pour visualiser l'impact de deux items différents sur un même slot de build, contre des cibles avec différents niveaux d'armure/résistance magique.
    5.  **Calculateur de Dégâts par Sort :** Un tableau affichant les dégâts de chaque sort du champion en fonction du build sélectionné.
    6.  **Recherche de Joueur (Summoner Search) :** Barre de recherche permettant de trouver un joueur via son Riot ID et de consulter son historique de matchs récents.
    7.  **Tierlist Meta :** Une page dédiée affichant le classement des meilleurs champions par rôle (Top, Jungle, Mid, ADC, Support) basé sur les données actuelles.
    8.  **Intégration API Riot :** Utilisation de l'API pour récupérer les données statiques (items/champions), les données de matchs (historique) et les données de ligue.

*   **Fonctionnalités Out-of-Scope (ce que le MVP ne fera pas) :**
    *   Importation automatique des runes directement dans le client du jeu.
    *   Fonctionnalités sociales avancées (commentaires, votes sur les builds).
    *   Coaching IA ou analyse de replay vidéo.
    *   Support pour d'autres jeux (TFT, Valorant).

#### **Risques et Stratégies d'Atténuation**

*   **Risque 1 : Complexité et limites de l'API Riot Games.**
    *   **Atténuation :** L'ajout de l'historique et des tierlists demande beaucoup de requêtes. Il faudra mettre en place un système de mise en cache (caching) efficace pour ne pas dépasser les "rate limits" et éviter de rappeler l'API inutilement pour les mêmes données.
*   **Risque 2 : Précision des calculs de dégâts.**
    *   **Atténuation :** Commencer par implémenter les formules pour un petit groupe de champions populaires pour valider la logique avant de l'étendre à tous les personnages.
*   **Risque 3 : Charge de travail (Scope Creep).**
    *   **Atténuation :** Avec l'ajout des fonctionnalités de recherche et de tierlist, le projet est ambitieux. L'utilisation stricte de GitHub Projects pour prioriser les tâches sera cruciale. Si le temps manque, la partie "Tierlist" pourra être simplifiée (ex: mise à jour manuelle ou moins fréquente) pour privilégier le cœur du projet (Theorycraft + Recherche).

