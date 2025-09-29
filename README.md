# 🎉 Event Platform Backend + Frontend Static Serving

A Flask-based backend that registers multiple feature blueprints and serves a static frontend (HTML/CSS/JS) directly from the repository. Designed for quick local development and simple deployments.

- 🚀 Fast start
- 🔐 Auth-ready
- 👥 Organizer & Participant flows
- 🤝 Collaboration endpoints
- 🖥️ Virtual events
- 🗂️ Static frontend served by Flask

---

## 🧭 Overview

This application exposes API blueprints and serves the frontend pages from a sibling `frontend/` directory. Key routes like `/`, `/signin`, and `/signup` return corresponding HTML files. Additional pages include dedicated dashboards and a virtual event page.

Blueprints registered:
- 🔐 Auth: `auth_bp`
- 👥 Organizer: `organizer_bp`
- 🙋 Participant: `participant_bp`
- 🤝 Collaboration: `collab_bp`
- 🖥️ Virtual Event: `virtual_bp`

---

## 🧱 Project Structure


Copy

Insert

. ├─ backend/ │ ├─ app.py # Entry point registering blueprints and serving frontend │ ├─ config.py # Flask app, CORS, secrets, DB config │ └─ routes/ │ ├─ auth.py # auth_bp │ ├─ organizer.py # organizer_bp │ ├─ participant.py # participant_bp │ ├─ collaboration.py # collab_bp │ └─ virtual_event.py # virtual_bp ├─ frontend/ │ ├─ common_dashboard.html │ ├─ signin.html │ ├─ signup.html │ ├─ organizer_dashboard.html │ ├─ participant_dashboard.html │ ├─ virtual_event.html │ └─ assets/ │ ├─ css/ ... │ └─ js/ ... └─ README.md


> Note: Ensure `backend/__init__.py` exists so `backend` is recognized as a package.

---

## ⚙️ Prerequisites

- 🐍 Python 3.10+ recommended
- 📦 pip and virtualenv (or uv/poetry if preferred)
- 🍃 MongoDB (if you use `MONGO_URI` in `config.py`)
- 🔑 Environment variables (see below)

---

## 🚀 Setup

1) Clone and enter the project

Copy

Insert

git clone cd


2) Create and activate a virtual environment

Copy

Insert

python -m venv .venv

Windows
.venv\Scripts\activate

macOS/Linux
source .venv/bin/activate


3) Install dependencies

Copy

Insert

pip install -r requirements.txt


4) Create an environment file

Copy

Insert

cp .env.example .env # if you have a template

Otherwise, create .env and add the variables below

---

## 🔐 Environment Variables

The app reads common variables such as:

- `SECRET_KEY` — Flask session secret (required in production)
- `MONGO_URI` — connection string for MongoDB (if used by your routes)
- `PORT` — port to run on (default 5000)
- `FLASK_DEBUG` — set to `True` for dev, `False` for prod

Example `.env`:

Copy

Insert

SECRET_KEY=supersecretkey MONGO_URI=mongodb://localhost:27017/your-db PORT=5000 FLASK_DEBUG=True


---

## ▶️ Run the App

Recommended (module mode from project root):

Copy

Insert

python -m backend.app


Alternative (Flask CLI):

Copy

Insert

PowerShell (Windows)
$env:FLASK_APP="backend.app"; $env:FLASK_DEBUG="True"; flask run --host 0.0.0.0 --port 5000

macOS/Linux
export FLASK_APP=backend.app export FLASK_DEBUG=True flask run --host 0.0.0.0 --port 5000


By default, the app binds to `0.0.0.0:5000`. Visit:
- 🏠 `GET /` → `frontend/common_dashboard.html`
- 🔑 `GET /signin` → `frontend/signin.html`
- 📝 `GET /signup` → `frontend/signup.html`
- 🗂️ `GET /organizer_dashboard` → `frontend/organizer_dashboard.html`
- 🙋 `GET /participant_dashboard` → `frontend/participant_dashboard.html`
- 🖥️ `GET /virtual_event` → `frontend/virtual_event.html`
- 🎨 `GET /assets/<path>` → serves from `frontend/assets/`

All other unknown paths will fallback to `common_dashboard.html`.

---

## 🔌 API Blueprints

The following blueprints are registered in `backend/app.py`:
- `auth_bp` (from `backend.routes.auth`)
- `organizer_bp` (from `backend.routes.organizer`)
- `participant_bp` (from `backend.routes.participant`)
- `collab_bp` (from `backend.routes.collaboration`)
- `virtual_bp` (from `backend.routes.virtual_event`)

> Check each file under `backend/routes/` for their specific endpoints and methods.

---

## 🧑‍💻 Development Tips

- Always run commands from the project root (the directory containing `backend/` and `frontend/`).
- Ensure `backend/` is a Python package by adding `backend/__init__.py` (can be empty).
- Do not attempt to `pip install backend.config` — `backend.config` is a local module (file `backend/config.py`), not a PyPI package.
- Use module execution for reliable imports:
  - `python -m backend.app`
- If you add new pages:
  - Place HTML under `frontend/` and assets under `frontend/assets/`
  - Add a new route in `backend/app.py` if you want a friendly path:
    - e.g., `@app.route("/my_new_page")` → returns `send_from_directory(FRONTEND_DIR, "my_new_page.html")`

---

## 🧪 Testing (Optional)

- Add test dependencies to `requirements-dev.txt`.
- Use `pytest`:

pytest -q

- Keep test data and cache out of Git via `.gitignore`.

---

## 🛡️ Production Notes

- Set `FLASK_DEBUG=False` and a strong `SECRET_KEY`.
- Serve static files via a proper web server/CDN if traffic is high.
- Run the app behind a WSGI server (e.g., gunicorn or waitress):

pip install gunicorn gunicorn -w 4 -b 0.0.0.0:${PORT:-5000} backend.app:app

- Configure CORS appropriately in `config.py` if exposing APIs cross-origin.

---

## 🧰 Troubleshooting

- pip error: “Could not find a version that satisfies the requirement backend.config”
  - Remove any occurrence of `backend.config` from requirements/setup files.
  - This is a local module; run the app with `python -m backend.app` from the project root.
- ModuleNotFoundError: No module named 'backend'
  - Ensure `backend/__init__.py` exists.
  - Run from the repo root (not from inside `backend/`).
  - Verify your virtualenv is active.

---

## 🤝 Contributing

- Fork, create a feature branch, commit with clear messages, open a PR.
- Keep routes cohesive and documented.
- Add/update frontend pages under `frontend/` when adding new views.

---

## 📄 License

Add your chosen license here (e.g., MIT, Apache-2.0).