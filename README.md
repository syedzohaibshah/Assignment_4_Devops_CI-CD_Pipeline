# 📝 Notes Application - Multi-Service DevOps Demo

A simple, fully functional multi-service application demonstrating frontend, backend, and database integration.

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Frontend (React)                             │
│                      Port 3000                                    │
│        ┌──────────────────────────────────────┐                 │
│        │  React Components                     │                 │
│        │  - NoteList, NoteForm                 │                 │
│        │  - StatusBar, App                      │                 │
│        └──────────────────────┬─────────────────┘                 │
└─────────────────────────────────┼─────────────────────────────────┘
                                  │
                    HTTP REST API (CORS Enabled)
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Backend (Node.js/Express)                     │
│                      Port 5555                                    │
│        ┌──────────────────────────────────────┐                 │
│        │  API Routes                           │                 │
│        │  - GET/POST/PUT/DELETE /api/notes     │                 │
│        │  - GET /api/health                    │                 │
│        └──────────────────────┬─────────────────┘                 │
└─────────────────────────────────┼─────────────────────────────────┘
                                  │
                        PostgreSQL Driver (pg)
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Database (PostgreSQL)                           │
│                      Port 5432                                    │
│        ┌──────────────────────────────────────┐                 │
│        │  Table: notes                         │                 │
│        │  - id (Primary Key)                   │                 │
│        │  - title, content                     │                 │
│        │  - created_at, updated_at             │                 │
│        └──────────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────┘

```

## 📋 Project Structure

```
A4/
├── backend/
│   ├── package.json           # Backend dependencies
│   ├── server.js              # Express server with API routes
│   ├── init.sql               # Database schema and sample data
│   ├── .env                   # Environment variables
│   └── node_modules/          # Dependencies (after npm install)
│
├── frontend/
│   ├── package.json           # Frontend dependencies
│   ├── public/
│   │   └── index.html         # HTML template
│   ├── src/
│   │   ├── App.js             # Main App component
│   │   ├── App.css            # App styles
│   │   ├── index.js           # React entry point
│   │   ├── index.css          # Global styles
│   │   └── components/
│   │       ├── NoteForm.js    # Form component
│   │       ├── NoteForm.css
│   │       ├── NoteList.js    # Notes list component
│   │       ├── NoteList.css
│   │       ├── StatusBar.js   # Status indicator
│   │       └── StatusBar.css
│   └── node_modules/          # Dependencies (after npm install)
│
└── README.md                  # This file
```

## 🚀 Prerequisites

Make sure you have these installed on your machine:

- **Node.js** (v14 or higher) - [Download](https://nodejs.org/)
- **npm** (comes with Node.js)
- **PostgreSQL** (v12 or higher) - [Download](https://www.postgresql.org/download/)
- **git** (optional, for cloning)

## 📦 Installation & Setup

### Step 1: Install PostgreSQL & Create Database

#### macOS (using Homebrew):

```bash
brew install postgresql@15
brew services start postgresql@15
```

#### Linux (Ubuntu/Debian):

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

#### Windows:

Download and install from [postgresql.org](https://www.postgresql.org/download/windows/)

### Step 2: Create Database and Schema

Open PostgreSQL terminal:

```bash
psql postgres
```

Or if PostgreSQL is running as a service:

```bash
psql -U postgres
```

Then run these commands:

```sql
-- Create database
CREATE DATABASE notes_db;

-- Connect to database
\c notes_db

