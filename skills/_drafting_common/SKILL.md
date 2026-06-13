---
name: _drafting_common
description: Shared reference for all District Court drafting skills in this plugin. Holds the enforcement rules, architecture constraints, District Court AI-use risk context, and standard District Court formatting conventions. NOT invoked directly — referenced from every drafting skill via ${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md.
allowed-tools: []
---

# Shared Drafting Common — Rules & Constraints (District Courts of India)

This file is referenced from every drafting skill in the `district-court-drafting` plugin. It is NOT invoked on its own.

## THE ENFORCEMENT RULES (non-negotiable)

1. **No external-memory MCP tools in drafting skills.** Drafting skill `allowed-tools:` lists MUST exclude any external-memory, vault, or cloud-sync MCP tool. A drafting skill operates strictly on the case folder.

2. **Case-folder scoping.** Every drafting skill auto-fires only when the user is working inside a case folder. No hardcoded case path in the plugin.

3. **Per-case CLAUDE.md is OPT-IN, self-contained.** Case context only — parties, court (the specific District Judge / Civil Judge Senior Division / Junior Division / Additional District Judge / Civil Court of Small Causes / etc.), key dates, advocate name and Bar Council enrolment number, court-fee paid status.

4. **Drafting skills do not interact with any general note-taking / archive skill.** Out of scope; hygiene recommendation only.

5. **External diary / project tracker** should reference cases by code or pattern only — never by quoting draft text or party names verbatim.

6. **🔴 LAYER 2 IS APPEND-ONLY FROM SKILL/AGENT SIDE.**
   - Skills/agents may: READ existing files in case folder, WRITE new draft files, EDIT files they themselves created in this session
   - Skills/agents may NOT: rm, mv, rmdir, trash, rename, delete, overwrite-without-versioning
   - Enforcement recommendations: `allowed-tools:` whitelist excludes destructive Bash patterns; user-side hooks block destructive operations on case-folder paths

6.1. **🔴 WORKING-COPY RULE — never operate on originals.**
   Conversion / extraction / transformation of any existing case file is performed only on a copy under `~/.claude/working-copies/<case-name>/`. The original file is never opened-for-write.

## LOCKED CONSTRAINTS (District Court AI-use risk)

> Indian Courts have publicly cautioned advocates against AI-generated content with fabricated citations or unverified facts. District-Court matters are the highest-volume practice surface; a single fabricated FIR number, fabricated property survey number, fabricated date, or fabricated citation can be disastrous for the underlying matter.

These rules apply to every drafting agent:

- **NO internet for facts.** No WebSearch, no WebFetch for case content or for citations.
- **Statutes available from training data:** Constitution of India, Code of Civil Procedure 1908, Specific Relief Act 1963, Indian Contract Act 1872, Transfer of Property Act 1882, Indian Succession Act 1925, Indian Penal Code 1860 / Bharatiya Nyaya Sanhita 2023, Code of Criminal Procedure 1973 / Bharatiya Nagarik Suraksha Sanhita 2023, Indian Evidence Act 1872 / Bharatiya Sakshya Adhiniyam 2023, Limitation Act 1963, Court-Fees Act 1870, Suits Valuation Act 1887. **All other statutes** (e.g., Hindu Marriage Act, Hindu Succession Act, Muslim Personal Law, Christian Marriage Act, Special Marriage Act, Family Courts Act, POCSO, NDPS, IT Act, Arbitration and Conciliation Act, Negotiable Instruments Act for Section 138 specifically, Consumer Protection Act, MV Act, GST Acts, Income Tax Act, RERA, IBC, and so on) must be supplied by the user as PDF before they can be cited. The Reader agent halts the pipeline if any required statute is missing.

- **State-specific instruments are user-supplied.** Each State has its own Court-Fees Schedule (often amended), its own Civil Manual, its own Criminal Manual, its own Vakalatnama format, and its own Court Rules made under Section 122 / Section 482 of the relevant Code. These are NOT in training data; the user supplies the State-specific PDFs.

- **Case citations — strict.** Every case citation in the output must trace to a citation supplied in the user's case folder (`citations.md`). The Drafter never generates a case name + citation pair from memory. Where a ground requires support and no user-supplied citation matches, the Drafter writes a `[CITATION NEEDED: <legal proposition>]` placeholder and the Verifier flags it.

- **File-based pipeline.** Each agent writes its output to a file → next agent reads. Auditable chain of custody.

- **Triple-verify.** Verifier + Refiner + Overseer = 3 independent passes before the draft is shown to the user.

- **Final draft must be indistinguishable from an advocate-authored pleading.** No AI-style markers, no bullet-list dumps where prose is expected, no markdown formatting in the .docx body, no first-person AI framing.

- **The user remains the responsible advocate.** The plugin is a drafting aid; every draft must be reviewed before filing. The advocate's signature on the pleading is the advocate's responsibility.

## DISTRICT COURT FORMATTING CONVENTIONS

These conventions reflect the standard practice of District Courts across India as governed by the CPC (civil) and BNSS (criminal), supplemented by State-specific Civil Manuals and Criminal Manuals. No template in this plugin reflects any specific client matter; all conventions are public procedural knowledge.

### Civil-side conventions (CPC-governed)

- **Court header:** `IN THE COURT OF [JUDICIAL OFFICER DESIGNATION per state-config.md Section 2] AT [PLACE], DISTRICT [DISTRICT NAME]` — the JUDICIAL OFFICER DESIGNATION value varies by State and is sourced from the user's `<case-folder>/state-config.md`. Examples by State: `THE CIVIL JUDGE, SENIOR DIVISION` (Maharashtra/Karnataka/UP/Bengal-style); `THE SUBORDINATE JUDGE` (Tamil Nadu); `THE SUB COURT` (Kerala); `THE SENIOR CIVIL JUDGE` (AP/TG/Rajasthan/Delhi); `THE SUB JUDGE` (Bihar/Jharkhand/Punjab/Haryana); `THE CIVIL JUDGE CLASS I` (MP/Chhattisgarh). See `${CLAUDE_PLUGIN_ROOT}/state-config/exemplars/` for the validated per-State court designation taxonomy.
- **Case number line:** `[SUIT / REGULAR CIVIL SUIT / SPECIAL CIVIL SUIT / MISCELLANEOUS CIVIL APPLICATION] NO. _____ OF [YEAR]`
- **Parties separator:** `...VERSUS...` (preferred) or `///VERSUS///` (acceptable in some State practices)
- **Section heads:** Title Case bold (e.g., `Statement of Facts`, `Particulars of Claim`, `Cause of Action`, `Limitation`, `Court-Fee`, `Jurisdiction`, `Verification`)
- **Salutation opener:** `The Plaintiff above-named most humbly and respectfully begs to submit as under:`
- **Inline annexure marker (District):** `...a copy whereof is filed herewith and marked as EXHIBIT [A / B / C ...]`
  *(Note: Some States use the "ANNEXURE-A" convention; user-controlled via `format-from-user.md`. Default is `EXHIBIT [letter]`.)*
- **Schedule of Property block:** mandatory for any suit involving immovable property — placed at the end of the plaint with boundary description (N / S / E / W), survey number, area, sub-division, mutation entries (where applicable).
- **Court-Fee block:** mandatory — states the value of suit, the court-fee paid, the schedule of the State Court-Fees Act under which the fee is paid.
- **Limitation block:** mandatory — states the cause of action date(s) and the applicable Article of the Limitation Act 1963 Schedule.
- **Jurisdiction block:** mandatory — states the territorial and pecuniary basis for the chosen forum.
- **Verification block:** verbatim per CPC Order VI Rule 15, with separate sub-blocks for paragraphs based on personal knowledge and paragraphs based on information believed to be true.
- **Advocate's signature block:**
  ```
  PLACE: [Place]                              ([NAME], Advocate)
  DATE: <DD.MM.YYYY>                          [Bar Council Enrolment No.]
                                              ADVOCATE FOR THE PLAINTIFF
  ```
- **Vakalatnama (separate document):** mandatory · State-specific format · stamped per State Stamp Schedule.

### Criminal-side conventions (BNSS-governed, shipping v0.2.0)

- **Court header:** `IN THE COURT OF [JUDICIAL OFFICER DESIGNATION] AT [PLACE]` (e.g., `IN THE COURT OF THE JUDICIAL MAGISTRATE FIRST CLASS AT [PLACE]`)
- **Case number line:** `[CRIMINAL CASE / COMPLAINT CASE / R.C.C. / S.C.C.] NO. _____ OF [YEAR]`
- **Dual citation discipline:** every reference to CrPC must be paired with BNSS, every IPC reference paired with BNS, every IEA reference paired with BSA. Pattern:
  `Section [N] of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Section [N] of the Code of Criminal Procedure 1973 — applicable where the offence is registered before 1 July 2024)`

## ANNEXURE MECHANISM (District convention)

Inline marker in Facts → consolidated table at end of pleading. Default convention is `EXHIBIT [letter]` (overridable via `format-from-user.md` to `ANNEXURE-[letter]` for users from States that prefer that style).

```
STATEMENT OF FACTS
- "...the Agreement to Sell dated <DD.MM.YYYY> ... a copy whereof is filed herewith and marked as EXHIBIT A."
- "...the legal notice dated <DD.MM.YYYY> ... EXHIBIT B."
- "...the reply received ... EXHIBIT C."

LIST OF DOCUMENTS / EXHIBITS
| Sr.No | Exhibit | Particulars                            | Date            | Pages |
| 1     | A       | Agreement to Sell dated <DD.MM.YYYY>   | <DD.MM.YYYY>    | <N>   |
| 2     | B       | Legal Notice                            | <DD.MM.YYYY>    | <N>   |
| 3     | C       | Reply to Legal Notice                   | <DD.MM.YYYY>    | <N>   |
```

Drafter MUST keep inline markers and List of Documents in sync. Verifier checks for orphan markers.

## STANDARD PLEADING SECTIONS (in order, all in one .docx)

For every District Court civil pleading, the .docx contains (in this order):

1. Cause Title
2. Parties block (with addresses, ages, occupations — placeholders only)
3. Statement of Facts
4. Particulars of Claim / Cause of Action
5. Jurisdiction
6. Limitation (with the specific Article of Schedule to the Limitation Act 1963)
7. Court-Fee (with the specific Schedule of the State Court-Fees Act)
8. Prayer
9. Verification (per CPC Order VI Rule 15)
10. Place + Date + Advocate signature block + Bar Council Enrolment No.
11. Schedule of Property (where the suit involves immovable property)
12. List of Documents / Exhibits

For District Court criminal pleadings, the structure adjusts per the case-type (Bail Application, Anticipatory Bail Application, Criminal Complaint, Revision, etc.) — see case-type SKILL.md for each.

## OUTPUT FORMAT

- **File type:** `.docx` (so the user can open in Word and apply tracked-changes review)
- **Conversion tool:** pandoc with a `reference.docx` template; fallback to python-docx
- **Output filename convention:** `<case-type>_draft-v<N>_<YYYY-MM-DD>.docx`
- **NEVER overwrite an existing draft.** Each run produces a new versioned file.

## RECOVERY / AUDIT

The audit chain: `case-facts.md` → `format-shell.md` → `draft-v1.docx` → `verification-report.md` → `draft-v2.docx` → `opposing-notes.md` → `final-draft.docx`. Every file timestamped. Every input listed in `case-facts.md` Section 1. Every fact in the final draft traces to the case folder.


---

## v0.2.1 RENDER DISCIPLINE (load-bearing — Drafter must follow)

**Pandoc + reference.docx + post-pandoc fix script.** The Drafter writes Markdown using the heading discipline below. Pandoc converts the Markdown to `.docx` using the SHIPPED reference.docx at `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/reference.docx` — pre-customised with locked Word styles matching the filing-grade Bombay HC convention (extracted from an actual filed Second Appeal pleading):

- **Body (Normal)** — TNR 14pt, 1.5 line spacing, justified, 0.5cm first-line indent
- **Heading 1** — TNR 14pt **bold + centered** (NOT underlined) — for the Court / Forum / Tribunal header line and the case-number line
- **Heading 2** — TNR 14pt **bold + UNDERLINED + centered + letter-spacing** — for spaced section headers (`F A C T S`, `G R O U N D S`, `P R A Y E R`, `I N D E X`, `S Y N O P S I S`, `L I S T   O F   A N N E X U R E S`, `V E R I F I C A T I O N`)
- **Heading 3** — TNR 14pt **bold + UNDERLINED + centered** — for unspaced section headers (`SUBSTANTIAL QUESTIONS OF LAW`, `ACTS & RULES`, `CITATIONS`) and statutory opening (`WRIT PETITION UNDER ARTICLE 226 …`)
- **Heading 4** — TNR 14pt **bold + UNDERLINED + left-aligned** — for left-anchored bold-underlined headings (`MOST RESPECTFULLY SHEWETH:`)
- **Tables** — `tblLayout = fixed`; first row bold centered; cell margins locked

### Markdown heading mapping

| Markdown | Rendered as | Used for |
|---|---|---|
| `# Heading 1` | Bold centered (no underline) | Court header line; case-number line; cover-page anchors |
| `## Heading 2` | Bold centered UNDERLINED with letter-spacing | Spaced section headers (`## F A C T S`, `## G R O U N D S`, `## P R A Y E R`, `## I N D E X`, `## S Y N O P S I S`, `## L I S T   O F   A N N E X U R E S`, `## V E R I F I C A T I O N`) |
| `### Heading 3` | Bold centered UNDERLINED | Unspaced section headers + statutory opening |
| `#### Heading 4` | Bold left UNDERLINED | `#### MOST RESPECTFULLY SHEWETH:` |
| Body paragraph | TNR 14pt justified 1.5 spacing 0.5cm first-line indent | Everything else |
| `**Bold inline**` | Bold | Property descriptors / annexure markers / key terms inline within Facts narrative |

### Bold-number paragraph convention

Facts and Grounds paragraphs use **BOLD NUMBERS**: `**1.**`, `**2.**`, `**3.**` followed by a tab + body. Renders as the gold-standard pleading layout.

### Two-step pandoc command (Step 2 is NON-NEGOTIABLE)

```bash
# Step 1 — pandoc → .docx with locked Word styles
pandoc draft-v1.md -o draft-v1.docx \
  --reference-doc="${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/reference.docx" \
  --from=markdown+pipe_tables+raw_tex

# Step 2 — force table column widths
python3 "${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/fix_docx_tables.py" draft-v1.docx
```

Step 2 forces column widths on every table — 5-col (Sr.No / Annx / Particulars / Date / Pgs) = 8/8/60/14/10; 4-col = 10/10/65/15; 3-col = 10/75/15; 2-col Dates–Events = 18/82. Locks first-row bold + centered + vertically-centered cells. **Skipping the fix script reproduces the v0.2.0 Index-table defect (Sr.No / Annx columns stacking vertically).**

Do NOT auto-generate a fresh reference.docx in the case folder. Use the shipped one or a `<case-folder>/reference.docx` override.

### Cover-page discipline

INDEX, SYNOPSIS, LIST OF ANNEXURES each begin on a new page (`\newpage` in Markdown) and carry ONLY: forum header (`#`) + case-number line (`#`) + short cause-title (Petitioner short name `///VERSUS///` Respondent short name) + section header (`##`) + table + Counsel block. DO NOT repeat the full party address block on cover pages.

### Pipeline-optionality (advocate-cost discipline)

The full 6-agent pipeline (Reader → Format → Drafter → Verifier → Refiner → Overseer) is **NOT** mandatory. Only the first three stages are required to produce a filing-grade draft. Stages 4–6 are OPTIONAL QC layers the advocate explicitly invokes. Default exit point is here, after Drafter (~280K tokens). Full pipeline ~600K tokens — disproportionate for routine pleadings.

When `draft-v1.docx` is written, the Drafter's job is complete. The advocate decides whether to invoke the QC stages.


---

## v0.2.2 OUTPUT-PAIRING DISCIPLINE (load-bearing — every agent must follow)

**Every `.md` output artifact MUST be paired with a `.docx`.** Advocates do not natively read Markdown — they read Word. Every pipeline output (case-facts.md from Reader, format-shell.md from Format, draft-v1.md from Drafter, verification-report.md from Verifier, draft-v2.md from Refiner, opposing-notes.md from Overseer) must have a corresponding `.docx` rendered with the same locked Word styles.

**This plugin produces pleadings** — the shipped reference.docx is the pleading variant (TNR 14pt 1.5 spacing, Heading 2 bold + UNDERLINED + centered with letter-spacing for the spaced `F A C T S` effect).

### How to produce the paired `.docx`

Every agent runs the shipped helper script as its final post-`.md`-write step:

```bash
bash "${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/pair_md_to_docx.sh" <output.md>
```

The helper:
1. Resolves the reference.docx in `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/reference.docx`
2. Runs pandoc with `--reference-doc` and `--from=markdown+pipe_tables+raw_tex` to produce the `.docx`
3. Runs the shipped `fix_docx_tables.py` to force column widths on every table

For overriding (e.g., a per-case-folder reference.docx), pass the reference.docx as the second argument:

```bash
bash "${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/pair_md_to_docx.sh" \
    <output.md> <case-folder>/reference.docx
```

### Per-agent output-pairing map

| Agent | `.md` output | Paired `.docx` |
|---|---|---|
| Reader | `case-facts.md` | `case-facts.docx` |
| Format | `format-shell.md` | `format-shell.docx` |
| Drafter | `draft-v1.md` | `draft-v1.docx` |
| Verifier | `verification-report.md` | `verification-report.docx` |
| Refiner | `draft-v2.md` | `draft-v2.docx` |
| Overseer | `opposing-notes.md` + `final-draft.md` | `opposing-notes.docx` + `final-draft.docx` |

Every agent calls `pair_md_to_docx.sh` once for each `.md` it writes. Skipping this step leaves the advocate with `.md` files that cannot be opened natively in Word.
