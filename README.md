# Goddard Riverside FY2025 Year-End Program Dashboard
### Data Manager Candidate Assessment — README
**Prepared by:** [Your Name]
**Date:** May 2026

---

## Overview

Goddard Riverside Community Center collects end-of-year program data to answer three core leadership questions:

1. **What are programs producing?** — outputs vs. targets
2. **Who are they serving?** — participant demographics
3. **Are they operationally healthy?** — staffing capacity

This project delivers an end-to-end data workflow that collects, stores, cleans, visualizes, and presents that data in a unified, interactive dashboard. Three program submissions were used as the dataset: Corner House, ACT Team, and #DegreesNYC (Capitol Hall).

---

## Architecture & Workflow

```
Jotform Survey
      ↓
Google Sheets (Raw Data Tab)
      ↓
Google Sheets (Cleaned Data Tab)
      ↓
Looker Studio Dashboard
      ↓
HTML/CSS Front-End Presentation
```

### Step 1 — Jotform Survey
A condensed 10-section survey was designed in Jotform to capture the essential structure of Goddard Riverside's FY2025 Year-End Program Survey. The full survey was not replicated in its entirety due to Jotform's free-tier 100-row limit. Instead, a focused form was built that captures the three categories leadership needs most: program identification, staffing capacity, and participant demographics.

**Survey Sections:**
- Section 1: Program Identification
- Section 2: Staffing Capacity
- Section 3: Participants Served
- Section 4: Age Distribution
- Section 5: Income Brackets
- Section 6: Race & Gender
- Section 7: Health & Disability
- Section 8: Key Program Outputs
- Section 9: Targets vs. Actuals
- Section 10: Survey Feedback

**Live Form:** https://form.jotform.com/261354976898075

### Step 2 — Google Sheets: Raw Data
Jotform was integrated directly with Google Sheets via the native Jotform → Google Sheets integration. Each form submission automatically populates a new row in the Raw Data tab with no manual entry required. Column headers are generated automatically from form field labels.

Three submissions were entered through the live Jotform form using data from the three PDF survey responses provided. Made-up names and emails were used where real personal data was not appropriate for a demo environment. All numerical values reflect the exact figures reported in the original PDF submissions.

**Raw Data tab contains:**
- Unmodified form responses exactly as submitted
- No rounding, no calculations, no transformations
- One row per program submission

### Step 3 — Google Sheets: Cleaned Data
The Cleaned Data tab references the Raw Data tab using formulas. No data is manually entered in the Cleaned Data tab — all values are either pulled directly from Raw Data or calculated.

**Cleaning transformations applied:**

| Field | Cleaning Rule | Reason |
|---|---|---|
| Full-Time Staff | ROUNDUP to whole number | ACT Team reported 9.5 FTE (shared position); rounded up for headcount reporting |
| Program Director Name | First + Last combined with & | Raw data stores names in two columns; combined for readability |
| Full Address | TEXTJOIN of Street, City, State, Zip | Combined four address fields into one readable string |
| Total FTE | Full-Time + (Part-Time × 0.5) | Standardizes staffing capacity across programs |
| Participants per FTE | Total Participants ÷ Total FTE | Measures staff workload for operational comparison |
| Target % Achieved | (Actual ÷ Goal) × 100 | Enables quick performance assessment |
| Age Total Check | IF sum = total participants | Flags demographic mismatches automatically |
| Race Total Check | IF sum = total participants | Flags demographic mismatches automatically |
| Gender Total Check | IF sum = total participants | Flags demographic mismatches automatically |
| Data Quality Flags | Concatenated IF statements | Auto-generates plain English flag descriptions |

**Conditional formatting:** MISMATCH cells are highlighted red. Data Quality Flag cells containing issues are highlighted yellow.

### Step 4 — Looker Studio Dashboard
The Cleaned Data tab was connected to Looker Studio as the sole data source. The dashboard includes six visualizations:

| Chart | Type | Purpose |
|---|---|---|
| Participation by Program | Table | Shows total participants per program |
| Staffing Capacity | Stacked Bar | Compares full-time, part-time, and vacant positions |
| Participants per FTE | Bar Chart | Identifies staff workload disparities |
| Race/Ethnicity Breakdown | 100% Stacked Bar | Compares demographic composition across programs |
| Age Distribution | Stacked Bar | Shows age group breakdown per program |
| FY25 Targets vs. Actuals | Table | Displays target names, goals, actuals, and % achieved |

