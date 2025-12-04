# Stage 1 Report: Portfolio Project

### 1. Team Formation & Roles Definition (Task 0)

*   **Team:** Solo Project.
*   **Member:** rpokman
    *   **Description:** Developer in training, passionate about the League of Legends ecosystem and full-stack development. The goal of this project is to develop skills in creating a web application from A to Z, from the database to the user interface, including the integration of an external API.
*   **Assumed Roles:**
    *   **Project Manager:** Responsible for planning, defining the scope, and ensuring MVP objectives are met.
    *   **Full-Stack Developer:** In charge of backend development (server, database, business logic, authentication) and frontend (user interface, user experience).
    *   **API Integrator:** Manages the connection, API calls, and processing of data coming from the Riot Games external API.
    *   **Documentation Owner:** Responsible for writing and maintaining project documentation.
*   **Collaboration & Documentation Tools:**
    *   **Project Management and Versioning:** GitHub (including READMEs for documentation and GitHub Projects for task tracking).

### 2. Idea Selection and Justification (Task 1)

The idea for this project was clear from the start, motivated by a personal interest in League of Legends and the desire to create a useful tool for the community. Rather than a scattered brainstorming session, the reflection focused on validating this single idea by comparing it to classic alternatives for a portfolio project.

The idea of a comprehensive theorycrafting tool was chosen because it presents a much more relevant technical challenge than a simple showcase site and is more original and motivating than a clone of an existing application. It allows demonstrating a wide range of full-stack skills (backend, frontend, database, authentication) and the complex integration of a third-party API (Riot API), which corresponds perfectly to learning objectives.

### 3. Selected MVP: Decision and Refinement (Task 2)

#### **Provisional Project Name**

*   *SlotDiff*

#### **Problem, Solution, and Target**

*   **The Problem:** Many League of Legends players, whether new or experienced, struggle to visualize the real impact of their item and rune choices. They often make mistakes in their builds, which decreases their efficiency and win rate. There is a lack of a centralized tool that combines pure theory (calculations) and real data (matches/tierlists) in a simple way.
*   **The Solution:** An all-in-one web application where a user can not only theorize precise builds with damage comparisons but also consult the current "meta" via a tierlist and analyze the history of real players.
*   **The User Target:** Any League of Legends player wishing to optimize their performance, from the beginner looking for the best current champions to the veteran player wanting to refine their damage calculations.

#### **MVP Scope**

*   **Application Type:** Web Application (website).
*   **In-Scope Features (What the MVP will do):**
    1.  **Authentication System:** Account creation, user login, and logout.
    2.  **Build Constructor (Theorycraft):** Interface to select a champion, items, runes, and define the level.
    3.  **Build Saving:** Logged-in users can save their builds to their account.
    4.  **"SlotDiff" Comparison Tool:** A comparison table to visualize the impact of two different items on the same build slot against targets with different armor/magic resistance levels.
    5.  **Damage Calculator per Spell:** A table displaying the damage of each champion spell based on the selected build.
    6.  **Player Search (Summoner Search):** Search bar allowing to find a player via their Riot ID and view their recent match history.
    7.  **Meta Tierlist:** A dedicated page displaying the ranking of the best champions by role (Top, Jungle, Mid, ADC, Support) based on current data.
    8.  **Riot API Integration:** Use of the API to retrieve static data (items/champions), match data (history), and league data.

*   **Out-of-Scope Features (What the MVP will not do):**
    *   Automatic import of runes directly into the game client.
    *   Advanced social features (comments, voting on builds).
    *   AI coaching or video replay analysis.
    *   Support for other games (TFT, Valorant).

#### **Risks and Mitigation Strategies**

*   **Risk 1: Complexity and limits of the Riot Games API.**
    *   **Mitigation:** Adding history and tierlists requires many requests. An efficient caching system will need to be implemented to not exceed "rate limits" and avoid calling the API unnecessarily for the same data.
*   **Risk 2: Precision of damage calculations.**
    *   **Mitigation:** Start by implementing formulas for a small group of popular champions to validate logic before extending it to all characters.
*   **Risk 3: Workload (Scope Creep).**
    *   **Mitigation:** With the addition of search and tierlist features, the project is ambitious. Strict use of GitHub Projects to prioritize tasks will be crucial. If time runs short, the "Tierlist" part can be simplified (e.g., manual update or less frequent) to prioritize the core of the project (Theorycraft + Search).
