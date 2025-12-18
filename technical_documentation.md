# Technical Documentation - BuildForge: LoL Build Manager

## 0. User Stories & Mockups

### User Stories
We have prioritized our user stories using the **MoSCoW** method to ensure the MVP focuses on core value.

#### Must Have (Critical for MVP)
*   **Champion Selection:** "As a user, I want to search and select a champion from the full roster so that I can start creating a specific build for them."
*   **Item Selection:** "As a user, I want to fill my 6 item slots and 1 trinket slot with valid items so that I can visualize my full in-game equipment."
*   **Build Validation:** "As a user, I want to be prevented from selecting incompatible items (e.g., two pairs of boots, Mythic restrictions if applicable) so that my build is realistic."
*   **Rune Configuration:** "As a user, I want to select a Primary and Secondary Rune path so that I can complete my champion's setup."

#### Should Have (Important)
*   **Build Saving:** "As a registered user, I want to save my builds with a custom name so that I can access them later."
*   **Build Dashboard:** "As a registered user, I want to view a list of my saved builds so that I can edit or delete them."

#### Could Have (Nice to have)
*   **Stat Calculation:** "As a user, I want to see the total stats (AP, AD, Haste) of my build so that I can analyze its theoretical power."
*   **Sharing:** "As a user, I want to generate a shareable link for my build so that I can show it to friends."

### Mockups
*Mockups are visual representations of these stories.*
1.  **Champion Select (Home):** Grid of champion portraits + Search Bar.
2.  **The Forge (Builder):** Central view with Champion stats (left), Item slots (center), and Rune tree (right).
3.  **User Dashboard:** List of saved builds with "Edit" and "Delete" actions.

---

## 1. System Architecture

### High-Level Architecture Diagram
This diagram illustrates the separation of concerns between our Client, Server, and Data layers.

![System Architecture](images/System%20Architecture%20Diagram%20BuildForge.png)

### Data Flow
1.  **Initialization:** The Front-End fetches static data (Items, Champions) directly from Riot's **Data Dragon** (CDN) to reduce server load.
2.  **User Action:** When a user saves a build, the Front-End sends a JSON payload to the FastAPI Back-End.
3.  **Processing:** FastAPI validates the token (Auth) and the build data.
4.  **Storage:** Valid data is written to **PostgreSQL** via SQLAlchemy.
5.  **Retrieval:** When loading "My Builds", the Back-End queries PostgreSQL and returns a list of build objects to the Front-End.

---

## 2. Components, Classes, and Database Design

### Back-End Classes (FastAPI/Pydantic Models)

*   **User:** Handles authentication and identification.
    *   `id` (int): Unique identifier.
    *   `username` (str): Display name.
    *   `email` (str): Login credential.
    *   `password_hash` (str): Secured password.
*   **Build:** Represents a complete configuration.
    *   `id` (int): Unique identifier.
    *   `title` (str): Name of the build (e.g., "Tank Teemo").
    *   `champion_id` (str): Riot API ID (e.g., "Teemo").
    *   `items` (List[int]): Array of item IDs.
    *   `runes` (JSON): Structure containing selected runes.
    *   `user_id` (int): Foreign key to User.

### Database Design (ER Diagram)

We use a Relational Database (**PostgreSQL**) to ensure data integrity between Users and their Builds.

![Database Design](images/Database%20Design%20Diagram%20BuildForge.png)

---

## 3. High-Level Sequence Diagrams

### Use Case 1: User Saves a New Build

![User Saves Build](images/User%20Saves%20a%20New%20Build%20Diagram%20BuildForge.png)

### Use Case 2: User Logs In

![User Login](images/User%20Logs%20In%20Diagram%20BuildForge.png)

---

## 4. API Specifications

### External APIs
*   **Riot Games API (Data Dragon):**
    *   **Why:** Official source of truth for League of Legends data (Champions, Items, Runes, Icons). It provides static JSON files and images.
    *   **Usage:** Fetched primarily by the Front-End to display images and descriptions without taxing our own database.

### Internal API (FastAPI Endpoints)

| Method | Endpoint | Description | Request Body (Input) | Response (Output) |
| :--- | :--- | :--- | :--- | :--- |
| **POST** | `/auth/register` | Register new user | `{ "username": "...", "email": "...", "password": "..." }` | `{ "id": 1, "username": "..." }` |
| **POST** | `/auth/login` | Log in user | `{ "username": "...", "password": "..." }` | `{ "access_token": "...", "token_type": "bearer" }` |
| **GET** | `/builds` | Get user's builds | *Header: Authorization* | `[ { "id": 1, "title": "...", "champion": "..." }, ... ]` |
| **POST** | `/builds` | Create new build | `{ "title": "...", "champion_id": "...", "items": [...] }` | `{ "id": 2, "message": "Build created" }` |
| **GET** | `/builds/{id}` | Get specific build | *None* | `{ "id": 1, "title": "...", "items": [...] }` |
| **DELETE**| `/builds/{id}` | Delete a build | *Header: Authorization* | `{ "message": "Deleted successfully" }` |

---

## 5. SCM and QA Strategies

### Source Control Management (SCM)
*   **Versioning:** Git hosted on GitHub.
*   **Branching Strategy:**
    *   `main`: Production-ready code. No direct commits allowed.
    *   `dev`: Integration branch. All features merge here first.
    *   `feature/feature-name`: Temporary branches for specific tasks (e.g., `feature/auth-login`, `feature/champion-grid`).
*   **Workflow:**
    1.  Create a feature branch from `dev`.
    2.  Develop and test locally.
    3.  Open a Pull Request (PR) to `dev`.
    4.  Code Review by at least one peer before merging.

### Quality Assurance (QA) Strategy
*   **Testing Types:**
    *   **Unit Testing (Back-End):** Using `pytest` to test API endpoints and database models in isolation.
    *   **Manual Testing:** Verifying UI flows (Drag & Drop, Saving) manually before each merge.
*   **Tools:**
    *   **Pytest:** For Python backend tests.
    *   **Postman:** For manual API endpoint testing.
    *   **ESLint/Prettier:** To ensure code style consistency on the Front-End.

---

## 6. Technical Justifications

*   **FastAPI:** Chosen for its speed (async support), automatic documentation (Swagger UI), and strict data validation with Pydantic, which reduces bugs significantly compared to Flask.
*   **PostgreSQL:** Chosen over MongoDB because our data is structured (Users have Builds) and relational integrity is important for account management.
*   **React + Vite:** React is the industry standard for dynamic UIs. Vite is selected for its superior build performance and developer experience compared to Create React App.
*   **Riot Data Dragon:** Using the static CDN instead of the live API endpoints allows us to avoid rate limits and ensures the app works fast even if Riot's game servers are busy.
