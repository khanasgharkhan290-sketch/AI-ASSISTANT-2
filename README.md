# AI-ASSISTANT-2
# 📄 ATS Resume Analyzer

A Streamlit app that scores a resume for ATS (Applicant Tracking System) friendliness and suggests concrete improvements, powered by Google Gemini Flash.

## Features

- Upload a **PDF or DOCX** resume
- Optional **job description** for a targeted keyword match
- **Estimated ATS score (0-100)** with a breakdown:
  - Format & parseability (email, phone, length, bullets) - *rule-based*
  - Section structure (Experience, Education, Skills...) - *rule-based*
  - Keywords & relevance, Impact & achievements, Clarity & language - *Gemini*
- Prioritised improvements, present/missing keywords, and before/after bullet rewrites
- "Extracted text" view showing what an ATS parser actually sees

> The score is an **estimate**. Real ATS products differ in how they parse and rank resumes.

## Project structure

```
.
├── app.py
├── requirements.txt
└── README.md
```

## Run locally

1. Get a free Gemini API key at <https://aistudio.google.com/apikey>.
2. Install and run:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

export GEMINI_API_KEY="your_key_here"      # Windows PowerShell: $env:GEMINI_API_KEY="your_key_here"
streamlit run app.py
```

You can also skip the environment variable and paste the key into the sidebar.

Optional: set `GEMINI_MODEL` (default `gemini-2.5-flash`) to use a different Gemini Flash model.

### Local secrets file (alternative)

Create `.streamlit/secrets.toml` (never commit it):

```toml
GEMINI_API_KEY = "your_key_here"
# GEMINI_MODEL = "gemini-2.5-flash"
```

## Push to GitHub

Create a `.gitignore` first so secrets never get uploaded:

```bash
printf "venv/\n__pycache__/\n.env\n.streamlit/secrets.toml\n" > .gitignore
```

Then:

```bash
git init
git add .
git commit -m "Initial commit: ATS resume analyzer"
git branch -M main
```

Create an **empty** repository on GitHub (github.com → New), then:

```bash
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

If GitHub asks for a password, use a **Personal Access Token** (Settings → Developer settings → Tokens) or sign in with GitHub CLI (`gh auth login`).

## Deploy on Streamlit Community Cloud

1. Go to <https://share.streamlit.io> and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Choose your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings → Secrets** and paste:

   ```toml
   GEMINI_API_KEY = "your_key_here"
   ```

5. Click **Deploy**. The first build takes a couple of minutes.

To update the app later, just `git push` - it redeploys automatically.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Gemini rejected the API key" | Re-copy the key; make sure it's from Google AI Studio. |
| "Model was not found" | Change the model in the sidebar (e.g. `gemini-2.5-flash`). |
| "Rate limit / quota reached" | Wait a minute; free-tier limits are low. |
| "Almost no readable text" | The resume is a scanned image. Export a text-based PDF/DOCX. |
| `ModuleNotFoundError` | Run `pip install -r requirements.txt` in your active environment. |

## Privacy

Resume text is sent to the Gemini API for analysis. The app does not save uploaded files. If you deploy publicly, tell users their text is processed by Google, and don't put your API key in the code or the repo.
