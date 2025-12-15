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

#### **MVP Scope - Core Features (Priority 0)**

*   **Application Type:** Web Application (website).
*   **Core Features (What the MVP MUST have):**
    
    1.  **Authentication System**
        *   Account creation, user login, and logout
        *   JWT-based session management
        *   User profile page
        *   **Data Source:** Application Database
        *   **Riot API Calls:** None
    
    2.  **Build Constructor (Theorycraft)**
        *   Interface to select a champion from the complete roster
        *   Selection of 6 items with stats preview
        *   Selection of primary and secondary runes
        *   Champion level selection (1-18)
        *   Automatic calculation of total stats (AD, AP, Armor, MR, HP, etc.)
        *   **Data Source:** Data Dragon (Riot's CDN - no rate limits)
        *   **Riot API Calls:** None
    
    3.  **Build Saving System**
        *   Save created builds to user account
        *   "My Builds" page showing all saved builds
        *   Edit/Delete existing builds
        *   Public/Private build visibility option
        *   **Data Source:** Application Database
        *   **Riot API Calls:** None
    
    4.  **Damage Calculator per Spell**
        *   Display all champion abilities (Q, W, E, R)
        *   Calculate damage for each spell based on current build
        *   Variable inputs: Spell level, Target Armor/MR
        *   Damage comparison table against different resistance levels
        *   **Data Source:** Data Dragon + Custom damage formulas
        *   **Riot API Calls:** None
        *   **Note:** Start with 5-10 popular champions, expand gradually
    
    5.  **"SlotDiff" Comparison Tool**
        *   Compare two different items in the same build slot
        *   Display stat differences between items
        *   Calculate and compare damage output against various Armor/MR values
        *   Visual representation of which item performs better in different scenarios
        *   **Data Source:** Data Dragon + Custom calculations
        *   **Riot API Calls:** None

#### **Secondary Features (Priority 1 - Time Permitting)**

6.  **Player Search & Match History**
    *   Search bar for players using Riot ID (GameName#TagLine)
    *   Display last 10 matches of searched player
    *   For each match: Champion played, KDA, Win/Loss, Final items
    *   **Data Source:** Riot API
    *   **API Calls:** ~12 calls per search (within Development Key limits)
    *   **Optimization:** 5-minute cache per player search
    *   **Rate Limit Impact:** Low (manageable with Development Key)

7.  **Riot Account Verification via Profile Icon**
    *   User links their Riot account to the application
    *   Verification process: User changes in-game profile icon to randomly generated one
    *   Application verifies icon change via API
    *   Upon verification, user's builds are linked to their Riot account
    *   **Data Source:** Riot API
    *   **API Calls:** 2 calls per verification attempt
    *   **Rate Limit Impact:** Minimal

#### **Out-of-Scope Features (Post-MVP)**

*   **Meta Tierlist:** Requires millions of API calls to aggregate reliable statistics. Alternative: Use Kaggle datasets or manual curation, or implement post-MVP with Production API Key.
*   **Automatic rune import** into game client.
*   **Advanced social features** (comments, voting on builds, build sharing).
*   **AI coaching** or video replay analysis.
*   **Support for other games** (TFT, Valorant).

#### **Data Sources Strategy**

**Primary Data Sources:**
*   **Data Dragon (Riot CDN - No API Key Required):**
    *   Champions data (names, stats, abilities, images)
    *   Items data (stats, prices, icons)
    *   Runes data (effects, images)
    *   Summoner spells data
    *   **No rate limits, publicly accessible JSON files**

*   **Application Database:**
    *   User accounts and authentication
    *   Saved builds
    *   Account verification status

*   **Riot API (With Development Key Limits):**
    *   Player search and match history (Priority 1 feature)
    *   Account verification (Priority 1 feature)
    *   **Development Key Limits:** 20 requests/second, 100 requests/2 minutes

#### **Risks and Mitigation Strategies**

*   **Risk 1: Complexity of damage calculation formulas**
    *   **Mitigation:** Start by implementing formulas for 5-10 popular champions (e.g., Jinx, Ezreal, Ahri, Zed, Darius) to validate the calculation logic. Expand champion pool incrementally after core functionality is proven.

*   **Risk 2: Riot API rate limits for Player Search feature**
    *   **Mitigation:** Implement caching system (5-minute cache per player). Add rate limiting on frontend to prevent abuse. Monitor API usage and display clear error messages if limits are reached.

*   **Risk 3: Workload and scope creep**
    *   **Mitigation:** Strict prioritization using P0 (Core) and P1 (Secondary) features. If time is limited, Player Search and Account Verification can be postponed to post-MVP without affecting core functionality demonstration.

*   **Risk 4: Damage formula accuracy**
    *   **Mitigation:** Focus on physical and magic damage basics first. Add complex mechanics (true damage, penetration interactions, conditional effects) incrementally. Document any known limitations.

#### **Technical Architecture Overview**

**Frontend:**
*   Modern JavaScript framework (React/Vue recommended)
*   Responsive design for desktop and mobile
*   State management for build construction
*   API client for backend communication

**Backend:**
*   Node.js with Express (or similar)
*   RESTful API architecture
*   JWT authentication
*   Caching layer for Riot API responses
*   Rate limiting middleware

**Database:**
*   PostgreSQL or MongoDB
*   Tables: Users, Builds, VerificationPending (for account linking)

**External APIs:**
*   Data Dragon (CDN) for static game data
*   Riot API for player search and verification (limited use)

#### **Deployment Strategy**

**For Local Development:**
*   Application fully functional with Development API Key
*   All core features (P0) work without API key limitations

**For Production Deployment (Post-MVP):**
*   Frontend: Vercel or Netlify
*   Backend: Render or Railway
*   Database: Managed PostgreSQL (Render/Railway)
*   Caching: Redis (optional for optimization)
*   **API Key Requirements:**
    *   Core features (Build Constructor, Calculator, SlotDiff) work without Production API Key
    *   Player Search feature requires Production API Key for public deployment
    *   Account Verification requires Production API Key for public deployment
*   **Note:** Application can be deployed with Player Search disabled if Production Key is not obtained, without affecting core demonstration value.

### 4. Development Timeline

*   **Phase 1: Backend Foundation (Week 1-2)**
    *   Project setup (Node.js + Express + Database)
    *   Authentication system (register, login, JWT)
    *   Database models (User, Build)

*   **Phase 2: Core Features (Week 3-5)**
    *   Data Dragon integration (champions, items, runes)
    *   Build Constructor (frontend + backend)
    *   Build saving and retrieval system

*   **Phase 3: Calculation Engine (Week 6-8)**
    *   Damage calculator implementation (5-10 champions)
    *   SlotDiff comparison tool
    *   Testing and formula validation

*   **Phase 4: Secondary Features (Week 9-10)**
    *   Player search + match history (if time permits)
    *   Riot account verification system (if time permits)

*   **Phase 5: Polish & Deployment (Week 11)**
    *   End-to-end testing
    *   Bug fixes and optimizations
    *   Documentation finalization
    *   Presentation preparation

### 5. Success Criteria for RNCP Level 5

This project demonstrates:
*   ✅ **Full-stack development:** Frontend, Backend, and Database integration
*   ✅ **External API integration:** Riot API and Data Dragon
*   ✅ **Secure authentication:** JWT-based system with password hashing
*   ✅ **Complex business logic:** Damage calculation formulas and stat aggregation
*   ✅ **User experience design:** Creative account verification method
*   ✅ **Constraint management:** Working within API rate limits and technical limitations
*   ✅ **Scalable architecture:** Designed for future feature additions
*   ✅ **Project management:** Clear prioritization and scope management

The core features (P0) are sufficient for RNCP Level 5 validation and can be fully demonstrated without Production API Key limitations.
