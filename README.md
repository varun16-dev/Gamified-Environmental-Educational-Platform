# NEXORA - Interactive Environmental Learning & Gamified EdTech Platform

NEXORA is a full-stack educational web platform designed to make environmental science, sustainability, and green habits engaging for young learners. Through gamified learning modules, interactive video lessons, quizzes, badges, and competitive leaderboards, NEXORA transforms ecological education into an immersive experience.

---

## System Architecture

NEXORA follows a decoupled client-server architecture built on the MERN stack (MongoDB, Express, React, Node.js):

```
+-------------------------------------------------------------+
|                     Client (Frontend)                       |
|  React 18 SPA + React Router DOM v6 + Styled Components/CSS |
+-------------------------------------------------------------+
                              |
                              | HTTP REST Requests / JSON
                              | Authorization: Bearer <JWT>
                              v
+-------------------------------------------------------------+
|                     Server (Backend)                        |
|        Express v5.1 / Node.js (REST API Layer)              |
|   +-----------------------------------------------------+   |
|   | Middleware: CORS, Auth (JWT), Multer File Uploads   |   |
|   +-----------------------------------------------------+   |
|   | Routes & Controllers:                               |   |
|   |   - /api/auth          (Sign up, Login, JWT issue)  |   |
|   |   - /api/profile       (Stats, Badges, Certs)       |   |
|   |   - /api/leaderboard   (Scores & EcoPoints ranking) |   |
|   |   - /api/games         (Session tracking & scores)  |   |
|   |   - /api/activity      (Engagement & quiz metrics)  |   |
|   +-----------------------------------------------------+   |
+-------------------------------------------------------------+
                              |
                              | Mongoose v8 ODM
                              v
+-------------------------------------------------------------+
|                     Database Tier                           |
|                      MongoDB                                |
|   Collections: Users, GameSessions, Badges, Quizzes,        |
|                Certificates, Activities                     |
+-------------------------------------------------------------+
```

### Architectural Highlights
- **Stateless Authentication**: Token-based authentication using JSON Web Tokens (JWT) stored client-side for secure route access.
- **Modular Backend Organization**: Clear separation of concerns with isolated models, controllers, routes, and middleware.
- **Responsive Presentation Layer**: Single Page Application (SPA) architecture with stateful dashboard widgets, expandable sidebar navigation, and modular card views.

---

## Workflow

The core user journey flows through onboarding, discovery, gamified learning, and progress tracking:

```
[User Registration / Login]
           │
           ▼
[JWT Token Generated & Stored in LocalStorage]
           │
           ▼
[Personalized Dashboard Entry]
    ├── View Live Leaderboard & EcoPoints
    ├── Track Active Milestones (Games, Badges, Quizzes)
    └── Select Educational Activity
           │
     ┌─────┴──────────────────────┐
     ▼                            ▼
[Play Eco Games]        [Watch Video Lessons / Quizzes]
     │                            │
     └─────────────┬──────────────┘
                   │
                   ▼
[Post Activity / Score Update via REST API]
                   │
                   ▼
[DB Update: Points, Badges, Certificates, & Ranking]
                   │
                   ▼
[Real-Time Dashboard Refresh]
```

1. **Authentication Flow**: Users register or log in with hashed passwords (bcrypt). The backend validates credentials and issues a signed JWT token.
2. **Dashboard Initialization**: Upon login, the client initiates concurrent API requests to fetch user-specific analytics (badges, completed quizzes, game count) and global leaderboard ranks.
3. **Interactive Learning & Play**: The student interacts with curated video lessons or launches educational games.
4. **Scoring & EcoPoints Calculation**: As tasks and games are completed, events post updates to `/api/games` and `/api/activity`.
5. **Reward & Milestone System**: The server calculates point increases, evaluates criteria for new badges or completion certificates, and persists progress to MongoDB.

---

## Tech Stack & Dependencies

### Frontend
- **Library**: React `18.2.0`
- **Routing**: React Router DOM `6.11.2`
- **Styling**: Vanilla CSS3 & Styled Components `6.1.19`
- **Iconography**: React Icons `5.5.0`
- **Tooling & Build**: React Scripts `5.0.1`

### Backend
- **Runtime**: Node.js
- **Web Framework**: Express `5.1.0`
- **Database ODM**: Mongoose `8.18.2`
- **Authentication**: JSON Web Token (`jsonwebtoken`) `9.0.2` & `bcryptjs` `3.0.2`
- **Middleware**: CORS `2.8.5`, Multer `2.0.2`, Dotenv `17.2.2`
- **Dev Utility**: Nodemon `3.1.10`

### Database
- **Engine**: MongoDB (Local instance / Atlas cluster)

---

## Project Structure

```
├── NEXORA/
│   └── backend/
│       ├── config/          # Database connection setup
│       ├── controllers/     # Request handlers & business logic
│       ├── middleware/      # JWT authentication and file upload filters
│       ├── models/          # Mongoose schemas (User, GameSession, Badge, etc.)
│       ├── routes/          # REST route declarations
│       ├── server.js        # Server entry point & route registration
│       └── package.json     # Backend dependencies and scripts
├── public/                  # Static assets and index.html
├── src/
│   ├── components/          # Dashboard, Sidebar, Topbar, Games, Videos, Auth
│   ├── App.jsx              # Main route switch & application root
│   ├── data.js              # Fallback fixtures & configuration data
│   ├── index.css            # Base stylesheet & theme definitions
│   └── index.js             # Client mounting point
├── package.json             # Frontend dependencies & configurations
└── README.md                # Project documentation
```

---

## Getting Started

### Prerequisites
- Node.js (v18.x or later recommended)
- MongoDB running locally on `mongodb://127.0.0.1:27017` or a MongoDB Atlas URI

### 1. Backend Setup
```bash
# Navigate to the backend directory
cd NEXORA/backend

# Install dependencies
npm install

# Configure environment variables in .env:
# PORT=5000
# MONGO_URI=mongodb://127.0.0.1:27017/environex
# JWT_SECRET=your_jwt_secret_key

# Start the server (development mode with hot-reload)
npm run dev
```

### 2. Frontend Setup
```bash
# Navigate to project root
cd ../../

# Install frontend dependencies
npm install

# Start development server
npm start
```
The client will launch at `http://localhost:3000` and communicate with the backend on `http://localhost:5000`.
