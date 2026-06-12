---
name: seas-appointment
description: Generates a complete Harvard SEAS research appointment package for a new recruit. Use this skill whenever a PI or faculty member at Harvard SEAS wants to onboard a new visitor, postdoc, research intern, fellow, visiting scholar, or any research appointee — even if they don't use those exact words. Trigger on phrases like "I'm recruiting", "new student joining my lab", "visiting researcher", "how do I onboard", "what forms do I need", "appointment package", or any mention of a recruit's name alongside a start date or research topic. The skill asks for recruit details, determines the correct appointment category, downloads and pre-fills the offer letter, and produces a ready-to-sign document package with a clear PI action checklist.
---

# Harvard SEAS Research Appointment Skill

This skill streamlines the research appointment process for Harvard SEAS PIs. Given basic recruit information, it determines the correct appointment category, downloads and pre-fills the offer letter template, and assembles a complete form package — eliminating the back-and-forth between PIs, admins, and the Academic Appointments Office.

---

## Step 1: Collect Recruit Information

Open with this message and direct the PI to the intake form. Do not ask questions manually unless the PI provides information directly in chat.

---

Welcome! I'll help you prepare the appointment package for your recruit.

**Please start by filling out the intake form:**
📄 Open this file in your browser: `/Users/nali/.claude/skills/seas-appointment/forms/intake_form.html`

Fill in all the fields, click **Download Intake Summary**, and upload the downloaded `.txt` file here. I'll take it from there.

