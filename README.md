# CV-JD Compatibility Checker

A generic Streamlit application for comparing any uploaded CV with a job
description. Users can upload a PDF or DOCX CV, paste a job description or
provide a public job link, and receive:

- A 0-100 compatibility score and justification
- Evidence-based strengths
- Gaps or requirements not clearly evidenced in the CV
- Fact-preserving recommendations to improve the CV
- A complete editable tailored CV
- A validated Microsoft Word download

The app uses Google's official `google-genai` SDK and defaults to
`gemini-3.5-flash-lite`.

## Run locally

1. Create a free Gemini API key in [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Create and activate a virtual environment, then install dependencies:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. Set your API key:

   ```bash
   export GEMINI_API_KEY="your-key"
   ```

4. Start the app:

   ```bash
   streamlit run app.py
   ```

Every user uploads their own CV. The application does not preload a candidate
or retain uploaded documents between sessions.

## Deploy

For Streamlit Community Cloud, add `GEMINI_API_KEY` under **App settings ->
Secrets**. Do not commit API keys or a local `.streamlit/secrets.toml` file.

## Factual safeguards

- Compatibility is assessed only from the uploaded CV and supplied job description.
- Job links are read using Gemini URL Context; the app server does not fetch
  arbitrary URLs itself.
- LinkedIn and other login-protected job boards can be assessed by copying and
  pasting the job-description text into the app.
- Missing evidence is described as unclear or unverified, not as proof that the
  candidate lacks a skill.
- Tailored CV drafts can reorder, clarify and foreground existing evidence but are
  explicitly instructed not to add or strengthen facts.
- A separate Gemini audit blocks Word export when edited claims are unsupported,
  contradictory to, or stronger than the uploaded CV.
