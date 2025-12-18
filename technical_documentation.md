# Technical Documentation — BuildForge (LoL Theorycrafting & Build Manager)

## 0. Purpose & MVP Alignment

BuildForge is a web application focused on League of Legends theorycrafting: build creation, validation, damage/stat calculations, and optional Riot-powered player features.
This technical documentation is aligned with the MVP scope described in the repository README:
- Authentication (register/login/logout).
- Build constructor (champion/items/runes/level) using Data Dragon.
- Build saving (per user).
- SlotDiff comparison tool.
- Damage calculator per spell (start with a limited champion pool).
- Summoner search (recent matches).
- Riot account verification (profile icon challenge).

---

## 1. User Stories & Mockups (MoSCoW)

### Must Have (Critical for MVP)
* **Authentication:** "As a user, I want to create an account and log in so that I can save and manage my builds."
* **Champion selection:** "As a user, I want to select a champion so that I can create a build for them."
* **Item selection:** "As a user, I want to fill 6 item slots + 1 trinket slot so that I can simulate my full equipment."
* **Build validation:** "As a user, I want to be prevented from selecting incompatible items (e.g., duplicate boots) so that the build is realistic."
* **Rune configuration:** "As a user, I want to select Primary and Secondary rune paths so that the build matches in-game rules."
* **SlotDiff comparison:** "As a user, I want to compare two items in the same slot against different resistances so that I can decide the best choice."
* **Damage per spell (limited champions):** "As a user, I want to see spell damages based on my build so that I can theorycraft outcomes."

### Should Have (Important / P1)
* **Build saving:** "As a logged-in user, I want to save builds with a name so that I can reuse them."
* **Build dashboard:** "As a logged-in user, I want to list, view, delete my saved builds so that I can manage them."
* **Summoner search:** "As a user, I want to search a player by Riot ID and view recent matches so that I can inspect activity/build choices."
* **Riot account verification:** "As a logged-in user, I want to link my Riot account via a profile icon challenge so that my account can be verified."

### Could Have (Nice to have / Post-MVP)
* **Shareable build links:** "As a user, I want to share my build via a link so that I can send it to friends."
* **Full champion coverage for damage formulas** (expand gradually after correctness is validated).
* **Advanced caching & background refresh jobs** (e.g., match history snapshots).

### Mockups (Textual)
1. **Home / Champion Select:** champion grid + search bar.
2. **Builder ("The Forge"):** champion panel, item slots, runes panel, level selector, calculators (SlotDiff + spell damage).
3. **My Builds:** list + view details + delete.
4. **Summoner Search:** search bar + profile header + recent matches list.
5. **Account Linking:** verification screen with icon challenge steps.

---

## 2. System Architecture

### High-Level Architecture Diagram
![System Architecture](images/System%20Architecture%20Diagram%20BuildForge.png)

### Components
- **Front-End (Vanilla JS + Tailwind CSS):**
  - **HTML5:** Semantic structure of the application.
  - **Tailwind CSS:** Utility-first styling for a responsive and modern UI.
  - **Vanilla JavaScript (ES6+):** Direct DOM manipulation to handle interactivity (champion selection, item slots logic, API calls). No heavy framework.
  - Fetches static game data from **Data Dragon** (CDN).
- **Back-End (FastAPI):**
  - Authentication (JWT).
  - Build CRUD & validation rules.
  - Riot API gateway endpoints.
  - Caching layer for Riot responses (5-minute cache for player search).
- **Database (PostgreSQL + SQLAlchemy):**
  - Users, builds, and linked Riot account data.
- **External Sources:**
  - **Riot Data Dragon (static)**: champions, items, runes, images.
  - **Riot Developer API (live)**: account, summoner profile, match history.

### Data Flow (Typical)
1. **Initialization:** `script.js` fetches champion/item data from Data Dragon on page load.
2. **Interactivity:** JS Event Listeners handle user clicks (e.g., adding an item to a slot).
3. **Build Save:** A simple `fetch()` request sends the JSON payload to the FastAPI backend.
4. **Summoner search:** Front-End requests FastAPI `/players/search`; FastAPI calls Riot API and caches results for 5 minutes.
5. **Account verification:** FastAPI issues a random icon challenge; user changes icon; FastAPI verifies via Riot profile call.

---

## 3. Domain Model & Database Design

### Core Entities (Conceptual)
* **User**
  * `id` (int)
  * `username` (str)
  * `email` (str)
  * `password_hash` (str)
  * `created_at` (datetime)
* **Build**
  * `id` (int)
  * `title` (str)
  * `champion_id` (str) — e.g. "Teemo"
  * `level` (int)
  * `items` (int[]) — item IDs
  * `trinket` (int | null)
  * `runes` (jsonb)
  * `user_id` (int, FK -> User)
  * `created_at` (datetime)