Two filters were added at the page level — Program Name and Cause Area — allowing leadership to slice all charts simultaneously.

### Step 5 — HTML/CSS Front-End
A styled single-page HTML/CSS presentation wraps the Looker dashboard embed and provides additional context for leadership. The page includes:
- Sticky navigation header
- Hero section with agency-wide summary statistics
- About section explaining the three core questions
- Key highlights pulled from the data
- Embedded Looker Studio iframe
- Program profile cards for each of the three programs
- Data quality flags section
- Footer documenting the full data pipeline

---

## Data Cleaning Decisions

### Decision 1: Condensed Age Brackets
The original survey used 11 age bands (0-5, 6-10, 11-15, etc.). The condensed form collapsed these into 5 broader brackets (Under 20, 21-40, 41-60, 61-84, 85+) to stay within the Jotform row limit while preserving the most analytically useful age groupings for a population that skews older.

### Decision 2: ROUNDUP for Fractional FTE
ACT Team reported 9.5 full-time staff members, indicating one position is shared between programs at 50%. ROUNDUP was used rather than ROUND because a shared employee still represents a real person providing services. The original 9.5 value is preserved in the Raw Data tab.

### Decision 3: Income Not Collected Flag
ACT Team does not collect participant income data. Rather than leaving income fields blank, the full participant count (94) was entered under "Income Data Not Collected" to ensure the income total check formula returns OK and the absence of data is explicitly documented rather than ambiguous.

### Decision 4: Participants per FTE Formula
Part-time staff were weighted at 0.5 FTE rather than 1.0 to reflect standard nonprofit staffing conventions. This produced materially different workload figures — particularly for ACT Team, which has 4 part-time staff — and gives leadership a more accurate picture of actual capacity.

---

## Design Rationale

### Form Design
The condensed Jotform survey was designed around the three leadership questions rather than trying to replicate the full 20-page PDF survey. Fields were selected based on what is most useful for cross-program comparison in a dashboard context. Survey feedback (Section 10) was included deliberately to measure respondent confidence in the tool and support iterative improvement.

### Dashboard Design
Charts were chosen to maximize cross-program comparability:
- 100% stacked bars for demographics allow fair comparison regardless of program size
- The targets vs. actuals table was sorted by program name for consistent navigation
- Participants per FTE was highlighted separately because it tells the most important operational story

### Front-End Design
The HTML/CSS page uses a dark navy color scheme with teal accents to convey professionalism and clarity. The design follows a clear information hierarchy: summary statistics first, context second, interactive dashboard third, program details fourth, data quality last. This mirrors how leadership would naturally want to consume the information — starting with the big picture and drilling down.

---

## Assumptions & Limitations

| Item | Detail |
|---|---|
| Sample size | Only 3 of Goddard Riverside's full program portfolio are included. Findings are illustrative, not representative of the full agency. |
| Made-up names | Program director names and emails used in the demo are fictional. Numerical data reflects exact PDF values. |
| Jotform row limit | The free tier limits forms to 100 rows. The condensed survey was designed with this constraint in mind. A paid Jotform account would support the full survey. |
| Looker data refresh | Looker Studio pulls from Google Sheets on a scheduled refresh. Changes to the Cleaned Data tab may not appear in Looker immediately. |
| FTE calculation | Part-time staff are assumed to work 50% of full-time hours. Actual hours may vary by position. |
| Corner House disability flag | 36 participants reported with any disability against 31 total participants. This was flagged but not corrected in the Cleaned Data tab as the counting methodology is unclear. |
| Income data | ACT Team does not collect income data. This limits the usefulness of income-based analysis at the agency level. |
| Historical comparison | This dashboard covers FY2025 only. Year-over-year trend analysis would require data from prior survey cycles. |

---

## Deliverables

| Deliverable | Link / File |
|---|---|
| Jotform Survey | https://form.jotform.com/261354976898075 |
| Google Sheet | [Link to your Google Sheet] |
| Looker Dashboard | https://datastudio.google.com/reporting/6d3ce0a5-e16d-44c0-85af-92a52f255ec1 |
| HTML/CSS File | goddard_dashboard.html |
| README | README.md (this document) |

---

## Tools Used

- **Jotform** — Survey design and data collection
- **Google Sheets** — Data storage, cleaning, and transformation
- **Looker Studio** — Data visualization and interactive dashboard
- **HTML/CSS** — Front-end presentation layer
- **Google Fonts** — Typography (DM Serif Display, DM Sans)
