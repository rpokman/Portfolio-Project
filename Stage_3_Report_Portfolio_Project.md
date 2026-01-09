# Technical Documentation: Portfolio-Project (BuildForge)

## 0. Define User Stories and Mockups

### Prioritized User Stories (MoSCoW)
Based on the MVP features defined in the project scope (Authentication, Build Constructor, Saving, Comparison).

**Must Have (Critical):**
*   "As a **player**, I want to **register and log in**, so that I can secure my account and save my work."
*   "As a **theorycrafter**, I want to **select a champion, items, and runes**, so that I can see the final calculated statistics of my build."
*   "As a **player**, I want to **view calculated damage for each spell**, so that I can understand my burst potential."

**Should Have (Important):**
*   "As a **logged-in user**, I want to **name and save my builds**, so that I can retrieve them later."
*   "As an **analyst**, I want to **use the 'SlotDiff' tool to compare two items side-by-side**, so that I can choose the most efficient option against specific resistances."

**Could Have (Nice to have):**
*   "As a **user**, I want to **search for a summoner by Riot ID**, so that I can view their recent match history."
*   "As a **user**, I want to **verify my Riot account via profile icon**, so that I can link my in-game identity."

### Mockups Guidelines
*   **Build Constructor Screen:** A central interface featuring the champion portrait, 6 item slots (empty or filled), runes selection, and a side panel displaying real-time calculated stats (AD, AP, Armor) and spell damage breakdowns (Q, W, E, R).
*   **SlotDiff Comparison Screen:** A simple two-column comparison table (Item A vs. Item B) showing damage differences against a configurable "Target Dummy" (Health/Resistances).

---

## 1. Design System Architecture

### High-Level Architecture Diagram
The project follows a modern web application architecture separating the frontend client from the backend API.

*   **Front-end:** Web Application (likely React or Vue.js) responsible for the User Interface.
*   **Back-end:** **FastAPI** (Python) exposing a RESTful API. Handles business logic (damage calculations), authentication, and data validation.
*   **Database:** **PostgreSQL** storing `Users` and their saved `Builds`.
*   **External Services:**
    *   **Riot Data Dragon (CDN):** For retrieving static assets (images, champion data, item JSONs) with no rate limits.
    *   **Riot API:** For dynamic data (Summoner lookups, Match History).

### Data Flow
1.  The **Frontend** fetches static data (images/icons) directly from **Data Dragon**.
2.  For user actions (login, save build), the Frontend sends JSON requests to the **FastAPI Backend**.
3.  The Backend processes logic and interacts with **PostgreSQL** via **SQLAlchemy**.

---

## 2. Define Components, Classes, and Database Design

### Database Schema (Relational - PostgreSQL)
*   **Table `users`**
    *   `id`: Integer, Primary Key
    *   `email`: String, Unique, Not Null
    *   `password_hash`: String, Not Null
    *   `riot_account_id`: String, Optional
    *   `is_verified`: Boolean, Default False

*   **Table `builds`**
    *   `id`: Integer, Primary Key
    *   `user_id`: Integer, Foreign Key (`users.id`)
    *   `champion_id`: String (e.g., "Ahri"), Not Null
    *   `items`: JSON/String (List of Item IDs)
    *   `runes`: JSON/String (Runes configuration)
    *   `created_at`: DateTime, Default Now

### Key Backend Classes
*   **`User` (SQLAlchemy Model):** Represents a registered user entity.
*   **`Build` (SQLAlchemy Model):** Represents a specific build configuration saved by a user.
*   **`DamageCalculator` (Service Class):** Contains the core business logic for theorycrafting.
    *   *Method:* `calculate_spell_damage(champion_stats, item_stats, spell_ratios) -> DamageResult`

---

## 3. Create High-Level Sequence Diagrams

### Use Case: Saving a New Build
1.  **User** clicks "Save Build" on the Frontend interface.
2.  **Frontend** sends a `POST /api/builds` request with the JWT token and build data (champion ID, items list).
3.  **Backend (FastAPI)** verifies the JWT signature (Authentication).
4.  **Backend** validates the payload (e.g., checking if item IDs are valid).
5.  **Backend** calls **SQLAlchemy** to insert the new record into the `builds` table.
6.  **Database** confirms the insertion and returns the new Build ID.
7.  **Backend** responds with `201 Created` and the build object.
8.  **Frontend** displays a "Build Saved Successfully" notification.

---

## 4. Document External and Internal APIs

### External APIs
*   **Riot Data Dragon:** Used for static data (Champions.json, Items.json) and images. Selected for its stability and lack of rate limits.
*   **Riot Platform API:** Used for `summoner-v4` (profile lookup) and `match-v4` (history).

### Internal API (FastAPI Endpoints)

| Method | Endpoint | Description | Input (Body/Query) | Output |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | `/auth/register` | Register a new user | `{"email": "...", "password": "..."}` | `{"id": 1, "email": "..."}` |
| `POST` | `/auth/token` | User Login | `{"username": "...", "password": "..."}` | `{"access_token": "...", "token_type": "bearer"}` |
| `GET` | `/builds` | Get current user's builds | Header: `Authorization: Bearer <token>` | `[{"id": 1, "champion": "Ahri", ...}]` |
| `POST` | `/builds` | Save a new build | `{"champion_id": "Ahri", "items": [...]}` | `{"id": 2, "status": "saved"}` |
| `GET` | `/player/{name}/{tag}` | Search for a player | Path: `name`, `tag` | `{"summonerLevel": 120, "iconId": ...}` |

---

## 5. Plan SCM and QA Strategies

### SCM (Source Control Management) Strategy
*   **Tool:** Git & GitHub.
*   **Branching Model:**
    *   `main`: Production-ready code only.
    *   `develop`: Integration branch for ongoing development.
    *   `feat/feature-name`: Feature branches for specific tasks (e.g., `feat/auth`, `feat/calculator`).
*   **Workflow:** Code is pushed to feature branches and merged into `develop` via Pull Requests.

### QA (Quality Assurance) Strategy
*   **Unit Testing:** Use **Pytest** for the backend.
    *   *Critical:* Test the `DamageCalculator` class logic extensively to ensure math accuracy for the theorycrafting aspect.
    *   *Standard:* Test API endpoints for correct HTTP responses (200 OK, 401 Unauthorized).
*   **Integration Testing:** Verify database interactions (Create/Read operations) using a test database.
*   **Manual Testing:** User Interface testing to ensure responsiveness and correct data binding (e.g., selecting an item updates the stats panel correctly).