* **RiotAccountLink** (optional but recommended for verification feature)
  * `id` (int)
  * `user_id` (int, FK -> User)
  * `game_name` (str)
  * `tag_line` (str)
  * `puuid` (str)
  * `region` (str)
  * `verified_at` (datetime | null)

### Database Design (ER Diagram)
![Database Design](images/Database%20Design%20Diagram%20BuildForge.png)

---

## 4. Key Use Cases (Sequences)

### Use Case 1: User Saves a New Build
![User Saves Build](images/User%20Saves%20a%20New%20Build%20Diagram%20BuildForge.png)

### Use Case 2: User Logs In
![User Login](images/User%20Logs%20In%20Diagram%20BuildForge.png)

### Use Case 3: Summoner Search (High-Level)
**Sequence (text)**
1. Front-End -> FastAPI: `/players/search?riotId=GameName#TAG&region=...`
2. FastAPI -> Cache: check `riotId+region` key
3. If miss: FastAPI -> Riot API: account lookup -> summoner/profile -> matches
4. FastAPI -> Cache: store response with TTL=5 min
5. FastAPI -> Front-End: return normalized player + recent matches

---

## 5. API Specifications

## 5.1 External APIs

### Riot Data Dragon (Static CDN)
Used for champions, items, runes, and images; fetched mainly by the Front-End.

### Riot Developer API (Live)
Used for Summoner Search and Account Verification.
Caching policy: Summoner search responses cached for ~5 minutes per player.

---

## 5.2 Internal API (FastAPI Endpoints)

### Auth
| Method | Endpoint | Description | Request Body | Response |
|---|---|---|---|---|
| POST | `/auth/register` | Register user | `{ "username": "...", "email": "...", "password": "..." }` | `{ "id": 1, "username": "..." }` |
| POST | `/auth/login` | Login user | `{ "username": "...", "password": "..." }` | `{ "access_token": "...", "token_type": "bearer" }` |
| POST | `/auth/logout` | Logout | *(none)* | `{ "message": "Logged out" }` |

### Builds
| Method | Endpoint | Description | Request Body | Response |
|---|---|---|---|---|
| GET | `/builds` | List my builds | *(Auth header)* | `[ { "id": 1, "title": "..." }, ... ]` |
| POST | `/builds` | Create build | `{ "title": "...", "champion_id": "...", "items": [...], "runes": {...} }` | `{ "id": 2, "message": "Build created" }` |
| GET | `/builds/{id}` | Get build details | *(Auth recommended)* | `{ "id": 1, "items": [...], "runes": {...} }` |
| DELETE | `/builds/{id}` | Delete build | *(Auth header)* | `{ "message": "Deleted successfully" }` |

### Players & Verification
| Method | Endpoint | Description |
|---|---|---|
| GET | `/players/search` | Search by Riot ID + region |
| POST | `/riot/verification/start` | Start icon challenge |
| POST | `/riot/verification/confirm` | Confirm icon challenge |

---

## 6. Validation & Calculation Rules (MVP)

### Build Validation
- Exactly 6 item slots + 1 trinket slot.
- Boots uniqueness rule (no duplicate boots).
- Reject invalid item IDs not found in Data Dragon.

### Calculators
- **SlotDiff:** compare two item candidates with same baseline build.
- **Damage per spell:** formulas implemented for a limited champion pool initially.

---

## 7. SCM and QA Strategies

### Source Control (GitHub)
- `main`: stable.
- `dev`: integration.
- `feature/*`: feature branches with PRs.

### QA Strategy
- **Backend:** Unit tests with `pytest` for auth, CRUD, and calculation logic.
- **Frontend:**
  - **Linting:** Standard ESLint for JavaScript.
  - **Manual Testing:** Verifying DOM updates and calculations manually across browsers.
  - No framework-specific testing tools required.

---

## 8. Technical Justifications

### Why Vanilla JavaScript over React/Vue?
*   **Performance:** Removes the overhead of a Virtual DOM and heavy bundle sizes. The app loads instantly.
*   **Mastery:** Demonstrates a deep understanding of core web technologies (DOM API, Event Loop, Fetch API) without relying on abstractions.
*   **Simplicity:** For a single-page MVP, direct DOM manipulation is sufficient and avoids complex build chains.

### Why Tailwind CSS?
*   **Speed:** Allows rapid UI prototyping directly in the HTML classes without context switching to CSS files.
*   **Consistency:** Ensures a cohesive design system (spacing, colors) out of the box.

### Why FastAPI & PostgreSQL?
*   **FastAPI:** Provides high-performance async endpoints and automatic documentation (Swagger UI), perfect for a clear API contract with the Vanilla JS frontend.
*   **PostgreSQL:** Essential for structured relational data (Users <-> Builds).
