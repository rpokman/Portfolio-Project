# BuildForge - Developer Setup

Minimal setup guide to run BuildForge locally.

## 1. Backend (FastAPI)

**Prerequisites:** Python 3.10+ and PostgreSQL installed.

### Installation
Navigate to the backend directory:
```
cd backend
```

Create and activate a virtual environment:
```
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

Install dependencies:
```
pip install -r requirements.txt
```

### Configuration (.env)
Create a `.env` file inside the `backend/` folder with your local credentials:
```
DATABASE_URL=postgresql://postgres:password@localhost/buildforge
RIOT_API_KEY=RGAPI-YOUR-KEY-HERE
SECRET_KEY=dev_secret_key_123
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

### Run Server
Start the API server with hot-reload:
```
uvicorn main:app --reload
```
> **API Docs:** http://localhost:8000/docs

---

## 2. Frontend (Vanilla JS + Tailwind)

### Setup
No npm install required (using direct script imports & Tailwind CDN/CLI for MVP).

### Run
To avoid CORS issues with local file access, serve the frontend folder.
Open a new terminal in the `frontend/` directory:

```
cd frontend
python -m http.server 3000
```

> **Application:** http://localhost:3000
```
