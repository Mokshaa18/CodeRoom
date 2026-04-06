# CodeRoom

**Real-time collaborative coding platform for developers and students.**

Share a link. Start coding together. No signup. No installation.

---

## Live Demo
> Will be updated after deployment to Railway + Vercel

---

## What It Does

- Real-time collaborative code editor (Monaco Editor — same editor as VS Code)
- Upload local code files into a shared session and download after editing
- Import files directly from GitHub repositories and push edits back as commits
- Mock interview mode with dual roles, synchronized timer, problem panel, and structured feedback
- Execute code with Judge0 API (Java, Python, C++, JavaScript, and 40+ languages)
- Conflict resolution UI when offline edits clash — users choose which version to keep
- Session playback to review how a problem was approached

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Java 17, Spring Boot 3.2 |
| Real-Time | Spring WebSocket, STOMP protocol, SockJS |
| Cache | Redis (active room content, sessions) |
| Database | MySQL (metadata, feedback, history, GitHub tokens) |
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Code Editor | Monaco Editor (same as VS Code, loaded via CDN) |
| Code Execution | Judge0 API via RapidAPI |
| GitHub Integration | GitHub REST API v3, OAuth 2.0 |
| Build Tool | Maven |
| Deployment | Railway (backend + Redis + MySQL), Vercel (frontend) |

---

## Project Structure

```
CodeRoom/
│
├── coderoom-backend/                          Spring Boot Application
│   ├── pom.xml                                Maven dependencies
│   └── src/
│       ├── main/
│       │   ├── java/com/coderoom/
│       │   │   ├── CoderoomApplication.java   Entry point
│       │   │   ├── config/
│       │   │   │   ├── WebSocketConfig.java   STOMP WebSocket setup
│       │   │   │   ├── RedisConfig.java       Redis template config
│       │   │   │   ├── SecurityConfig.java    Spring Security + JWT
│       │   │   │   └── CorsConfig.java        Cross-origin config
│       │   │   ├── controller/
│       │   │   │   ├── HealthController.java  GET /api/health
│       │   │   │   ├── RoomController.java    Room CRUD APIs
│       │   │   │   ├── WebSocketController.java Real-time handlers
│       │   │   │   ├── FileController.java    Upload and download
│       │   │   │   ├── FeedbackController.java Feedback APIs
│       │   │   │   ├── GitHubController.java  GitHub OAuth + API
│       │   │   │   ├── AuthController.java    Register and login
│       │   │   │   └── ExecutionController.java Judge0 integration
│       │   │   ├── model/
│       │   │   │   ├── Room.java              rooms table
│       │   │   │   ├── User.java              users table
│       │   │   │   ├── Feedback.java          feedback table
│       │   │   │   ├── SessionEvent.java      session_events table
│       │   │   │   └── GitHubToken.java       github_tokens table
│       │   │   ├── repository/
│       │   │   │   ├── RoomRepository.java
│       │   │   │   ├── UserRepository.java
│       │   │   │   ├── FeedbackRepository.java
│       │   │   │   ├── SessionEventRepository.java
│       │   │   │   └── GitHubTokenRepository.java
│       │   │   ├── service/
│       │   │   │   ├── RoomService.java       Room business logic
│       │   │   │   ├── FileService.java       File upload/download
│       │   │   │   ├── FeedbackService.java   Feedback logic
│       │   │   │   ├── GitHubService.java     GitHub API calls
│       │   │   │   ├── ExecutionService.java  Judge0 API calls
│       │   │   │   ├── ConflictService.java   Conflict detection
│       │   │   │   └── AuthService.java       Auth logic
│       │   │   ├── dto/
│       │   │   │   ├── CreateRoomRequest.java
│       │   │   │   ├── RoomResponse.java
│       │   │   │   ├── CodeMessage.java       WebSocket payload
│       │   │   │   ├── CursorMessage.java     Cursor position
│       │   │   │   ├── TimerMessage.java      Timer sync
│       │   │   │   ├── FeedbackRequest.java
│       │   │   │   ├── FeedbackResponse.java
│       │   │   │   ├── FileUploadResponse.java
│       │   │   │   ├── ConflictMessage.java
│       │   │   │   ├── ExecutionRequest.java
│       │   │   │   ├── ExecutionResponse.java
│       │   │   │   ├── AuthRequest.java
│       │   │   │   ├── AuthResponse.java
│       │   │   │   └── ErrorResponse.java
│       │   │   ├── security/
│       │   │   │   ├── JwtUtil.java           Token generation/validation
│       │   │   │   └── JwtFilter.java         Request interceptor
│       │   │   └── exception/
│       │   │       ├── GlobalExceptionHandler.java
│       │   │       ├── RoomNotFoundException.java
│       │   │       ├── FileUploadException.java
│       │   │       └── GitHubIntegrationException.java
│       │   └── resources/
│       │       └── application.properties     All configuration
│       └── test/
│           └── java/com/coderoom/             Unit tests go here
│
├── coderoom-frontend/                         Plain HTML/CSS/JS
│   ├── pages/
│   │   ├── index.html                         Home — create/join room
│   │   ├── room.html                          Editor — main collaboration page
│   │   ├── feedback.html                      Post-interview feedback form
│   │   └── playback.html                      Session replay page
│   ├── js/
│   │   ├── api.js                             All fetch() calls to backend
│   │   ├── room.js                            WebSocket + Monaco Editor logic
│   │   ├── filehandler.js                     Upload and download logic
│   │   ├── github.js                          GitHub OAuth + import/push UI
│   │   ├── conflict.js                        Conflict resolution modal
│   │   ├── timer.js                           Countdown timer logic
│   │   └── playback.js                        Session replay logic
│   ├── css/
│   │   ├── main.css                           Global styles and dark theme
│   │   ├── room.css                           Room page specific styles
│   │   └── conflict.css                       Conflict modal styles
│   └── assets/                                Icons and images
│
├── .gitignore
└── README.md
```

