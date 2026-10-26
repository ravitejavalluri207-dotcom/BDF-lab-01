# Backend Development Fundamentals – Lab 1: Node.js and Express Server

## Student Details
- **Name:** Valluri Ravi Teja
- **Registration Number:** 252U1R9166
- **Section:** CSE GEN-AI 2C
- **GitHub Repository:** [https://github.com/ravitejavalluri207-dotcom/BDF-lab-01](https://github.com/ravitejavalluri207-dotcom/BDF-lab-01)

---

## Objective
The objective of this lab is to:
1. Build a basic web server using Node.js's built-in `http` module.
2. Develop a REST API server using the **Express.js** framework.
3. Understand routing and HTTP status codes (`200 OK`, `404 Not Found`).
4. Implement dynamic configuration using environment variables (`process.env.PORT || 3000`) for deployment compatibility.
5. Manage dependencies and establish proper Git hygiene by excluding `node_modules/` and `.env` using `.gitignore`.

---

## Project Structure
```text
lab-01/
├── node_modules/          # Local dependencies (ignored by git)
├── screenshots/           # Output verification screenshots
│   └── .gitkeep
├── .gitignore             # Git ignore file (excludes node_modules, .env)
├── package.json           # Project manifest and scripts
├── package-lock.json      # Dependency lock file
├── README.md              # Project documentation and lab report
├── server-http.js         # Basic HTTP server using built-in http module
└── server.js              # Express.js REST API server
```

---

## Prerequisites
- **Node.js** (v18.x or later installed)
- **npm** (Node Package Manager)
- **Git**

---

## Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ravitejavalluri207-dotcom/BDF-lab-01.git
   cd BDF-lab-01
   ```
   *(Or navigate into `lab-01` if cloned as part of a multi-directory setup)*

2. **Install dependencies:**
   ```bash
   npm install
   ```

---

## Execution Instructions

### 1. Running the Built-in HTTP Server (`server-http.js`)
To run the server created with Node's native `http` module:
```bash
node server-http.js
# Or via npm script:
npm run start:http
```
- Open `http://localhost:3000` in your browser.
- Expected output: Plain text `"Hello"`.

### 2. Running the Express.js Server (`server.js`)
To run the Express application:
```bash
node server.js
# Or via npm script:
npm start
```
- Server will start at `http://localhost:3000` (or the port specified in `process.env.PORT`).

---

## API Endpoints & Expected Status Codes

| Endpoint | Method | Description | Status Code | Expected Response |
| :--- | :--- | :--- | :--- | :--- |
| `http://localhost:3000` (`server-http.js`) | GET | Built-in HTTP module greeting | `200 OK` | `Hello` |
| `/` (`server.js`) | GET | Express welcome message | `200 OK` | `Welcome to my Node.js and Express server!` |
| `/about` (`server.js`) | GET | Student information (Name, Reg No, Section) in JSON | `200 OK` | `{"name":"Valluri Ravi Teja","registrationNumber":"252U1R9166","section":"CSE GEN-AI 2C"}` |
| `/time` (`server.js`) | GET | Current server timestamp generated via `new Date()` | `200 OK` | `{"currentDateTime":"2026-10-26T08:53:59.459Z"}` |
| Any undefined route (e.g. `/random`) | GET / Any | Catch-all handler for unknown routes | `404 Not Found` | `404 - Page not found` |

---

## Screenshots

> [!NOTE]
> Upload your execution screenshots to the `./screenshots/` directory and replace the file paths below with your actual screenshot filenames.

<!-- Replace the filenames below with your actual screenshot images -->
### 1. Built-in HTTP Server Output (Port 3000)
![Screenshot](./screenshots/Screenshot%202026-09-16%20094720.png)

### 2. Express Server - GET /
![Screenshot](./screenshots/Screenshot%202026-09-16%20094721.png)

### 3. Express Server - GET /about
![Screenshot](./screenshots/Screenshot%202026-09-16%20094722.png)

### 4. Express Server - GET /time
![Screenshot](./screenshots/Screenshot%202026-09-16%20094723.png)

### 5. Express Server - 404 Route Not Found
![Screenshot](./screenshots/Screenshot%202026-09-16%20094724.png)

---

## Viva Questions & Answers

### Q1: What is the primary difference between Node's built-in `http` module and Express.js?
**Answer:** The `http` module is a low-level native module provided by Node.js that requires manual handling of routing, query parameters, content types, and request parsing. Express.js is a minimalist web framework built on top of the `http` module that provides powerful abstractions such as middleware pipelines, robust routing, template engines, and convenient helper methods (`res.json()`, `res.status()`).

### Q2: What is middleware in Express.js, and what is the role of `next()`?
**Answer:** Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application's request-response cycle (`next`). Calling `next()` passes control to the next middleware in the stack; omitting it without ending the response leaves the client hanging.

### Q3: Why is `process.env.PORT || 3000` used instead of hardcoding port 3000?
**Answer:** Cloud hosting platforms (such as Render, Railway, AWS, or Heroku) assign an arbitrary port dynamically via environment variables (`PORT`). Using `process.env.PORT || 3000` allows the application to dynamically bind to the host's port in production while defaulting to `3000` during local development.

### Q4: Why should `node_modules/` and `.env` be added to `.gitignore`?
**Answer:**
1. `node_modules/` contains thousands of third-party files that are platform-dependent and easily re-installed via `npm install` using `package.json` and `package-lock.json`. Tracking it causes repository bloat and slow Git operations.
2. `.env` contains sensitive secrets, API keys, and environment-specific settings that must never be exposed publicly.

### Q5: How does Express handle unknown routes (404 errors)?
**Answer:** Express executes route handlers and middleware in the order they are registered. By placing a fallback middleware (`app.use((req, res) => ... )`) at the very end after all valid route definitions, any request that did not match an earlier route will fall through to this handler, allowing us to send a `404 Not Found` response.

---

## Problems Faced & Solutions

1. **Problem:** `node_modules/` was previously committed into the Git repository, bloating repo size and cluttering commits.
   - **Solution:** Created a `.gitignore` file specifying `node_modules/` and `.env`, and untracked the folder from Git cache using `git rm -r --cached node_modules` while keeping local dependencies intact.

2. **Problem:** Port was hardcoded to `3000`, which could cause port collision or failure when deploying to cloud hosting services.
   - **Solution:** Updated `server.js` and `server-http.js` to read `process.env.PORT || 3000`.

3. **Problem:** Undefined routes would return Express default HTML 404 responses instead of a concise, controlled error message.
   - **Solution:** Added a terminal catch-all middleware (`app.use`) returning status `404` with `"404 - Page not found"`.
