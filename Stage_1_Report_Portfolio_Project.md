# Stage 1 Report: Portfolio Project

### 1. Team Formation & Roles Definition

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

*   TBD (Options: BuildForge.GG, TheoryLab.GG, CalcEdge.GG, DeepCalc.GG)

#### **Problem, Solution, and Target**

*   **The Problem:** Many League of Legends players, whether new or experienced, struggle to visualize the real impact of their item and rune choices. They often make mistakes in their builds, which decreases their efficiency and win rate. There is a lack of a centralized tool that combines pure theory (calculations) with user-friendly build creation and comparison.
*   **The Solution:** A web application focused on theorycrafting where users can create precise builds, calculate damage outputs, and compare item choices side-by-side to optimize their performance.
*   **The User Target:** Any League of Legends player wishing to optimize their performance through better understanding of game mechanics, from beginners learning about builds to veteran players wanting to refine their damage calculations.

#### **MVP Scope**

*   **Application Type:** Web Application (website).
*   **In-Scope Features (What the MVP will do):**
    1.  **Authentication System:** Account creation, user login, and logout.
        *   *Data Source: Application Database | Riot API Calls: None*
    2.  **Build Constructor (Theorycraft):** Interface to select a champion, items, runes, and define the level.
        *   *Data Source: Data Dragon (Riot CDN - no rate limits) | Riot API Calls: None*
    3.  **Build Saving:** Logged-in users can save their builds to their account.
        *   *Data Source: Application Database | Riot API Calls: None*
    4.  **"SlotDiff" Comparison Tool:** A comparison table to visualize the impact of two different items on the same build slot against targets with different armor/magic resistance levels.
        *   *Data Source: Data Dragon + Custom calculations | Riot API Calls: None*
    5.  **Damage Calculator per Spell:** A table displaying the damage of each champion spell based on the selected build.
        *   *Data Source: Data Dragon + Custom formulas | Riot API Calls: None*
        *   *Note: Start with 5-10 popular champions, expand gradually*
    6.  **Player Search (Summoner Search):** Search bar allowing to find a player via their Riot ID and view their recent match history.
        *   *Data Source: Riot API | API Calls: ~12 per search (manageable with Development Key)*
        *   *Optimization: 5-minute cache per player*
    7.  **Riot Account Verification:** Users can link their Riot account by changing their in-game profile icon to a randomly generated one, which the app verifies.
        *   *Data Source: Riot API | API Calls: ~2 per verification attempt*
        *   *Enables linking user builds to their Riot account*

*   **Out-of-Scope Features (What the MVP will not do):**
    *   **Meta Tierlist:** Requires millions of API calls to aggregate reliable statistics. Can be added post-MVP with Kaggle datasets or Production API Key.
    *   Automatic import of runes directly into the game client.
    *   Advanced social features (comments, voting on builds).
    *   AI coaching or video replay analysis.
    *   Support for other games (TFT, Valorant).

#### **Risks and Mitigation Strategies**

*   **Risk 1: Complexity and limits of the Riot Games API.**
    *   **Mitigation:** Core features (Build Constructor, Calculator, SlotDiff) use Data Dragon only, which has no rate limits. Player Search uses minimal API calls with caching. Account verification is optional and uses only 2 calls per attempt.
*   **Risk 2: Precision of damage calculations.**
    *   **Mitigation:** Start by implementing formulas for a small group of popular champions (5-10) to validate logic before extending it to all characters.
*   **Risk 3: Workload (Scope Creep).**
    *   **Mitigation:** Strict prioritization. Core theorycrafting features (P0) are independent of API limitations. Player Search and Account Verification are secondary (P1) and can be postponed if needed.

### 4. High-Level Project Plan (Stage 2)

This timeline provides a high-level overview of the project's major phases and key milestones.

*   **Week 1: Foundations**
    *   **Stage 1: Idea Development (Completed):** The project concept, scope, and MVP features were defined.
        *   *Deliverable: Stage 1 Report.*
    *   **Stage 2: Project Planning (Current):** This high-level plan is created.
        *   *Deliverable: Project Timeline.*

*   **Week 2-3: Technical Design**
    *   **Stage 3: Technical Documentation:** Focus on designing the technical architecture before writing code.
        *   *Milestones:*
            *   Define the database schema (user tables, builds, etc.).
            *   Design the API endpoints (e.g., `/api/users/register`, `/api/builds`, `/api/player/:riotId`).
            *   Create simple diagrams for the application flow.

*   **Week 4-10: Development**
    *   **Stage 4: MVP Development:** The core phase of building the application, broken down into sprints.
        *   *Milestones (Week 4-5): Backend Foundations*
            *   Set up the server and project structure.
            *   Implement the database and user authentication system (registration, login).
        *   *Milestones (Week 6-7): Core Logic & API*
            *   Integrate Data Dragon for champion/item/rune data.
            *   Integrate the Riot API for player search and account verification.
            *   Develop the backend logic for saving and retrieving user builds.
        *   *Milestones (Week 8-9): Frontend Implementation*
            *   Develop the user interface for the main pages: Build Constructor, Damage Calculator, SlotDiff, and Player Search.
            *   Connect the frontend to the backend API endpoints.
        *   *Milestones (Week 10): Integration & Testing*
            *   End-to-end testing of all features.
            *   Bug fixing and final adjustments.

*   **Week 11: Finalization**
    *   **Stage 5: Project Closure:** Prepare the project for final submission and presentation.
        *   *Milestones:*
            *   Deploy the application to a live server (or document deployment strategy if Production API Key is required).
            *   Finalize the `README.md` with a link to the live demo and setup instructions.
            *   Prepare the final project presentation.
