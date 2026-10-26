# BuildSight — AI-Powered Construction Intelligence

BuildSight brings visual site observations, preliminary construction estimates, weather context, and construction guidance into one workspace. It is built with **React + Vite** on the frontend and **FastAPI** on the backend.

> **Important:** BuildSight provides decision support, not certified engineering or safety approval. Verify findings and recommendations with qualified site professionals and applicable local requirements.

## Screenshots

Add screenshots of your running application to `docs/screenshots/`, then uncomment or adapt the image links below. GitHub renders these images directly in this README.

Suggested screenshots:

- `docs/screenshots/overview.png` — main dashboard
- `docs/screenshots/site-safety-audit.png` — site safety image analysis
- `docs/screenshots/crack-analysis.png` — crack analysis results
- `docs/screenshots/planning-valuation.png` — construction estimate and plan
- `docs/screenshots/construction-assistant.png` — assistant and report view

<!-- Remove the comment markers below after adding the screenshot files. -->
<!--
### Overview
![BuildSight overview dashboard](docs/screenshots/overview.png)

### Site Safety Audit
![BuildSight site safety audit](docs/screenshots/site-safety-audit.png)

### Crack Analysis
![BuildSight crack analysis](docs/screenshots/crack-analysis.png)

### Planning & Valuation
![BuildSight planning and valuation](docs/screenshots/planning-valuation.png)

### Construction Assistant
![BuildSight construction assistant](docs/screenshots/construction-assistant.png)
-->

### How to add screenshots

1. Run the frontend and backend locally and open the page you want to capture.
2. Take a clear screenshot (preferably 1440px wide for desktop views). Hide API keys, personal information, and any sensitive project details.
3. Create `docs/screenshots/` in the repository and save the images using the filenames listed above.
4. Uncomment the Markdown image section above, or add individual image links wherever you want them to appear.
5. Commit the images and README changes to GitHub.

## Features

- **Site Safety Audit:** Gemini multimodal image analysis for visible site-safety observations. Provider results are schema validated. If no key is configured, image analysis reports that it is unavailable instead of fabricating a result.
- **Crack Analysis:** AI-assisted descriptions of visible crack patterns and cautious next steps. Crack dimensions are not inferred from ordinary photos without a scale.
- **Planning & Valuation:** Deterministic indicative INR estimates using area, location tier, property tier, and structure type inputs, with editable material quantities, cost breakdowns, schedules, and resource plans.
- **Construction Assistant:** Construction guidance and a daily-report workflow. Offline guidance is labeled; Gemini responses require a configured API key.
- **Weather:** Open-Meteo geocoding, current conditions, and a three-day forecast for a user-entered location or coordinates. Weather is reported as unavailable when the location or provider cannot be resolved.
- **Reporting:** Structured safety/crack assessment exports, assistant report copying/export, browser printing, and a persisted light/dark theme preference.
- **API safeguards:** CORS configured through environment variables and strict Pydantic response models.

AI visual observations do not verify compliance or replace a physical site inspection. Planning figures use broad, transparent tier factors and are indicative estimates—not professional valuations, quotations, or guarantees.

## Tech stack

- Frontend: React, Vite, Axios, CSS
- Backend: FastAPI, Pydantic, Uvicorn
- AI image analysis and assistant: Google Gemini API (optional; configured on the backend)
- Weather data: Open-Meteo

## Prerequisites

- Python 3.10+ (use the version supported by your backend dependencies)
- Node.js and npm
- Optional: a Gemini API key for AI-powered features

## Local setup (Windows PowerShell)

### 1. Start the backend

Open a terminal at the repository root:

```powershell
cd backend
Copy-Item .env.example .env
# Optional: edit .env and set GEMINI_API_KEY. Keep this key backend-only.
.\.venv-new\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

If your virtual environment has a different name, activate that environment instead. If you do not use a virtual environment yet, create one before installing dependencies.

Backend health check: [http://localhost:8000/api/health](http://localhost:8000/api/health)  
Interactive API docs: [http://localhost:8000/docs](http://localhost:8000/docs)

### 2. Start the frontend

Open a second terminal at the repository root:

```powershell
cd frontend
npm install
npm run dev
```

Open [http://localhost:5173](http://localhost:5173).

To point the UI at a hosted API, set `VITE_API_URL` in `frontend/.env` before building, for example:

```env
VITE_API_URL=https://your-backend.example.com
```

Restart the Vite development server after changing environment variables.

## Tests and production build

Run backend tests:

```powershell
cd backend
python -m pytest
```

Build the frontend:

```powershell
cd frontend
npm run build
```

## Deployment

### Backend (FastAPI)

Deploy `backend` as a Python web service. Set the service root directory to `backend`, install dependencies with `pip install -r requirements.txt`, and use this start command:

```bash
uvicorn main:app --host 0.0.0.0 --port $PORT
```

Configure `GEMINI_API_KEY` and, optionally, `GEMINI_MODEL` as backend environment variables. Set `CORS_ORIGINS` to include the exact deployed frontend origin. Never expose the Gemini key in frontend variables or commit it to Git.

### Frontend (Vercel or another static host)

Deploy the `frontend` directory as a Vite static site. Use `npm ci` when a lockfile is present (otherwise use `npm install`), build with `npm run build`, and publish `dist`. Set `VITE_API_URL` to the deployed backend URL at **build time**.

## GitHub

Ensure `.env`, virtual environments, `node_modules`, and build output are excluded by `.gitignore`. From the repository root, after creating an empty GitHub repository, run:

```powershell
git init
git add README.md .gitignore backend frontend docs
git commit -m "Rebrand project as BuildSight"
git branch -M main
git remote add origin <YOUR_REPOSITORY_URL>
git push -u origin main
```

Replace `<YOUR_REPOSITORY_URL>` with your repository URL. Do not commit `.env` files, API keys, credentials, or private project screenshots.

## Safety and estimation disclaimer

BuildSight is an informational decision-support tool. AI image observations may be incomplete or incorrect and do not certify PPE compliance, structural integrity, or code compliance. Crack dimensions cannot be reliably measured from an ordinary photo without a suitable scale and assessment method. Cost and schedule outputs are preliminary estimates and should be reviewed by qualified professionals before use in real projects.