-- Create table
CREATE TABLE notes (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create index
CREATE INDEX idx_notes_created_at ON notes(created_at DESC);

-- Insert sample data
INSERT INTO notes (title, content) VALUES
  ('Welcome to Notes App', 'This is a simple multi-service application demonstrating React frontend, Node.js backend, and PostgreSQL database.'),
  ('Getting Started', 'You can create, read, update, and delete notes using this application.'),
  ('Architecture Overview', 'Frontend: React.js running on port 3000\nBackend: Node.js Express API on port 5000\nDatabase: PostgreSQL on port 5432');

-- Exit psql
\q
```

Or use the provided SQL script:

```bash
psql -U postgres -d notes_db -f backend/init.sql
```

### Step 3: Setup Backend

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Verify .env file has correct database credentials
cat .env

# Start the backend server
npm start
# or for development with auto-reload:
npm run dev
```

You should see:

```
✓ Backend server running on http://localhost:5000
✓ Database connection pool created
```

### Step 4: Setup Frontend (in a new terminal)

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start the React development server
npm start
```

This will automatically open your browser at `http://localhost:3000`

## ✅ Verification Checklist

Before using the application, verify all services are running:

- [ ] PostgreSQL is running (`psql -U zohaib` should connect)
- [ ] Backend is running (`curl http://localhost:5555/api/health` should return JSON)
- [ ] Frontend is running on `http://localhost:3000` (browser opens automatically)
- [ ] You can see "✓ Backend Connected" in the status bar
- [ ] Sample notes are displayed

## 🎯 Usage Guide

### Creating a Note

1. Fill in the "Note Title" field in the left panel
2. Fill in the "Note Content" field
3. Click "Create Note"
4. The note appears in the grid on the right

### Viewing Notes

- Notes are displayed as cards in the main grid
- Click on any note to select it (it will be highlighted)
- The note preview shows the first 100 characters

### Editing a Note

1. Click on a note card to select it
2. The form on the left will populate with the note's data
3. Make your changes
4. Click "Update Note"
5. Click "Cancel" to deselect without saving

### Deleting a Note

- Click the trash icon (🗑️) on any note card
- Confirm the deletion when prompted

## 🔌 API Endpoints Reference

All endpoints are prefixed with `/api`

### Health Check

```
GET /api/health
Response: { "status": "Backend is running!", "timestamp": "2026-05-08T..." }
```

### Get All Notes

```
GET /api/notes
Response: [
  {
    "id": 1,
    "title": "Note Title",
    "content": "Note content...",
    "created_at": "2026-05-08T10:00:00.000Z",
    "updated_at": "2026-05-08T10:00:00.000Z"
  }
]
```

### Get Single Note

```
GET /api/notes/:id
Response: { "id": 1, "title": "...", "content": "...", ... }
```

### Create Note

```
POST /api/notes
Request: { "title": "New Note", "content": "Content here" }
Response: { "id": 2, "title": "New Note", ... }
```

### Update Note

```
PUT /api/notes/:id
Request: { "title": "Updated Title", "content": "Updated content" }
Response: { "id": 1, "title": "Updated Title", ... }
```

### Delete Note

```
DELETE /api/notes/:id
Response: { "message": "Note deleted successfully", "note": {...} }
```

## 🛠️ Troubleshooting

### Problem: "Cannot connect to database"

**Solution:**

- Check PostgreSQL is running: `sudo systemctl status postgresql` (Linux) or `brew services list` (macOS)
- Verify `.env` file has correct credentials
- Try connecting manually: `psql -U postgres -d notes_db`

### Problem: "Backend not connected" in the app

**Solution:**

- Ensure backend is running: `npm start` in the backend directory
- Check backend terminal for error messages
- Verify port 5000 is not in use: `lsof -i :5000` (macOS/Linux)

### Problem: "Module not found" or npm errors

**Solution:**

```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Problem: Port already in use

- Backend (5000): `lsof -i :5000` and kill the process
- Frontend (3000): `lsof -i :3000` and kill the process
- PostgreSQL (5432): Check if another instance is running

### Problem: CORS errors in browser console

**Solution:**

- Ensure backend is running on http://localhost:5000
- Check that the frontend's `package.json` has `"proxy": "http://localhost:5000"`

## 📝 Database Schema

### notes table

| Column     | Type         | Constraints               |
| ---------- | ------------ | ------------------------- |
| id         | SERIAL       | PRIMARY KEY               |
| title      | VARCHAR(255) | NOT NULL                  |
| content    | TEXT         | NOT NULL                  |
| created_at | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP |

**Index:** `idx_notes_created_at` on `created_at DESC` for performance

## 🔄 Development Workflow

### Backend Development

```bash
cd backend
npm install nodemon --save-dev  # For auto-reload
npm run dev
```

### Frontend Development

```bash
cd frontend
npm start
# Hot reload is enabled by default with react-scripts
```

### Testing API Endpoints

Using curl:

```bash
# Get all notes
curl http://localhost:5000/api/notes

# Create a note
curl -X POST http://localhost:5000/api/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","content":"Test content"}'
```

Using Postman or Insomnia:

1. Import the API endpoints listed above
2. Set base URL to `http://localhost:5000`
3. Test each endpoint

## 🚀 Deployment on Localhost

The current setup is already deployed on localhost:

- **Frontend:** http://localhost:3000
- **Backend:** http://localhost:5000
- **Database:** localhost:5432

To make the frontend and backend available to other machines on your network:

### Expose Frontend

Change in `frontend/package.json`:

```json
"proxy": "http://<your-machine-ip>:5000"
```

Start frontend with:

```bash
npm start -- --host 0.0.0.0
```

### Expose Backend

Change in `backend/.env`:

```
DB_HOST=localhost
PORT=5000
```

Backend is already listening on all interfaces by default.

Other machines can access:

- Frontend: `http://<your-machine-ip>:3000`
- Backend API: `http://<your-machine-ip>:5000`

## 📊 Technologies Used

| Component              | Technology   | Version |
| ---------------------- | ------------ | ------- |
| **Frontend Framework** | React        | 18.2.0  |
| **Backend Framework**  | Express.js   | 4.18.2  |
| **Backend Runtime**    | Node.js      | 14+     |
| **Database**           | PostgreSQL   | 12+     |
| **HTTP Client**        | Axios        | 1.5.0   |
| **CORS**               | cors package | 2.8.5   |

## 📚 Learning Resources

- [React Documentation](https://react.dev)
- [Express.js Documentation](https://expressjs.com)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Node.js Documentation](https://nodejs.org/docs/)

## 🎓 Assignment Requirements Coverage

✅ **Frontend Application:** React with interactive components
✅ **Backend Service:** Node.js with Express REST API
✅ **Database Component:** PostgreSQL with proper schema
✅ **Service Interaction:** Full CRUD operations across all services
✅ **Localhost Deployment:** All services running on localhost
✅ **Demo Ready:** Fully functional with sample data

## 📄 License

This project is created for educational purposes.

## 👤 Author

Created for DevOps Assignment A4

---

**Last Updated:** May 8, 2026
**Status:** Ready for Production Demo
