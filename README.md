# MedBuddy

**Medical Report and Doctors Prescription Explainer**

Upload a lab report or a doctor's prescription and get a clear, safe explanation in simple English, Hindi or Hinglish.

> **Status:** in development. Phase 1 (lab reports) is being built first. Phase 2 (prescriptions) is planned.

> **Disclaimer:** MedBuddy explains medical papers to help people understand them. It is **not** a medical device, does **not** diagnose, and does **not** replace a doctor or a pharmacist. The lab report explainer gives no medicine or dosage advice. The prescription explainer only restates what the doctor wrote and never changes it. Always confirm with your doctor or pharmacist.

---

## The problem

**Lab reports**

- **Hard to read.** Reports list values, units and reference ranges with no explanation. A "High" flag can cause panic, and an important value can get ignored.
- **Short doctor visits.** Patients often leave with questions unanswered.
- **Language barrier.** Reports are in English with technical terms, while many patients think in Hindi or a regional language.
- **Waiting causes worry.** Results often arrive on WhatsApp or a lab portal days before the appointment, and people turn to Google, which can give frightening or unrelated results.
- **Existing tools fall short.** General chatbots can explain pasted values, but they don't reliably read real Indian lab report formats, don't guarantee correct high/low flags, and can overstep into diagnosis.

**Prescriptions**

- **Hard to read.** Prescriptions are written in handwriting and shorthand that many patients cannot read.
- **Dependence on the medical shop.** People rely on the medical store owner to read the prescription, and in a crowded shop the dosage information can be passed on incorrectly.

## How it works

**Code checks, AI explains.** Whether a lab value is normal, low, high or urgent is decided by reference ranges and code, never by an AI guess. For prescriptions, code checks and validates what was read, and the user confirms it. The AI only writes the plain-language explanation.

**Phase 1: lab report explainer**

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

**Phase 2: prescription explainer (planned)**

```
Prescription photo
        │
        ▼
 1. Clean-up      →  grayscale, contrast, noise removal, straightening
        │
        ▼
 2. Extraction    →  medicine, strength, frequency, duration  ("unclear" if unsure)
        │
        ▼
 3. Code checks   →  match medicine names, decode shorthand (1-0-1, BD, SOS), reject impossible values
        │
        ▼
 4. User confirms →  extracted fields shown beside the photo
        │
        ▼
 5. Explanation   →  plain-language instructions, exactly as the doctor wrote them
```

## Features

**Phase 1: lab reports**

- [ ] Upload a report (PDF or photo), with consent and a "don't save" option
- [ ] Extract test name, value, unit and reference range
- [ ] Flag each value as normal, low, high or urgent
- [ ] Plain-language explanation of each abnormal value
- [ ] Summary with the most important items first
- [ ] "Questions to ask your doctor" list
- [ ] English, Hindi and Hinglish output
- [ ] Safety messages and disclaimers

**Phase 2: prescriptions (planned)**

- [ ] Upload a photo of a prescription, with consent and a "don't save" option
- [ ] Read medicine name, strength, frequency and duration
- [ ] Mark unclear text instead of guessing
- [ ] Decode dosage shorthand with a lookup table
- [ ] Let the user confirm or correct the extracted text before it is explained
- [ ] Plain-language instructions in English, Hindi and Hinglish
- [ ] Printed prescriptions first, then handwritten ones

## Initial focus

The project starts by targeting a few of the most important and commonly ordered lab reports, so each one can be read, flagged and explained reliably. More report types will be added as the project grows. The prescription explainer follows once the lab report flow works end to end.

Development and testing use **synthetic or consented samples only**.

## Safety rules

- No diagnosis and no naming of diseases the person might have
- The lab report explainer gives no medicine or dosage advice
- The prescription explainer only restates what the doctor wrote. It never changes, recommends or substitutes a medicine or a dose
- A clear "contact a doctor" message for very abnormal values
- Lab flags come from code and reference ranges, not from the AI
- Text read from a prescription is confirmed by the user, and unclear parts are marked instead of guessed
- Nothing is saved unless the user consents

## Tech stack

| Part | Tools |
|---|---|
| Language | Python |
| Reading PDFs | pdfplumber, PyMuPDF |
| Reading lab report photos (OCR) | Tesseract |
| Flagging rules | Python rules + reference range table |
| Explanations and Hindi | LLM API (Claude) |
| Interface | Streamlit |
| Storage (optional) | SQLite |
| Charts (optional) | Plotly |
| Prescription image clean-up (Phase 2) | OpenCV, Pillow |
| Handwriting reading (Phase 2) | Vision-capable LLM (Claude API) |
| Medicine name matching (Phase 2) | rapidfuzz + a public medicine dataset |
| Output validation (Phase 2) | pydantic |
| Deployment | Streamlit Community Cloud or Hugging Face Spaces |

## Project structure

```
├── app.py                  Streamlit UI
├── data/
│   ├── reference_ranges.json
│   ├── sample_reports/     synthetic reports only
│   ├── medicines.csv       (Phase 2, planned)
│   └── dosage_shorthand.json  (Phase 2, planned)
├── docs/                   notes and documentation
├── src/mre/
│   ├── models.py           data shapes
│   ├── pipeline.py         connects all the steps
│   ├── extraction/         PDF reader, OCR, parser
│   ├── rules/              flagging logic
│   ├── explain/            LLM calls, prompts, safety
│   ├── prescription/       (Phase 2, planned)
│   ├── storage/            optional SQLite
│   └── charts.py           optional trend charts
└── tests/
```

## Getting started

**Requirements:** Python 3.10+, Git, and [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) (only needed for photo lab reports).

```bash
# 1. Clone
git clone https://github.com/sarthaktripathi444-sys/Medical-Report-Prescription-Explainer.git
cd Medical-Report-Prescription-Explainer

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
4. Never commit real patient reports, prescriptions or your `.env` file

See [CONTRIBUTING.md](CONTRIBUTING.md) for more details.

## License

Licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