*For reference on appointment categories, see the [SEAS Research Appointment Categories](https://seas.harvard.edu/office-faculty-affairs/research-appointments/research-appointment-categories) page.*

---

If the PI provides information directly in chat instead of uploading a file, extract the following fields from what they share:

- PI full name and title
- Recruit full name, email, home institution
- Visiting Scholar (tenured academic on leave)? Yes/No
- Degree status and date
- *(If currently pursuing PhD)* Nature of hire: visiting PhD student or incoming postdoc?
- *(If PhD received or incoming postdoc)* Intention and experience since PhD
- Research topic, start date, end date
- Paid or unpaid? If paid: stipend or salaried employee? Funding source? Annual amount?
- Visa/work authorization status
- *(If J-1)* English proficiency methods
- *(If postdoc/senior)* Full-time or part-time, office space, research allowance, travel allowance
- *(If stipend, postdoc)* Funding source
- *(If salaried employee, postdoc)* Additional duties
- *(If unpaid, postdoc)* External funding source

---

## Step 2: Determine Appointment Category

Use the decision logic below to automatically determine the correct category from the PI's answers. State the chosen category and explain the reasoning briefly.

**Decision logic (apply in order):**

1. **Tenured/established academic on leave?** → **Visiting Scholar**
2. **Currently pursuing bachelor's?** → **VURI**
3. **Currently pursuing master's or PhD?** → **Fellow**
4. **Received PhD** → default to **Postdoctoral Fellow**, unless PI's description of intent and experience clearly indicates a senior category:
   - ~3–4 years postdoctoral/research experience → **Research Associate**
   - ~5–9 years → **Research Scientist**
   - ~10+ years → **Senior Research Scientist**
   - If unclear, default to Postdoctoral Fellow and note that the Academic Appointments Office can confirm.
5. **Unpaid, employed full-time elsewhere or non-ladder faculty at another university?** → **Associate**

*For full official criteria, see the [SEAS Research Appointment Categories](https://seas.harvard.edu/office-faculty-affairs/research-appointments/research-appointment-categories) page.*

| Category | Key Eligibility Criteria |
|---|---|
| **Visiting Undergraduate Research Intern (VURI)** | Current undergraduate student at another institution |
| **Fellow** | Graduate student (master's or PhD); bachelor's required; paid or unpaid; under PI supervision |
| **Postdoctoral Fellow** | Holds a doctoral degree; paid or unpaid; under general PI supervision |
| **Research Associate** | Doctoral degree + 3+ years postdoctoral experience; paid |
| **Research Scientist** | Doctoral degree + 5+ years postdoctoral experience; directs projects; paid |
| **Senior Research Scientist** | Doctoral degree + 10+ years postdoctoral experience; paid |
| **Senior Research Fellow** | Independent researcher at tenured-faculty level; spans multiple faculty programs |
| **Visiting Scholar** | Established academic (typically tenured) on leave from their home institution; paid or unpaid |
| **Associate** | Unpaid only; must be employed full-time elsewhere or hold a non-ladder faculty position at another university |

**Domestic vs. International for letter and forms purposes:**
- Treat as **international** (keep INTERNATIONAL: paragraphs, include ELP form) only if Harvard is **sponsoring a J-1 visa**.
- Treat as **domestic** (remove INTERNATIONAL: paragraphs, no ELP form) if the recruit already has US work authorization — including F-1 OPT/STEM OPT, green card, or US citizenship — regardless of their nationality or country of origin.

---

## Step 3: Download and Fill the Offer Letter

### 3a. Select the correct template

| Category | Compensation | Template URL |
|---|---|---|
| Fellow | Stipend | `https://seas.harvard.edu/media/92933/download?attachment` |
| Fellow | Employee/Salary | `https://seas.harvard.edu/media/93045/download?attachment` |
| Fellow | Unpaid | `https://seas.harvard.edu/media/92934/download?attachment` |
| Postdoctoral Fellow | Stipend | `https://seas.harvard.edu/media/93065/download?attachment` |
| Postdoctoral Fellow | Employee/Salary | `https://seas.harvard.edu/media/93149/download?attachment` |
| Postdoctoral Fellow | Unpaid | `https://seas.harvard.edu/media/93064/download?attachment` |
| Research Associate | Any | `https://seas.harvard.edu/media/93047/download?attachment` |
| Visiting Scholar | Stipend or Salary | `https://seas.harvard.edu/media/92944/download?attachment` |
| Visiting Scholar | Unpaid | `https://seas.harvard.edu/media/92945/download?attachment` |
| Research Scientist / Senior Research Scientist | New hire | `https://seas.harvard.edu/media/92943/download?attachment` |
| Research Scientist / Senior Research Scientist | Promotion | `https://seas.harvard.edu/media/92447/download?attachment` |
| VURI | Stipend | `https://seas.harvard.edu/media/92936/download?attachment` |
| VURI | Unpaid | `https://seas.harvard.edu/media/92937/download?attachment` |
| VURI | Salary/6120 | `https://seas.harvard.edu/media/92935/download?attachment` |
| Associate | New, full-time employed elsewhere | `https://seas.harvard.edu/media/92773/download?attachment` |
| Associate | Departing postdoc/fellow/grad student | `https://seas.harvard.edu/media/92930/download?attachment` |
| Associate | Departing stipendee/VURI/visiting scholar | `https://seas.harvard.edu/media/92931/download?attachment` |

### 3b. Run the base fill script

Download the template using WebFetch, then run the bundled script:

```bash
python3 "/Users/nali/.claude/skills/seas-appointment/scripts/fill_offer_letter.py" \
  --input "<downloaded_docx_path>" \
  --output "~/Downloads/<FirstName>_<LastName>_<Category>_Offer_Letter.docx" \
  --name "<recruit full name>" \
  --email "<recruit email>" \
  --topic "<research topic>" \
  --pi-name "<PI name>" \
  --pi-title "<PI title>" \
  --letter-date "<today's date>" \
  --start-date "<start date>" \
  --end-date "<end date>" \
  --international <true|false> \
  --phd-received <true|false>
```

- `--international true`: keep INTERNATIONAL: paragraphs, strip the prefix
- `--international false`: delete INTERNATIONAL: paragraphs entirely
- `--phd-received true`: salutation is `Dear Dr. Name,` (use for Postdoctoral Fellow and above)
- `--phd-received false`: salutation is `Dear Name,` (use for Fellow, VURI, Visiting Scholar without PhD)

The script also automatically **deletes** the `Dr. FirstName LastName` header line at the top of the letter; only the recruit's email address is kept as the recipient line.

### 3c. Postdoc-specific placeholders (fill after running the script)

The postdoc templates share some placeholders but differ by compensation type. After running the base script, fill these additional fields:

**Shared across all three postdoc templates:**

| Placeholder (run text) | Fill with |
|---|---|
| `Email Address` | recruit email |
| `[SPECIFY ONE: full-time/part-time]` | `full-time` or `part-time` |
| `[OFFICE/DESK SPACE, ...]` | Full sentence: office location, research support, travel allowance |
| `FACULTY MENTOR NAME` | PI name |
| `TITLE` | PI title |

**Stipend template only:**

| Placeholder (run text) | Fill with |
|---|---|
| `[FUNDER NAME]` (appears twice) | Funding source, e.g., `NSF grant` |
| `[SALARY] ` *(note: `$` is a separate run before it)* | Amount only, e.g., `70,000 ` |
| `[Harvard University/FUNDER NAME]` | Payment route, e.g., `Harvard University` |

**Employee template only:**

| Placeholder (run text) | Fill with |
|---|---|
| `[SALARY]` | Amount only, e.g., `70,000` |
| `[ADDITIONAL DUTIES/RESPONSIBILITIES.]` | Description of duties, or delete sentence if none |

**Unpaid template only:**

| Placeholder (run text) | Fill with |
|---|---|
| `[FUNDER NAME]` | External fellowship or funding source name |

Example Python snippet (adapt per compensation type):

```python
from docx import Document
doc = Document(output_path)
for para in doc.paragraphs:
    for run in para.runs:
        # Shared
        run.text = run.text.replace('Dr. FirstName LastName', f'Dr. {name}')
        run.text = run.text.replace('Email Address', email)
        run.text = run.text.replace('[SPECIFY ONE: full-time/part-time]', 'full-time')
        run.text = run.text.replace(
            '[OFFICE/DESK SPACE, RESEARCH ALLOWANCE, TRAVEL ALLOWANCE, INSTITUTE OR CENTER AFFILIATIONS AND RESOURCES, ETC.]',
            office_resources_sentence
        )
        run.text = run.text.replace('FACULTY MENTOR NAME', pi_name)
        run.text = run.text.replace('TITLE', pi_title)
        # Stipend-specific
        run.text = run.text.replace('[FUNDER NAME]', funder)
        run.text = run.text.replace('[SALARY] ', f'{salary_amount} ')
        run.text = run.text.replace('[Harvard University/FUNDER NAME]', 'Harvard University')
        # Employee-specific
        run.text = run.text.replace('[SALARY]', salary_amount)
        run.text = run.text.replace('[ADDITIONAL DUTIES/RESPONSIBILITIES.]', duties)
        # Unpaid-specific
        run.text = run.text.replace('[FUNDER NAME]', external_funder)
doc.save(output_path)
```

---

## Step 4: Identify All Required Forms

### Offer Letter
Always required. Use the pre-filled file from Step 3.

### Harvard University Participation Agreement
Required for **Postdoctoral Fellows** (all compensation types). The offer letter explicitly instructs postdocs to sign this electronically before their start date.
URL: `https://seas.harvard.edu/media/75051/download?attachment`

### Visitor Participation Agreement (VPA)
Required for **all non-postdoc** appointments. Select based on home institution type:

| Institution Type | VPA URL |
|---|---|
| US non-profit university or government (not Harvard medical) | `https://research.harvard.edu/files/2025/07/visitor_pa_for_visitors_from_us_non-profit_institutions_other_than_harvard_amcs_final_7-26-22.pdf` |
| For-profit company (US or foreign) | `https://research.harvard.edu/files/2022/10/visitor_pa_for_visitors_from_for-profit_organizations_final_11-7-2014.pdf` |
| Foreign non-profit university or institution | `https://research.harvard.edu/files/2025/07/visitor-PA-for-visitors-from-foreign-non-profits-FINAL-7-2-2025.pdf` |
| Unaffiliated individual | `https://research.harvard.edu/files/2025/07/visitor-PA-for-unaffiliated-visitors-FINAL-7-2-2025.pdf` |
| Harvard-affiliated medical center | `https://research.harvard.edu/files/2022/10/vpa_for_harvard_university_affiliated_academic_medical_centers_002.pdf` |
| US Government agency | `https://research.harvard.edu/files/2022/10/harvard_university_visitor_participation_agreement_for_visitors_from_us_government_only.pdf` |

### Acknowledgement of Risk and Release
Required for **all unpaid** appointments (including unpaid postdocs).
URL: `https://seas.harvard.edu/media/75046/download?attachment`

### English Language Proficiency (ELP) Form
Required for **all international** visitors (J-1 visa requirement). The PI fills and signs it.
URL: `https://seas.harvard.edu/media/90775/download?attachment`

---

## Step 5: Output the Appointment Package

Use this exact structure:

---

### Appointment Summary — [Recruit Name]

**Category:** [Category], [Paid/Unpaid]
**Dates:** [Start] – [End]
**Research Topic:** [Topic]
**PI:** [PI Name], [PI Title]

---

### Complete Form Package

| # | Document | Status | Who Signs | Action |
|---|---|---|---|---|
| 1 | [Offer Letter (Category, type)](path to ~/Downloads file) | ✅ Pre-filled | [PI Name] | Print on faculty letterhead, sign, send to Academic Appointments Manager for review |
| 2 | [Harvard University Participation Agreement](https://seas.harvard.edu/media/75051/download?attachment) | Download needed | [Recruit Name] | Sign electronically before appointment start date *(postdoc only)* |
| 2 | [Visitor Participation Agreement (type)](URL) | Download needed | [Recruit Name] | Sign and return PDF to Academic Appointments Manager *(non-postdoc only)* |
| 3 | [Acknowledgement of Risk and Release](URL) | Download needed | [Recruit Name] | Sign and return PDF to Academic Appointments Manager before arrival *(unpaid only)* |
| 4 | [English Language Proficiency Form](URL) | Download needed | [PI Name] | Check proficiency method(s), sign, return to departmental administrator *(international only)* |

Only include rows that apply to this case.

---

### Action Items for [PI Name]

- [ ] Print the offer letter on faculty letterhead, sign, and send to the Academic Appointments Manager for review
- [ ] Email the recruit's CV to the Academic Appointments Manager along with the offer letter
- [ ] Once cleared by the Academic Appointments Manager, send the offer letter to [Recruit Name] at [email]
- [ ] Forward the [Harvard University Participation Agreement / Visitor Participation Agreement] to [Recruit Name] to sign and return before arrival
- [ ] Forward the Acknowledgement of Risk and Release to [Recruit Name] to sign and return before arrival *(unpaid only)*
- [ ] Complete and sign the English Language Proficiency form, checking: [methods] — return to the departmental administrator *(international only)*
