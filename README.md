# Subh's CV Chatbot

A grounded Streamlit chatbot that answers questions using only Subh
Bhattacharyya's CV. If the answer is not supported by the CV, the application
returns exactly:

> I’m sorry, but Subh’s CV doesn’t clearly provide an answer to that question.
> Please call Subh at 0492205682 for further information.

The app also accepts a direct public job-description URL or pasted job-description
text and produces a 0–100 suitability score, overall justification, evidence-based
strengths, and gaps or requirements that are not clearly evidenced in the CV.
It can then suggest fact-preserving CV improvements, provide an editable tailored
draft, validate user edits against the original CV, and export the approved CV as
DOCX or PDF.

The app uses Google's official `google-genai` SDK and defaults to the stable
`gemini-3.5-flash-lite` model, which is available on the Gemini API free tier
(subject to Google's current limits and regional availability).

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

The supplied CV is stored at `assets/CV PM_Subh Bhatt.pdf`, so the default works
locally and in cloud deployments. You can still upload a different PDF in the
sidebar or override the default with `CV_PATH`.

## Deploy

For Streamlit Community Cloud, add `GEMINI_API_KEY` under **App settings →
Secrets**. The default CV is already included in the repository. Do not commit
API keys or a local `.streamlit/secrets.toml` file.

## Grounding safeguards

- Every question is sent with the extracted text of the CV.
- Gemini must return structured JSON declaring whether the answer is supported.
- The friendly fallback is inserted by application code—not left to model wording.
- Invalid or empty model output also resolves to the fallback.
- Job links are read using Gemini URL Context; the app server does not fetch
  arbitrary URLs itself.
- LinkedIn and other login-protected job boards can be assessed by copying and
  pasting the job-description text into the app.
- Job suitability uses a documented 100-point rubric and distinguishes a genuine
  mismatch from experience that is simply not evidenced in the CV.
- Tailored CV drafts can reorder, clarify and foreground existing evidence but are
  explicitly instructed not to add or strengthen facts.
- A separate Gemini audit blocks DOCX/PDF export when edited claims are unsupported
  by, contradictory to, or stronger than the original CV.
