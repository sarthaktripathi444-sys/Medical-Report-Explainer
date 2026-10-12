# Medical Report Explainer

Upload a lab report (PDF or photo) and get a clear, safe explanation in simple English, Hindi or Hinglish.

> **Status:** in development

> **Disclaimer:** This project explains lab values to help people understand their reports. It is **not** a medical device, does **not** diagnose, and does **not** give medicine or dosage advice. Always consult a doctor.

---

## The problem

- **Lab reports are hard to read.** They list values, units and reference ranges with no explanation. A "High" flag can cause panic, and an important value can get ignored.
- **Doctor visits are short.** Patients often leave with questions unanswered.
- **Language barrier.** Reports are in English with technical terms, while many patients think in Hindi or a regional language.
- **Waiting causes worry.** Results often arrive on WhatsApp or a lab portal days before the appointment, and people turn to Google, which can give frightening or unrelated results.
- **Existing tools fall short.** General chatbots can explain pasted values, but they don't reliably read real Indian lab report formats, don't guarantee correct high/low flags, and can overstep into diagnosis.

## How it works

**Rules decide, AI explains.** Whether a value is normal, low, high or urgent is decided by reference ranges and code, never by an AI guess. The AI only writes the plain-language explanation for values that are already flagged.

```
Report (PDF / photo)
        │
        ▼
 1. Extraction   →  table of test, value, unit, reference range
        │
        ▼
 2. Rules        →  normal / low / high / urgent   (code only, no AI)
        │
        ▼
 3. Explanation  →  simple text + questions to ask your doctor (with safety checks)
        │
        ▼
 4. Streamlit UI →  results, most important items first
```

## Features

- [ ] Upload a report (PDF or photo), with consent and a "don't save" option
- [ ] Extract test name, value, unit and reference range
- [ ] Flag each value as normal, low, high or urgent
- [ ] Plain-language explanation of each abnormal value
- [ ] Summary with the most important items first
- [ ] "Questions to ask your doctor" list
- [ ] English, Hindi and Hinglish output
- [ ] Safety messages and disclaimers
- [ ] Optional: trend charts across several reports
- [ ] Optional: one-page summary PDF

## Initial focus

The project starts by targeting a few of the most important and commonly ordered lab reports, so each one can be read, flagged and explained reliably. More report types will be added as the project grows.

Development and testing use **synthetic or consented sample reports only**.

## Safety rules

- No diagnosis and no naming of diseases the person might have
- No medicine or dosage advice
- A clear "contact a doctor" message for very abnormal values
- Flags come from code and reference ranges, not from the AI
- Reports are not saved unless the user consents

## Tech stack

| Part | Tools |
|---|---|
| Language | Python |
| Reading PDFs | pdfplumber, PyMuPDF |
| Reading photos (OCR) | Tesseract |
| Flagging rules | Python rules + reference range table |
| Explanations and Hindi | LLM API (Claude) |
| Interface | Streamlit |
| Storage (optional) | SQLite |
| Charts | Plotly |
| Deployment | Streamlit Community Cloud or Hugging Face Spaces |

## Project structure

```
├── app.py                  Streamlit UI
├── data/
│   ├── reference_ranges.json
│   └── sample_reports/     synthetic reports only
├── docs/                   notes and documentation
├── src/mre/
│   ├── models.py           data shapes
│   ├── pipeline.py         connects all the steps
│   ├── extraction/         PDF reader, OCR, parser
│   ├── rules/              flagging logic
│   ├── explain/            LLM calls, prompts, safety
│   ├── storage/            optional SQLite
│   └── charts.py           optional trend charts
└── tests/
```

## Getting started

**Requirements:** Python 3.10+, Git, and [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) (only needed for photo reports).

```bash
# 1. Clone
git clone https://github.com/sarthaktripathi444-sys/Medical-Report-Explainer.git
cd Medical-Report-Explainer

# 2. Virtual environment
python -m venv .venv
.venv\Scripts\activate            # Mac/Linux: source .venv/bin/activate

# 3. Install
pip install -r requirements.txt

# 4. Add your API key
copy .env.example .env            # Mac/Linux: cp .env.example .env
# open .env and replace your-key-here with your real key

# 5. Run
streamlit run app.py
```

Run the tests with:

```bash
pytest
```

## Contributing

1. Create a branch from `main`, for example `feature/pdf-extraction`
2. Make small commits and run `pytest`
3. Push the branch and open a Pull Request
4. Never commit real patient reports or your `.env` file

## License

Licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
