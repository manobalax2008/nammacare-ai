# NammaCare AI

**The right help, in the language you understand.**

NammaCare AI is an independent student project to help people in Tamil Nadu understand public-service information in simple Tamil or English. It is not a government website and is not affiliated with the Government of Tamil Nadu.

## Current implementation

- React + TypeScript + Vite frontend with responsive styling and English/Tamil interface toggle.
- Express backend, SQLite persistence, password hashing, signed HTTP-only session cookie, server-side authorization, input validation, rate limiting and security headers.
- Ollama integration for real local inference (default `gemma3:4b`); the backend reports an error if Ollama/model is unavailable rather than pretending AI worked.
- Service directory, official-source metadata, chat with retrieved source records, pasted-text simplifier, checklists and basic account/dashboard flow.
- Seed data is explicitly marked **needs review** until the official sources are independently checked. The assistant must not treat unverified records as verified requirements.
- Initial admin is assigned through the CLI command; no user is made admin automatically.

## Requirements

- Node.js 20+
- Ollama installed and running for local AI
- Git (optional, for version control)

## Run locally on macOS

```bash
cp .env.example .env
npm install
ollama pull gemma3:4b
ollama serve
```

In another terminal, run:

```bash
npm run dev
```

Open the Vite URL shown in the terminal (usually `http://localhost:5173`). The backend runs on `http://localhost:8787`.

If `ollama serve` says an instance is already running, leave the existing Ollama service running. The first model download needs internet; inference runs locally after it is downloaded.

## Configure secrets

Edit `.env` and set `SESSION_SECRET` to a long random value. Never commit `.env`. The AI request is sent from the backend to your local Ollama service; no AI provider API key is required for this local mode.

`DEMO_MODE=true` enables explicitly labelled sample interactions only when you intentionally want a UI demonstration. With `DEMO_MODE=false`, missing Ollama returns a real configuration error.

## Database and administrator

SQLite is created automatically on first backend start at `DATABASE_PATH`. To assign an existing account as administrator:

```bash
npm run make-admin -w backend -- your-account@example.com
```

This is a local machine-owner command; it is not exposed as a public endpoint. It only promotes an account that already exists. Do not run it on a shared machine you do not control.

## Build and tests

```bash
npm run build
npm test
```

The build command checks the frontend build and backend TypeScript. Tests use Node's test runner. Run them in the same environment where you will demo the project.

## GitHub

Create one repository named `namma-care-ai`, then:

```bash
git init
git add .
git commit -m "Build NammaCare AI local-first MVP"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/namma-care-ai.git
git push -u origin main
```

Check `git status` and ensure `.env`, database files and credentials are not staged.

## Deployment notes

The default architecture is intended for a local hackathon demo. A public deployment needs a hosted backend and a model endpoint that is safely reachable by that backend. Do not expose your laptop's Ollama port to the internet. Static hosting alone cannot run the local model. Hosting, database and model inference may have separate quotas or costs.

## Privacy and limitations

- This starter stores account data, conversation history and checklists in a local SQLite database. Avoid entering Aadhaar numbers, bank details, passwords or identity-document contents.
- This first release simplifies pasted text only; it does not implement PDF extraction or OCR.
- Service records seeded for development are marked `needs_review`. Before a public demo, check each official URL and the current rules yourself and record a real verification date. Do not present unverified details as official requirements.
- Password reset email delivery is not configured in this local starter; add a verified email provider before offering it publicly.
- This is an MVP scaffold and should receive security review before real public use.

## Manual demo checklist

1. Open the homepage on desktop and mobile width.
2. Browse services and open a service detail.
3. Start Ollama and ask a Tamil question; verify response and source metadata.
4. Stop Ollama and confirm the UI shows an honest error.
5. Paste official instructions into the simplifier.
6. Sign up, create a checklist, and refresh to verify persistence.
7. Sign in as another user and confirm the first user's records are not visible.
8. Confirm admin endpoints reject non-admin requests.
9. Test language toggle and print/download controls.
10. Check that `.env` is ignored by Git.