---

## REST API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/health | Health check |
| POST | /api/rooms | Create new room |
| GET | /api/rooms/{roomId} | Get room details |
| GET | /api/rooms/{roomId}/content | Get current code from Redis |
| POST | /api/rooms/{roomId}/files | Upload file to room |
| GET | /api/rooms/{roomId}/files/{filename} | Download file from room |
| GET | /api/rooms/{roomId}/files | List all files in room |
| POST | /api/rooms/{roomId}/feedback | Submit interview feedback |
| GET | /api/rooms/{roomId}/feedback | Get feedback for room |
| GET | /api/rooms/{roomId}/playback | Get session events for playback |
| POST | /api/execution/run | Execute code via Judge0 |
| GET | /api/github/authorize | Get GitHub OAuth URL |
| GET | /api/github/callback | Handle GitHub OAuth callback |
| GET | /api/github/repos | List user GitHub repositories |
| POST | /api/github/import | Import file from GitHub |
| POST | /api/github/push | Push edited file to GitHub |
| POST | /api/auth/register | Register new user |
| POST | /api/auth/login | Login and receive JWT token |

---

## WebSocket Topics

| Topic | Direction | Description |
|---|---|---|
| /topic/room/{id}/code | Server → All clients | Code change broadcast |
| /topic/room/{id}/cursor | Server → All clients | Cursor position updates |
| /topic/room/{id}/users | Server → All clients | User join and leave events |
| /topic/room/{id}/files | Server → All clients | File list updates |
| /topic/room/{id}/output | Server → All clients | Code execution output |
| /topic/room/{id}/conflict | Server → All clients | Conflict detected events |
| /topic/room/{id}/timer | Server → All clients | Timer start and sync |

---

## How to Run Locally

### Prerequisites
- Java 17 or higher
- Maven 3.8 or higher
- MySQL 8 running locally
- Redis 7 running locally

### Step 1 — Create MySQL database
```sql
mysql -u root -p
CREATE DATABASE coderoom;
exit;
```

### Step 2 — Configure database password
Open `coderoom-backend/src/main/resources/application.properties`
Change `yourpassword` to your actual MySQL root password.

### Step 3 — Start Redis
```bash
redis-server
```

### Step 4 — Start Spring Boot
```bash
cd coderoom-backend
mvn spring-boot:run
```
Server starts at http://localhost:8080

### Step 5 — Open the frontend
Open `coderoom-frontend/pages/index.html` in your browser.
Or serve it with: `npx serve coderoom-frontend/pages`

---

## Environment Variables for Railway Deployment

| Variable | What It Is |
|---|---|
| DB_URL | Full MySQL JDBC connection string from Railway |
| DB_USERNAME | MySQL username |
| DB_PASSWORD | MySQL password |
| REDIS_HOST | Redis hostname from Railway |
| REDIS_PORT | Redis port |
| REDIS_PASSWORD | Redis password |
| JWT_SECRET | Random 256-bit secret for signing tokens |
| GITHUB_CLIENT_ID | From your GitHub OAuth App settings |
| GITHUB_CLIENT_SECRET | From your GitHub OAuth App settings |
| GITHUB_REDIRECT_URI | Your Railway backend URL + /api/github/callback |
| JUDGE0_API_KEY | From RapidAPI after subscribing to Judge0 |

---

## Build Order (How This Was Built)

1. Phase 0 — Tooling setup and first commit
2. Phase 1 — Spring Boot backend (health API → MySQL → Redis → WebSocket → error handling)
3. Phase 2 — Frontend (JavaScript basics → home page → Monaco Editor → real-time sync)
4. Phase 3 — File operations (upload and download)
5. Phase 4 — Mock interview mode (timer and problem panel)
6. Phase 5 — GitHub integration (OAuth → import → push)
7. Phase 6 — Code execution (Judge0 Run button)
8. Phase 7 — Security layer (JWT authentication)
9. Phase 8 — Offline resilience (localStorage buffer)
10. Phase 9 — Conflict resolution UI
11. Phase 10 — Deployment (Railway + Vercel)

---

*Built by Mokshagna | Java · Spring Boot · WebSockets · Redis · MySQL*
