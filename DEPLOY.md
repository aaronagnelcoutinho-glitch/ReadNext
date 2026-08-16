# Deployment guide (Vercel + Fly.io)

Overview
--
This project can be deployed with the frontend on Vercel and backend + processing on Fly.io (Docker). The processing service requires system packages (Tesseract, poppler) so Fly.io Docker is a good fit.

Prerequisites
--
- GitHub repo for the project
- `flyctl` installed and logged-in (for Fly.io)
- Vercel account (connect GitHub)

Frontend (Vercel)
--
1. Push `frontend/` to GitHub (root of repo is fine). Connect the repo to Vercel.
2. Point Vercel to the `frontend` folder as the project root. Build command: `npm run build`. Output dir: adjust if using SvelteKit adapter (`.svelte-kit/output` or `build`).
3. Set any environment variables in Vercel (e.g. `VITE_API_BASE=https://your-backend.fly.dev`).

Backend & Processing (Fly.io)
--
1. Install and login to Fly.io: `flyctl auth login`
2. From repo root create two apps or one combined app. Example for separate apps:

  # Backend
  cd backend
  flyctl launch --name readnext-backend --region ord --dockerfile Dockerfile --no-deploy
  flyctl deploy

  # Processing
  cd ../processing
  flyctl launch --name readnext-processing --region ord --dockerfile Dockerfile --no-deploy
  flyctl deploy

3. Configure backend `config/index.js` (or env) to point `engineUrl` at the processing app URL (e.g. `https://readnext-processing.fly.dev`).

Notes
--
- Free tiers have resource limits; keep models small for demos.
- If you prefer a single host, combine services into one Docker app and use a process manager (e.g. `concurrently`) to run both Node and Uvicorn.
