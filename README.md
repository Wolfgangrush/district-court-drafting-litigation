# district-court-drafting

> **Open-source Claude-compatible plugin for drafting pleadings before District Courts across India — written for the District-court advocate who has never installed a plugin before.**
>
> Six-agent drafting pipeline · twelve case-type skills · State-aware design covering 16 State exemplars (covering all major District-court jurisdictions in India).
>
> Released under MIT. Open infrastructure for the legal community. No commercial engagement offered through this repository — see Disclaimer below.

---

## Table of contents

1. [What is a plugin? (ELI5 — read this first)](#what-is-a-plugin-eli5)
2. [What this plugin does](#what-this-plugin-does)
3. [State coverage — 16 State exemplars](#state-coverage)
4. [Case-type skills (full inventory with statutory authority)](#case-type-skills-full-inventory)
5. [The 6-agent drafting pipeline (what each agent does)](#the-6-agent-drafting-pipeline)
6. [Installation — step-by-step (no prior tech background assumed)](#installation--step-by-step)
7. [Your first plaint — full walkthrough](#your-first-plaint--full-walkthrough)
8. [The `state-config.md` file — how State customisation works](#the-state-configmd-file)
9. [Why MIT License (and not Apache 2.0, GPL, or anything else)](#why-mit-license)
10. [Sibling plugins (Wolfgang Rush legal-tech family)](#sibling-plugins)
11. [Why this exists](#why-this-exists)
12. [Roadmap](#roadmap)
13. [Contributing](#contributing)
14. [Contact](#contact)
15. [Author and brand](#author-and-brand)
16. [Provenance and privilege statement](#provenance-and-privilege-statement)
17. [Disclaimer and Bar Council of India Rule 36 compliance](#disclaimer-and-bar-council-of-india-rule-36-compliance)
18. [License](#license)

---

## What is a plugin? (ELI5)

If you have never used a Claude plugin before, read this section. If you are already familiar with Claude / ChatGPT / AI tools, skip to [What this plugin does](#what-this-plugin-does).

**Claude** is an AI assistant made by Anthropic. It can read text, write text, look at PDF files, and answer your questions. You can chat with Claude in a browser, in a desktop application, or in a terminal.

A **plugin** is a folder of instruction files that you place inside the Claude Desktop application's plugins folder on your computer. The plugin tells Claude how to do one specific job very well. Without a plugin, Claude is a general-purpose assistant. With a plugin installed, Claude becomes a specialist for the job that plugin describes.

This plugin — **district-court-drafting** — tells Claude how to draft a District-court pleading the way an Indian District-court advocate would draft it. The plugin contains:

- **Skills** — one file per case-type (suit for specific performance, plaint, bail application, etc.) that tells Claude what facts to ask for, what structure the pleading takes, what statutory provisions to cite, and what the final shape of the document should be.
- **Agents** — six small helpers that run in sequence. One reads your case folder, one formats the template, one writes the draft, one checks for fabrications, one polishes the language, one critiques the draft as opposing counsel.
- **State exemplars** — one file per State (Maharashtra, Karnataka, Tamil Nadu, etc.) that tells Claude about the Court-Fees Act, the Civil Manual, the Vakalatnama format, and the pecuniary jurisdiction limits of *your* State.

You install the plugin once. After that, whenever you open Claude inside a folder that contains your case documents, you can type a phrase like *"draft plaint"* or *"draft bail"* and Claude will produce a complete draft pleading in `.docx` form, which you can then open in Microsoft Word or LibreOffice, edit, verify, sign, and file.

**You do not need to know how to write code.** You only need to be able to copy a folder, edit a text file, and type a few words. The installation section below shows you every step.

---

## What this plugin does

This plugin produces complete District-court pleadings in `.docx` form, on the civil side and the criminal side, formatted in your State's idiom. The pipeline reads your case folder, asks you (through Claude) for any missing facts, drafts the pleading section-by-section, checks the draft against your facts, polishes the language, and finally critiques the draft as if opposing counsel were reading it.

Every pleading produced by this plugin has the following components, automatically:

- **Cause Title** in the District-Court convention (*IN THE COURT OF THE CIVIL JUDGE SENIOR DIVISION AT* / *IN THE COURT OF THE DISTRICT JUDGE AT* / *IN THE COURT OF THE METROPOLITAN MAGISTRATE AT* — your State's court designation taxonomy)
- **Statement of Facts** in chronological narrative form, with inline EXHIBIT markers (`EXHIBIT-A`, `EXHIBIT-B`, etc., or `Exhibit P/1`, etc., per your State's convention)
- **Particulars of Claim / Cause of Action** properly framed
- **Jurisdiction block** — territorial · pecuniary · subject-matter — citing the State Civil Courts Act + Sections 15-20 CPC where applicable
- **Limitation block** citing the specific Article of the Schedule to the Limitation Act 1963 (or the corresponding article in the BNSS / IPC / Specific Relief Act schedules)
- **Court-Fee block** citing the relevant State Court-Fees Act Schedule (Maharashtra Court Fees Act 1959 / Karnataka Court Fees and Suits Valuation Act 1958 / etc.)
- **Schedule of Property** (mandatory for immovable-property suits — auto-included if the case type requires it)
- **Prayer** with case-type-appropriate relief clauses
- **Verification** per Order VI Rule 15 CPC (verbatim — the wording the Code uses, not paraphrased)
- **Advocate's Signature Block** with Bar Council Enrolment Number placeholder
- **List of Documents / Exhibits**
- **Accompanying applications** — Temporary Injunction (Order 39 Rules 1 & 2 CPC) · Ad-Interim Injunction · Local Commission (Order 26 CPC) · Condonation of Delay (Section 5 Limitation Act) · etc., where the case type requires

The output is what an advocate would file before a District Court. **Not a template. Not a checklist. A pleading** — ready for the advocate's verification, signature, and filing.

---

## State coverage

District Courts vary materially across States — on the Court-Fees Schedule, on Civil Manual conventions, on Vakalatnama format, on pecuniary jurisdiction limits, on the Civil-Judge / District-Judge designation taxonomy, and on stamp-duty schedule. This plugin ships with **16 State exemplars** covering the full subcontinent:

| # | State / Region | State-config exemplar | Coverage notes |
|---|---|---|---|
| 1 | **Maharashtra** | `state-config/exemplars/maharashtra.md` | Most-deeply-validated State at v0.1.0-alpha (author's State). Bombay Court Fees Act 1959 + Maharashtra Civil Courts Act 1869 + Maharashtra Civil Manual. |
| 2 | **Karnataka** | `state-config/exemplars/karnataka.md` | Karnataka Court Fees and Suits Valuation Act 1958 + Karnataka Civil Courts Act 1964. |
| 3 | **Tamil Nadu** | `state-config/exemplars/tamil-nadu.md` | Tamil Nadu Court Fees and Suits Valuation Act 1955 + Tamil Nadu Civil Courts Act 1873. |
| 4 | **Kerala** | `state-config/exemplars/kerala.md` | Kerala Court Fees and Suits Valuation Act 1959 + Kerala Civil Courts Act 1957. |
| 5 | **Andhra Pradesh / Telangana** | `state-config/exemplars/andhra-pradesh-telangana.md` | Andhra Pradesh Court Fees and Suits Valuation Act 1956 (extends to both States post-bifurcation). |
| 6 | **Gujarat** | `state-config/exemplars/gujarat.md` | Bombay Court Fees Act 1959 (as extended to Gujarat) + Gujarat Civil Courts Act 2005. |
| 7 | **Delhi** | `state-config/exemplars/delhi.md` | Court Fees Act 1870 (central) + Delhi Civil Courts (Amendment) Acts. |
| 8 | **Uttar Pradesh / Uttarakhand** | `state-config/exemplars/uttar-pradesh-uttarakhand.md` | UP Court Fees Act 1870 + Civil Courts Act 1887. |
| 9 | **Madhya Pradesh / Chhattisgarh** | `state-config/exemplars/madhya-pradesh-chhattisgarh.md` | MP Court Fees Act 1870 + Civil Courts Act 1958 (extends to CG). |
| 10 | **West Bengal** | `state-config/exemplars/west-bengal.md` | Court Fees Act 1870 (West Bengal Amendment) + Bengal Civil Courts Act 1887. |
| 11 | **Bihar / Jharkhand** | `state-config/exemplars/bihar-jharkhand.md` | Bihar Court Fees Act 1870 + Civil Courts Act 1887 (extends to JH). |
| 12 | **Punjab / Haryana / Chandigarh** | `state-config/exemplars/punjab-haryana-chandigarh.md` | Court Fees (Punjab Amendment) Act + Punjab Courts Act 1918. |
| 13 | **Rajasthan** | `state-config/exemplars/rajasthan.md` | Rajasthan Court Fees and Suits Valuation Act 1961 + Rajasthan Civil Courts Ordinance 1950. |
| 14 | **Odisha** | `state-config/exemplars/odisha.md` | Court Fees Act 1870 (Odisha Amendment) + Bengal Civil Courts Act 1887 (extends to OD). |
| 15 | **North-East States** | `state-config/exemplars/north-east-states.md` | Assam / Arunachal / Meghalaya / Manipur / Mizoram / Nagaland / Tripura / Sikkim — combined exemplar with per-State overlays for the seven NE States + Sikkim. |
| 16 | **Jammu & Kashmir / Ladakh** | `state-config/exemplars/jammu-kashmir-ladakh.md` | J&K Court Fees Act 1977 + J&K Civil Courts Act 1976 (continues post-2019 reorganisation). |

**How a State exemplar works:** copy the one for your State into your case folder, rename it to `state-config.md`, verify the values (pecuniary limits move when the State amends them; check the latest), and the plugin renders your pleading in your State's idiom — with your State's Court-Fees Schedule citation, your State's Civil Manual reference, your State's Vakalatnama format.

---

## Case-type skills (full inventory)

The plugin ships **twelve case-type skills**, civil + criminal. Each is grounded in the statutory authority below.

### Civil-side skills

#### 1. `plaint-ws-draft` — Plaint and Written Statement

**Statutory authority:** CPC Order VI (general pleading rules) + Order VII (plaint) + Order VIII (written statement). **Use case:** the foundational civil-side pleading — the document that institutes a civil suit (plaint) or replies to one (WS). **Facts asked for:** parties + addresses, jurisdiction (territorial + pecuniary + subject-matter), cause of action with chronology, the wrong alleged + the loss suffered, the relief sought, court-fee value, limitation position, schedule of immovable property if any. **Output:** Plaint OR Written Statement (the skill handles both, per user instruction at invocation).

#### 2. `specific-relief-suit-draft` — Suit for Specific Performance / Declaration / Perpetual Injunction / Mandatory Injunction

**Statutory authority:** Specific Relief Act 1963 (as amended in 2018) + CPC Order VII. **Use case:** the four classical specific-relief actions — Specific Performance of contract, Declaration, Perpetual Injunction, Mandatory Injunction. **Facts asked for:** the contract / right being asserted + its proof, the defendant's breach / threatened breach, **the Readiness-and-Willingness averment for Specific Performance suits (Section 16(c) Specific Relief Act — without this averment the suit is liable to be dismissed)**, the alternative-remedy position (after the 2018 amendment, specific performance is the rule and not the exception, but the suit must still negative the bar in Section 14), the schedule of property where immovable property is involved, the court-fee + valuation framework under Section 7 of the Court-Fees Act. **Output:** complete suit, with the Section 16(c) Readiness-and-Willingness averment enforced by the Verifier.

#### 3. `petition-draft` — District Court Petitions

**Statutory authority:** CPC + the specific subject-matter statute (e.g. Section 27 Hindu Marriage Act before a District Judge exercising matrimonial jurisdiction; Guardian and Wards Act 1890 before a District Judge; Indian Succession Act 1925 before a District Judge in probate / letters of administration matters). **Use case:** non-suit petitions that the District Court entertains by virtue of the subject-matter statute. **Facts asked for:** the statute under which the petition is filed, the relief sought under that statute, the conditions precedent (e.g. one year of marriage for divorce under HMA Section 14, three years of stay together for divorce on cruelty under HMA Section 13(1)(ia), territorial-jurisdiction proof). **Output:** complete petition tailored to the subject-matter statute.

#### 4. `application-draft` — Interlocutory Application

**Statutory authority:** CPC + case-specific provisions. **Use case:** the wide universe of applications that arise within a pending suit — Temporary Injunction (Order 39 CPC), Attachment Before Judgment (Order 38 CPC), Receiver (Order 40 CPC), Amendment of Pleadings (Order 6 Rule 17), Production of Documents (Order 11), Interrogatories (Order 11), Local Commission (Order 26), Restoration of Suit (Order 9 Rule 9 / Rule 13), Condonation of Delay (Section 5 Limitation Act), etc. **Facts asked for:** the suit details, the application's specific Order + Rule, the grounds, the supporting case-law. **Output:** complete application with the prima-facie-case / balance-of-convenience / irreparable-injury triad framed (for injunction applications) and the supporting affidavit.

#### 5. `pleadings-draft` — Generic pleadings (civil + criminal mixed)

**Statutory authority:** CPC / BNSS as applicable. **Use case:** catch-all for pleadings that do not fall under the more specific skills above — Replication, Rejoinder, Surrejoinder, Note of Argument, written submissions. **Facts asked for:** the pleading being responded to, the position to be taken, the case-law in support. **Output:** complete pleading in the requested form.

### Criminal-side skills (BNSS-coded)

The plugin uses the **Bharatiya Nagarik Suraksha Sanhita 2023** (BNSS) section numbers — the corresponding CrPC 1973 sections are noted in brackets for the user's reference, because much existing case-law is keyed to the CrPC sections.

#### 6. `criminal-complaint-draft` — Private Criminal Complaint

**Statutory authority:** BNSS Section 223 (corresponding to CrPC Section 200). **Use case:** private complaint by the complainant directly to the Magistrate (as opposed to FIR + investigation by police). Common in cheque-bounce, defamation, criminal breach of trust, and trespass matters. **Facts asked for:** complainant + accused particulars, the offence alleged with section, the chronology of facts, the witnesses with their categories (oral / documentary), the documents relied upon. **Output:** complete criminal complaint + verification + list of witnesses + the supporting affidavit.

#### 7. `bail-draft` — Regular Bail Application

**Statutory authority:** BNSS Section 483 (corresponding to CrPC Section 439) — for non-bailable offences after arrest; BNSS Section 480 (corresponding to CrPC Section 437) — bail by Magistrate; bail under the substantive statute (NDPS Act / PMLA / UAPA, etc., where the conditions of those statutes apply). **Facts asked for:** FIR particulars, the offence and sentencing range, the period in custody, the *Sanjay Chandra v. CBI (2012) 1 SCC 40* parameters, medical / age / family / overcharging grounds, prior bail-rejection orders if any. **Output:** complete regular-bail application.

#### 8. `anticipatory-bail-draft` — Anticipatory Bail Application

**Statutory authority:** BNSS Section 482 (corresponding to CrPC Section 438). **Use case:** pre-arrest bail in anticipation of arrest on a non-bailable charge. **Facts asked for:** FIR particulars, the offence + sentencing range, applicant's antecedents, apprehension of arrest, the *Siddharam Satlingappa Mhetre v. State of Maharashtra (2011) 1 SCC 694* parameters. **Output:** complete anticipatory-bail application.

#### 9. `criminal-revision-draft` — Criminal Revision

**Statutory authority:** BNSS Sections 438 and 442 (corresponding to CrPC Sections 397 and 401). **Use case:** revision against interlocutory or final orders not amenable to appeal — discharge, framing of charge, summoning order, sentence revision. **Facts asked for:** the impugned order, the revisional limb invoked (jurisdiction / illegality / propriety), supporting case-law. **Output:** complete criminal revision.

#### 10. `bnss-313-statement-draft` — Statement of Accused

**Statutory authority:** BNSS Section 351 (corresponding to CrPC Section 313). **Use case:** the accused's reply to the questions put under Section 313 / Section 351 after the prosecution evidence closes — the accused's opportunity to explain the prosecution case in his own words. **Facts asked for:** the prosecution case in summary, the accused's defence position, the documentary defence (if any), the alibi (if pleaded). **Output:** draft statement for the accused, written in the first person, structured question-by-question against the prosecution's case.

### Shared infrastructure skills

- **`_drafting_common`** — anti-pollution rules, encoding standards, language conventions, AI-style-marker blacklist, District-Court AI-use risk constraints, common phrases.
- **`_district_pleading_base`** — universal District-Court pleading skeleton (cause title in the District-Court convention, parties block, statutory opening, Order VI Rule 15 verification, court-fee block, jurisdiction block, limitation block, schedule of property template, list of documents template) — reads State-specific values from the user's `state-config.md`.

---

## The 6-agent drafting pipeline

The plugin is built on the **Anthropic Agent SDK** convention — six markdown agent files (`agents/<name>/<name>.md`) with YAML frontmatter declaring `name`, `description`, and `allowed-tools`. Each agent is invoked in sequence on a case folder and reads/writes specific files in that folder.

### 1. `reader` — first agent

**What it does:** walks every file in your case folder — PDFs, DOCXs, scanned image of FIR, voice notes (transcribed), notes you typed — and extracts the facts in a structured form. It writes `case-facts.md` with a per-document audit log so you know which fact came from which document, and `annexure-candidates.md` mapping each document to a proposed EXHIBIT slot. If a statute PDF that the case-type skill requires is missing from the `laws/` folder, the Reader halts and asks you to supply it.

**Skills it loads:** `_drafting_common` + the specific case-type skill's `case-facts-questions.md`.

### 2. `format` — second agent

**What it does:** loads the case-type skill template (e.g. `specific-relief-suit-draft`), reads your `state-config.md`, and substitutes every State-specific value — Court-Fees Act citation, Civil Courts Act citation, Vakalatnama format, court designation (Civil Judge SD / DJ / etc.), stamp-duty schedule — into the template. Writes `format-shell.md` for the Drafter.

**Skills it loads:** `_district_pleading_base` + the case-type skill.

### 3. `drafter` — third agent

**What it does:** writes the actual pleading. Cause Title, parties block, Statement of Facts (chronological narrative form, not bullet-point), Particulars of Claim, Jurisdiction block, Limitation block, Court-Fee block, Schedule of Property (if needed), Prayer with case-type-appropriate relief clauses, Verification per Order VI Rule 15 (verbatim), Advocate's Signature Block, List of Documents. Outputs `draft-v1.md` and `draft-v1.docx`.

**Skills it loads:** `_drafting_common` (style + anti-pollution rules) + `_district_pleading_base` (skeleton) + the case-type skill.

### 4. `verifier` — fourth agent

**What it does:** anti-hallucination firewall. Compares every fact in the draft against `case-facts.md` line-by-line. Flags fabricated dates, mis-cited sections (statute section in draft doesn't match the law PDF), orphan EXHIBIT markers, unsupported assertions, and — most importantly for civil-side suits — **enforces the Section 16(c) Specific Relief Act Readiness-and-Willingness averment** in every Specific Performance suit. Writes `verification-report.md`.

**Skills it loads:** `_drafting_common` (verification ruleset).

### 5. `refiner` — fifth agent

**What it does:** applies Verifier flags, polishes the language to District-Court formal register, enforces State-specific formatting per your `state-config.md` (paper size, font, margins, page numbering), strips AI-style markers. Outputs `draft-v2.md` and `draft-v2.docx`.

**Skills it loads:** `_drafting_common` (style enforcement) + `_district_pleading_base` (formatting).

### 6. `overseer` — sixth and final agent

**What it does:** reads the polished draft with an opposing-counsel lens. Flags weak prayers, attackable defects, missing limbs of argument, contradictions between paragraphs, gaps in the Statement of Facts, jurisdictional vulnerabilities, limitation vulnerabilities. Writes `opposing-notes.md` and copies the polished draft to `final-draft.docx`.

**Skills it loads:** `_drafting_common` (opposing-counsel ruleset).

---

## Installation — step-by-step

This section assumes **no prior technical background**. If you have used a plugin before, skip to [Verifying the install](#verifying-the-install).

### Step 0 — what you need

- A computer running macOS, Windows, or Linux.
- **Claude Desktop application** — Anthropic's GUI application, download from <https://claude.ai/download>
- A working internet connection (one-time, for cloning the plugin)
- Either Microsoft Word or LibreOffice on your computer to open the `.docx` output

### Step 1 — install Claude Desktop

If you have not already installed the Claude Desktop application, download it from <https://claude.ai/download>. The installer walks you through Anthropic account login, plan selection, and basic setup. Once Claude is installed and you can chat with it, return here.

### Step 2 — find your plugins folder

The plugin needs to sit in a specific folder on your computer that Claude looks at on startup. The folder depends on your OS:

| OS | Path |
|---|---|
| **macOS** | `~/Library/Application Support/Claude/plugins/` |
| **Windows** | `%APPDATA%\Claude\plugins\` (typically `C:\Users\<yourname>\AppData\Roaming\Claude\plugins\`) |
| **Linux** | `~/.config/Claude/plugins/` |

You may need to create the folder the first time:

```bash
# macOS
mkdir -p ~/Library/Application\ Support/Claude/plugins

# Linux
mkdir -p ~/.config/Claude/plugins

# Windows (PowerShell)
mkdir -Force $env:APPDATA\Claude\plugins
```

### Step 3 — clone the plugin into that folder

Open Terminal (macOS / Linux) or PowerShell (Windows). Navigate to the plugin folder, then clone:

```bash
# macOS
cd ~/Library/Application\ Support/Claude/plugins
git clone https://github.com/Wolfgangrush/district-court-drafting-litigation.git district-court-drafting

# Linux
cd ~/.config/Claude/plugins
git clone https://github.com/Wolfgangrush/district-court-drafting-litigation.git district-court-drafting

# Windows (PowerShell)
cd $env:APPDATA\Claude\plugins
git clone https://github.com/Wolfgangrush/district-court-drafting-litigation.git district-court-drafting
```

If the `git clone` command fails because you don't have Git installed, install Git from <https://git-scm.com/downloads> and try again.

### Step 4 — restart Claude Desktop

Quit and reopen the Claude Desktop application. The plugin will be auto-discovered on the next session start.

### Verifying the install

In a Claude session, type any of the following:

- *"draft plaint"* — should trigger `plaint-ws-draft`
- *"draft specific relief suit"* — should trigger `specific-relief-suit-draft`
- *"draft bail"* — should trigger `bail-draft`
- `/specific-relief-suit-draft` — explicit slash-invocation

Claude should respond by reading the skill and asking you for the case folder path or the case-specific facts. If Claude does not recognise the trigger phrase, restart the Desktop application and confirm the plugin folder is at the correct path.

---

## Your first plaint — full walkthrough

Suppose you wish to draft a **Suit for Specific Performance of an Agreement to Sell immovable property** before the Civil Judge Senior Division at Nagpur. Here is the full flow.

### Step 1 — create a case folder on your computer

```
~/Desktop/cases/
└── spec-perf-plot-DDMMYYYY/
    ├── state-config.md           ← copied from state-config/exemplars/maharashtra.md
    ├── facts/
    │   ├── agreement-to-sell-DD.MM.YYYY.pdf
    │   ├── 7/12-extract-DD.MM.YYYY.pdf
    │   ├── notice-to-defendant-DD.MM.YYYY.pdf
    │   ├── defendant-reply-DD.MM.YYYY.pdf
    │   └── photographs-of-property/
    ├── laws/
    │   ├── specific-relief-act-1963.pdf
    │   ├── transfer-of-property-act-1882.pdf
    │   ├── limitation-act-1963.pdf
    │   └── maharashtra-court-fees-act-1959.pdf
    └── notes.md                   ← your free-form notes about the facts and strategy
```

### Step 2 — copy the State exemplar

From inside the plugin folder, copy the State exemplar for your State:

```bash
# macOS / Linux
cp ~/.claude/plugins/district-court-drafting/state-config/exemplars/maharashtra.md \
   ~/Desktop/cases/spec-perf-plot-DDMMYYYY/state-config.md
```

Open `state-config.md` in any text editor and verify the values. Pecuniary jurisdiction limits and Court-Fees Schedule entries change when the State amends them; confirm against your bar's latest notification before relying on the values.

### Step 3 — open the case folder in Claude

In the Claude Desktop application, point Claude at the case folder using the application's file-browser feature.

### Step 4 — invoke the skill

```
draft specific relief suit
```

The plugin will start the **Reader** agent. Reader will:

- Read your Agreement to Sell, your 7/12 extract, your notice, the defendant's reply, your photographs (if relevant)
- Write `case-facts.md` with the chronology — when the agreement was made, what the consideration was, what was paid as earnest money, what the timeline for execution was, when the breach occurred, what notice you served, what reply the defendant gave
- Write `annexure-candidates.md` mapping documents to EXHIBIT slots (`EXHIBIT-A` for the Agreement, `EXHIBIT-B` for the 7/12 extract, etc.)
- Halt and ask you for any missing law PDF

Open `case-facts.md`. **Verify every fact.** If Reader missed something or misread something, edit the file and save.

### Step 5 — continue the pipeline

**Format → Drafter → Verifier → Refiner → Overseer** run in sequence. The Drafter writes the suit with:

- Cause Title: *IN THE COURT OF THE CIVIL JUDGE SENIOR DIVISION AT NAGPUR* (per your `state-config.md`)
- Parties block in the Maharashtra District-Court convention
- Statement of Facts: chronological, narrative, with inline EXHIBIT markers
- **Readiness-and-Willingness averment per Section 16(c) Specific Relief Act 1963** (mandatory — without this averment, the suit is dismissible at threshold)
- Jurisdiction block: Section 16(d) CPC (immovable property situate within Nagpur Civil-Judge SD jurisdiction)
- Pecuniary jurisdiction: per Maharashtra Civil Courts Act 1869 + the relevant SD pecuniary limit
- Limitation: Article 54 of the Schedule to the Limitation Act 1963 (three years from the date of refusal of performance / the date fixed for performance)
- Court-Fee block: ad valorem on the consideration under Schedule I of the Maharashtra Court Fees Act 1959
- Schedule of Property: full description of the suit property with boundaries
- Prayer: decree for specific performance + consequential reliefs (delivery of possession, declaration of title, permanent injunction restraining alienation) + costs
- Verification: verbatim per Order VI Rule 15 CPC
- Advocate's Signature Block with Bar Council Enrolment Number placeholder

The Verifier then checks every fact against your `case-facts.md`, enforces the Section 16(c) averment, and flags any issue. The Refiner polishes. The Overseer writes `opposing-notes.md` from the defendant's perspective.

### Step 6 — review, sign, file

Open `final-draft.docx` in Microsoft Word or LibreOffice. Read every paragraph. Verify every citation. Verify the schedule of property. Verify the Court-Fee value. Sign. Submit to the Registry along with the prescribed Court Fee + Vakalatnama + List of Documents + Process Fee.

**You are responsible for the pleading. The plugin is responsible for the first draft.**

---

## The `state-config.md` file

The `state-config.md` is how the plugin knows *which State's District-Court conventions* to apply. A typical state-config has fields like:

```yaml
state: "Maharashtra"
court_designation_taxonomy:
  - "Civil Judge Junior Division"
  - "Civil Judge Senior Division"
  - "District Judge"
  - "Additional District Judge"
  - "Judicial Magistrate First Class"
  - "Chief Judicial Magistrate"
  - "Sessions Judge"
  - "Additional Sessions Judge"
court_fees_act: "Maharashtra Court Fees Act 1959 (Bombay Court Fees Act 1959 as adopted)"
civil_courts_act: "Maharashtra Civil Courts Act 1869"
civil_manual: "Maharashtra Civil Manual"
vakalatnama_format_reference: "Bombay High Court Appellate Side Rules / Maharashtra Civil Manual Appendix"
pecuniary_jurisdiction:
  civil_judge_jd: "up to Rs. 10 lakh"     # verify against latest State notification
  civil_judge_sd: "above Rs. 10 lakh"
  district_judge: "concurrent / appellate"
stamp_act: "Maharashtra Stamp Act 1958"
paper_size: "A4 / Legal as per local practice"
exhibit_marker_style: "EXHIBIT-A"
verification_clause_form: "Order VI Rule 15 CPC verbatim"
```

The Format agent reads this file and applies every value into the Drafter's output. If your State amends its Court Fees Act or revises the pecuniary jurisdiction limit, you edit the state-config — never the plugin source.

---

## Why MIT License

The plugin is released under the **MIT License**. This was a deliberate choice. The alternatives — and why they were rejected — are below.

### MIT vs the alternatives

| License | Suitable for this plugin? | Reasoning |
|---|---|---|
| **MIT** ✅ chosen | Yes | Permissive · 3-line attribution requirement · zero copyleft · zero patent-grant complexity · compatible with the Anthropic Plugin Marketplace TOS · compatible with adoption by District-court advocates, mid-market law firms, and legal-aid clinics · allows a future paid commercial layer to ship under a separate corporate entity without dual-license complexity. |
| **Apache 2.0** | Close second | Adds an explicit patent grant — but Indian procedural drafting content is non-patentable under Section 3(k) of the Patents Act 1970. The patent grant is dead weight; NOTICE-file ceremony adds friction. |
| **GPL-3.0** | ❌ Disqualifying | Copyleft would propagate to the advocate's case folder. The pleading — generated by the plugin from the advocate's case facts — could be argued to be a "derivative work" under GPL-3, forcing the pleading itself to be GPL-licensed. No advocate can ship privileged client material under any open-source licence. Hard structural blocker. |
| **AGPL-3.0** | ❌ Disqualifying | Same problem as GPL-3, plus the network-use clause triggers if anyone integrates the plugin into a SaaS legal-tech product. Blocks all commercial integration. |
| **LGPL-3.0** | ❌ Awkward | Designed for shared libraries, not for plugins that produce derivative documents. Library exception does not map cleanly to "the plugin produces a `.docx` that the advocate signs and files". |
| **BSD-3-Clause / BSD-2-Clause** | Functionally equivalent to MIT | Slightly different attribution wording; no practical advantage. MIT is more widely understood by non-developer audiences. |
| **Unlicense / CC0** | ❌ Forfeits authorship | Drops the copyright assertion. The author loses moral rights under Section 57 Copyright Act 1957 and the project loses derivative-misuse traceability. |
| **Creative Commons (any variant)** | ❌ Wrong instrument | CC licences are designed for creative content (text, images, video) and are not recommended for software. They lack software-grade warranty disclaimers and patent-grant clarity. |

### One-paragraph rationale

The plugin is released under MIT because District-court advocates and the mid-market law firms that handle the highest-volume civil and criminal docket in India must be able to clone, fork, adapt, and integrate this plugin alongside their privileged client material without the licence propagating into their case folders or attaching to the pleadings they file. Only MIT (and equivalently BSD and Apache 2.0) satisfies that constraint. MIT was preferred over Apache 2.0 because the patent-grant language Apache adds carries no practical benefit in the Indian context (procedural drafting content is unpatentable under Section 3(k) of the Patents Act 1970), and MIT's three-line clarity is friendlier to advocates who are not professional software developers and who will read the LICENSE file before adopting the plugin in chambers.

### Compatibility statement

MIT is compatible with:

- Apache License 2.0
- BSD 2-Clause and BSD 3-Clause
- GPL family (downstream-incorporation only)
- Anthropic Plugin Marketplace Terms of Service
- All major commercial-software integration policies

---

## Sibling plugins

This plugin is one in the **Wolfgang Rush** family of Indian legal-drafting plugins. All thirteen siblings ship under the same six-agent pipeline (Reader → Format → Drafter → Verifier → Refiner → Overseer) and the family-of-plugins doctrine — each plugin narrowly scoped to one practice area / forum:

| Plugin | GitHub repo | Scope |
|---|---|---|
| `supreme-court-drafting` | [supreme-court-drafting-litigation](https://github.com/Wolfgangrush/supreme-court-drafting-litigation) | SLPs · Writ Art 32 · Transfer · Review · Curative — Supreme Court of India |
| `indian-hc-drafting` | [indian-hc-drafting-litigation](https://github.com/Wolfgangrush/indian-hc-drafting-litigation) | Pleadings across all 25 Indian High Courts (bench-config-aware) |
| `district-court-drafting` (this) | [district-court-drafting-litigation](https://github.com/Wolfgangrush/district-court-drafting-litigation) | Plaints · WS · CPC applications · BNSS complaints across 25+ States (state-config) |
| `indian-family-drafting` | [indian-family-drafting-litigation](https://github.com/Wolfgangrush/indian-family-drafting-litigation) | HMA · SMA · IDA · matrimonial · custody · DV Act · maintenance · adoption |
| `indian-contracts-drafting` | [indian-contracts-drafting-litigation](https://github.com/Wolfgangrush/indian-contracts-drafting-litigation) | MSA · NDA · employment · lease · sale · GPA · SHA · will · loan · arbitration |
| `indian-banking-drafting` | [indian-banking-drafting-litigation](https://github.com/Wolfgangrush/indian-banking-drafting-litigation) | DRT · SARFAESI · NI Act 138 · IBC §7 / §95 · DRAT |
| `indian-labour-drafting` | [indian-labour-drafting-litigation](https://github.com/Wolfgangrush/indian-labour-drafting-litigation) | ID Act · POSH · PG · EPF · ESI · MW · IESO + State exemplars |
| `indian-property-drafting` | [indian-property-drafting-litigation](https://github.com/Wolfgangrush/indian-property-drafting-litigation) | Gift · Exchange · Release · Trust · Wakf · Easement · Partition · Settlement · Mortgage · TIR |
| `indian-company-drafting` | [indian-company-drafting](https://github.com/Wolfgangrush/indian-company-drafting) | NCLT (241/242 · 245 · 230-232 · 66 · 252 · 213) · NCLAT (421 + 61) · IBC §9 / §10 |
| `indian-tax-drafting` | [indian-tax-drafting](https://github.com/Wolfgangrush/indian-tax-drafting) | Form 35 CIT(A) · Form 36 ITAT · Form 10A · Sec 148A · 263/264 · 271/270A · 144C · 201 · 260A |
| `indian-consumer-drafting` | [indian-consumer-drafting](https://github.com/Wolfgangrush/indian-consumer-drafting) | District / State / NCDRC + medical-negligence + product liability |
| `indian-mact-drafting` | [indian-mact-drafting](https://github.com/Wolfgangrush/indian-mact-drafting) | MV Act 1988 (2019 amended) · Sarla Verma + Pranay Sethi · state-config |
| `indian-ip-drafting` | [indian-ip-drafting](https://github.com/Wolfgangrush/indian-ip-drafting) | Copyright · Trade Marks · Patents · Designs + HC IP Divisions (post-IPAB-abolition) + Anton Piller / John Doe |

Each plugin can be installed independently, each plugin's Rule 36 firewall is narrow and reviewable, each plugin's bench / forum discipline is depth-validated within its scope, and the user installs only what they need.

---

## Why this exists

**District Courts are the highest-volume forum in India** — roughly 90% of legal disputes in this country are heard, tried, and disposed of in District-court rolls, with comparatively fewer matters reaching the High Courts and a small fraction reaching the Supreme Court. The Code of Civil Procedure (for civil) and the Bharatiya Nagarik Suraksha Sanhita (for criminal) provide the procedural backbone, but the actual practice varies State-by-State on the Court-Fees Schedule, the Civil Manual conventions, the Vakalatnama format, and the pecuniary jurisdiction limits.

Generic AI tools do not understand CPC discipline (Order VI Rule 15 verification, Order VII Rule 14 schedule of documents, the Section 16(c) Specific Relief Act averment) or the State-specific overlay. They produce drafts that read superficially correct but fail Registry scrutiny on the small things — and at the District-court level, the Registry is unforgiving.

This plugin encodes the CPC + BNSS discipline directly, and lets the user supply the State-specific overlay through a single `state-config.md` file in the case folder.

The author's primary practice is at the Bombay High Court (Nagpur Bench); the deepest validation is therefore at the District-court tier within Maharashtra. Other States are supported via the State-config architecture; community contribution from advocates with regular District-court practice in each State will deepen per-State validation over time.

---

## Roadmap

- [x] **v0.1.0-alpha (current)** — `_drafting_common` + `_district_pleading_base` + civil-side and criminal-side skill scaffolds + 6-agent pipeline + 16 State exemplars
- [ ] **v0.1.x** — bug fixes, language-register polish, State-config refinements driven by user feedback
- [ ] **v0.x onward** — civil-side and criminal-side case-type coverage deepening, State-specific Court-Fees Schedule + Civil Manual + Vakalatnama-format refinements, Commercial-Court extension (Commercial Courts Act 2015), and additional case-type skills as the community contributes
- [ ] **v1.0.0** — Stable release after community-validated use across multiple States

Per-State deep validation will arrive in the order advocates contribute. The plugin's State-config architecture means any advocate with regular District-court practice in a given State can deepen the calibration for that State by opening an issue or pull request with their State's idiom — no central roadmap is needed to enable that.

---

## Contributing

Advocates with regular District-court practice in any State are invited to contribute:

- State-specific Court-Fees Schedule references (with confirmation of the latest pecuniary limits)
- Civil Manual conventions specific to your State
- Vakalatnama format calibrations
- BNSS-coded criminal-side conventions specific to your jurisdiction
- Pecuniary-jurisdiction-limit confirmations (these change when the State notifies)
- Edge-case skill suggestions (Commercial Court matters, special legislation matters, etc.)

Open a GitHub issue with your State + the contribution. Pull requests are welcome with a one-paragraph explanation of the change and a reference to the relevant State Act, Civil Manual, or High Court Practice Direction.

This plugin is open source under MIT.

---

## Contact

All inquiries and feedback via **GitHub Issues** on the project repository.

This project does not have an email contact channel and **does not accept private legal-services inquiries through this repository**. No commercial engagement is offered through this plugin or its repository.

*(Future releases may introduce a commercial layer published under a separate corporate entity — at that point this section will be updated. v0.x.x is open-source-only infrastructure with no commercial channel.)*

---

## Author and brand

This plugin is authored by **Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa, practising before the Bombay High Court (Nagpur Bench).

The plugin is published under the **Wolfgang Rush** open-source brand — the author's publishing handle for legal-technology infrastructure. All commits to this repository are signed under the Wolfgang Rush GitHub identity. The real-identity declaration appears here, and again in `NOTICE.md`, so that the Bar Council Rule 36 accountability mechanism (advocate-as-individual responsibility) is preserved transparently rather than displaced by the publishing handle.

---

## Provenance and privilege statement

See [`NOTICE.md`](./NOTICE.md) for the full provenance and privilege statement.

**In brief:** this plugin contains only public procedural knowledge — CPC + BNSS conventions, substantive statutory references, State Court-Fees Act references, generic placeholders. It does **not** contain any specific client matter, communication, document, or personal data.

---

## Compliance posture — Supreme Court e-Committee AI framework

This plugin is **assistive drafting infrastructure**, not autonomous decision-making software. Its operational posture is aligned with the Supreme Court of India e-Committee's stated position on AI in legal work.

> *"AI and digital tools must be used as supportive instruments and should not be allowed to override judicial reasoning."*
>
> — **Justice Rajesh Bindal**, Judge, Supreme Court of India
> [*Judicial Process Re-engineering and Digital Transformation*](https://www.sci.gov.in/press-release-dated-april-12-2026/) conference, 11–12 April 2026
> Organised by the Supreme Court e-Committee in collaboration with the Department of Justice, Government of India.
> ([Coverage — Law Trend](https://lawtrend.in/ai-must-not-replace-judicial-reasoning-warns-supreme-court-justice-rajesh-bindal/))

The same posture underpins the Supreme Court's own AI infrastructure for the judiciary:

- **[SUPACE](https://www.drishtiias.com/daily-news-analysis/ai-portal-supace)** — *Supreme Court Portal for Assistance in Court Efficiency.* AI-enabled assistive tool launched on 6 April 2021 by then-CJI S.A. Bobde. Provides legal research, fact extraction, document review, and drafting assistance to judges and legal researchers. **By design, SUPACE is not a decision-making system** — it processes facts and surfaces them to the human user. The Supreme Court has recommended adoption across all Indian High Courts.

- **[SUVAS](https://www.drishtijudiciary.com/current-affairs/supreme-court-vidhik-anuvaad-software-suvas)** — *Supreme Court Vidhik Anuvaad Software.* AI-powered translation tool launched in November 2019 by then-CJI S.A. Bobde. Translates judicial documents, orders, and judgments between English and ten Indian regional languages.

### What this plugin does — and does not — do under that framework

**Does:**

- Generate structural skeletons of pleadings, drawing on public statutes, schedule forms, and court rules.
- Run a six-agent assistive pipeline (Reader → Formatter → Drafter → Verifier → Refiner → Overseer) over the user's case facts.
- Surface citations, procedural anchors, and bench-specific conventions for advocate review.

**Does NOT:**

- Generate final filings autonomously.
- Substitute for advocate professional judgment.
- Replace human verification.
- Operate without an enrolled advocate retaining full professional responsibility.

**Every draft produced through this plugin must be advocate-owned and human-verified before filing.** The enrolled advocate using this plugin retains full professional responsibility under the Advocates Act 1961 and the Bar Council of India Rules, including verification of facts, accuracy of citations, correctness of legal grounds, propriety of the prayer, and signature on every pleading filed.

This is the same standard the Supreme Court itself applies to its own AI infrastructure (SUPACE / SUVAS): **AI as supportive instrument, never as decision-maker.**

---

## Disclaimer and Bar Council of India Rule 36 compliance

This plugin is **open-source infrastructure released free of cost** under the MIT licence.

**This plugin:**

- Does **not** constitute legal advice.
- Does **not** create an advocate-client relationship between the author and any user.
- Does **not** solicit professional work from any user, group, or audience.
- Does **not** advertise the professional services of the author or any advocate.
- Does **not** offer paid legal services, paid consultations, or commercial legal engagements through this repository or any associated channel.

**It is:**

- A drafting aid for use by **enrolled advocates** who retain full professional responsibility for every pleading produced.
- A reference implementation of open-source legal-tech infrastructure for District-Court practice in India.
- Released under the Bar Council of India Rules, Part VI, Chapter II, Section IV, **Rule 36** (Conduct and Etiquette — restriction on advertising and solicitation), to which the author and every Indian advocate is bound.

**Every advocate using this plugin is reminded:** the advocate retains full professional responsibility for the verification of facts, the accuracy of citations, the correctness of legal grounds, the propriety of the prayer, and the signature on every pleading filed. AI-generated drafting output is **starting material, not a finished pleading**.

---

## License

**MIT.** See [`LICENSE`](./LICENSE) for the full text.

Copyright (c) 2026 Wolfgang Rush. Authored by Rushikesh R. Mahajan, Advocate, publishing under the Wolfgang Rush open-source brand.
