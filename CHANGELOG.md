# Changelog — `district-court-drafting`

All notable changes to this plugin are documented here. Versioning follows [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`) with an `-alpha` / `-beta` suffix during pre-release.

---

## [0.2.2-alpha] — 2026-05-24

### Output-pairing discipline — every `.md` paired with `.docx`

Advocates do not natively read Markdown. Every pipeline output artifact (case-facts.md from Reader, format-shell.md from Format, draft-v1.md from Drafter, verification-report.md from Verifier, draft-v2.md from Refiner, opposing-notes.md + final-draft.md from Overseer) is now paired with a corresponding `.docx` rendered using the same locked Word styles in the shipped reference.docx.

### Added

- **`pair_md_to_docx.sh`** — helper script in `skills/<base>/` that every agent calls after writing a `.md` output. Wraps the two-step pandoc + fix_docx_tables.py pipeline so every agent produces a paired `.docx` without re-implementing the conversion logic.
- **OUTPUT-PAIRING DISCIPLINE** section in `_drafting_common/SKILL.md` documenting the per-agent output-pairing map (Reader → case-facts.{md,docx}; Format → format-shell.{md,docx}; Drafter → draft-v1.{md,docx}; Verifier → verification-report.{md,docx}; Refiner → draft-v2.{md,docx}; Overseer → opposing-notes.{md,docx} + final-draft.{md,docx}).

### Why the change

User feedback from the 2026-05-24 EPFO test demonstrated that the QC pipeline output (`verification-report.md`, `opposing-notes.md`) was not accessible to the advocate in their normal Word workflow. The advocate explicitly stated: "every note … needs to be docx too." v0.2.2 closes this gap.

### Clarification — per-court formatting

v0.2.1 propagated a single Bombay HC Nagpur pleading-style reference.docx across all 14 plugins. The structural styling (TNR 14pt 1.5 spacing 4cm-left margin Heading 1/2/3/4) is broadly defensible for pleading-style plugins (HC / SC / Tax / Rent / MACT / Banking / Company / Consumer / Labour / Family / IP / District Court) because the court-specific differences (cause-title text, annexure prefix, statutory opening, AOR Certificate language) live in the case-type SKILL.md (Drafter content) not the reference.docx (style template). For SC the universal style is correct as the SC Registry mandate matches the HC convention (A4 + TNR 14pt + 1.5 spacing + 4cm left margin). Court-specific content (P-1/P-2 annexure prefix instead of ANNEXURE-A; SYNOPSIS + LIST OF DATES instead of just INDEX; AOR Certificate verbatim) is rendered by the Drafter from the case-type skill. Per-bench fine-tuning (e.g., Delhi HC double-spacing under Original Side Rules 2018; Punjab & Haryana watermarked paper) is achieved by supplying a case-folder reference.docx override.

For the two TRANSACTIONAL plugins (indian-contracts-drafting-litigation + indian-property-drafting-litigation), v0.2.1 wrongly applied the pleading-style reference.docx. Those two plugins now ship a transactional-instrument variant (TNR 12pt single-spaced, no spaced section headers, no underline on headings) under their own v0.2.2 release.

---

## [0.2.1-alpha] — 2026-05-24

### Filing-grade render-defect repair + pipeline-optionality

The v0.1.0 render path produced filing-grade Markdown but the pandoc → `.docx` conversion failed Bombay HC / equivalent Registry expectations on multiple counts (title not bold, section headers left-aligned, Index table column-headers wrapping vertically, party block leaking onto cover pages, ~6,200-word bloat). This release repairs the render path, calibrated against an actual filed Bombay HC Nagpur Second Appeal pleading the author supplied as the filing-grade reference. Inherits the v0.2.1 fixes from `indian-hc-drafting-litigation`.

### Added

- **Pre-customised `reference.docx`** in the plugin's base-skill folder with locked Word styles (TNR 14pt body, 1.5 line spacing, 4cm left / 2.5cm right-top-bottom margins, Heading 1 bold centered, Heading 2 bold + UNDERLINED + centered + letter-spacing for the spaced `F A C T S` effect, Heading 3 bold + UNDERLINED + centered for unspaced section headers, Heading 4 bold + UNDERLINED + left for `MOST RESPECTFULLY SHEWETH:` style anchors, fixed table layout).
- **`build_reference_docx.py`** — reproducible python-docx build script for the shipped reference.docx.
- **`fix_docx_tables.py`** — post-pandoc Python script that forces column widths on every table (5-col 8/8/60/14/10; 4-col 10/10/65/15; 3-col 10/75/15; 2-col 18/82). Locks first-row bold + centered + vertically-centered cells. Drafter runs this as the final post-pandoc step.
- **MARKDOWN HEADING DISCIPLINE** in the Drafter prompt + base SKILL.md (Heading 1 / Heading 2 / Heading 3 / Heading 4 mapping for court header / spaced section headers / unspaced section headers / left-anchored headings).
- **VERBOSITY DISCIPLINE** with per-case-type word-count targets and hard ceilings.
- **PIPELINE-OPTIONALITY** — Verifier / Refiner / Overseer now OPTIONAL QC layers. Default exit point is after Stage 3 (Drafter).
- **COVER-PAGE DISCIPLINE** — INDEX / SYNOPSIS / LIST OF ANNEXURES each begin on `\newpage` with short cause-title only.
- **Bold-number paragraph convention** — Facts and Grounds paragraphs use `**1.** **2.** **3.**`.
- **Inline-bold highlighting convention** for property descriptors / annexure markers / key terms within Facts narrative.

### Changed

- **Drafter pandoc command** is now TWO steps (pandoc → .docx, then `fix_docx_tables.py`). Step 2 is non-negotiable; skipping it reproduces the v0.2.0 stacking-column defect.

### Cost / token-budget note

Running the full 6-agent pipeline burns approximately 600K tokens per draft, which can exhaust an advocate's Claude session limit. v0.2.1 makes Stages 4–6 OPTIONAL so a baseline Reader → Format → Drafter run (~280K tokens) is sufficient for routine pleadings. The optional QC stages remain available for high-stakes matters.

---

## [Unreleased]

### Pending before v0.1.0 stable
- User-paste style references into all ten `format-from-user.md` files
- Community Registry-validation feedback for non-Maharashtra State configurations
- Filed state-config exemplars contributed by advocates practising at NE States and J&K-Ladakh (currently stubs)
- First Registry-validation pass on sample filings (one civil, one criminal) at multiple State District Courts

---

## [0.1.0-alpha · second iteration] — 2026-05-15

### Added — State-config architecture (post-audit honest multi-State rebuild)

After a multi-State audit the plugin was rebuilt to be honestly multi-State rather than Maharashtra-rebadged-as-default. The core insight: while the procedural backbone (CPC, BNSS, IEA/BSA, Specific Relief Act, etc.) is pan-India, State-specific overlays — court designation taxonomy, Court-Fees Act, State Civil Courts Act, Stamp Act, Civil Manual — vary substantially.

#### State-config exemplars (16 files)
`state-config/state-config-template.md` — universal 12-section template
`state-config/exemplars/`:
- `maharashtra.md` — Researched · awaiting Registry-acceptance validation · Civil Judge SD/JD · Bombay Court-Fees Act 1959 · Bombay Civil Courts Act 1869
- `karnataka.md` — Senior Civil Judge / Civil Judge · Karnataka Court-Fees Act 1958 · Karnataka Civil Courts Act 1964
- `tamil-nadu.md` — **Subordinate Judge / District Munsiff** (distinct from Maharashtra) · TN Court-Fees Act 1955 · Tamil-language pleadings permitted
- `kerala.md` — **Sub Court / Munsiff Court** · Kerala Court-Fees Act 1959 · Kerala Civil Courts Act 1957
- `andhra-pradesh-telangana.md` — Senior Civil Judge / Junior Civil Judge · AP Court-Fees Act 1956 (continues post-2014 bifurcation)
- `gujarat.md` — Civil Judge SD/JD (inherited from Bombay State 1960) · Bombay Court-Fees Act 1959 · Gujarat Stamp Act 1958
- `delhi.md` — Senior Civil Judge / Civil Judge / ADJ · **Chief METROPOLITAN Magistrate** terminology · Punjab Courts Act 1918 (extended)
- `uttar-pradesh-uttarakhand.md` — Civil Judge SD/JD · Court-Fees Act 1870 (Central, UP-amended)
- `madhya-pradesh-chhattisgarh.md` — **Civil Judge Class I / Class II** (distinct terminology)
- `west-bengal.md` — Civil Judge SD/JD · West Bengal Court-Fees Act 1971 · Bengal Civil Courts Act 1887 · CMM/MM in Kolkata
- `bihar-jharkhand.md` — **Sub Judge / Munsif** (older terminology)
- `punjab-haryana-chandigarh.md` — Senior Sub Judge / Sub Judge · Punjab Courts Act 1918
- `rajasthan.md` — Senior Civil Judge / Civil Judge · Rajasthan Court-Fees Act 1961
- `odisha.md` — Civil Judge (Senior Division) / Civil Judge (Junior Division) parenthetical form · Bengal Civil Courts Act 1887 (historic)
- `north-east-states.md` — stub · community contribution welcomed
- `jammu-kashmir-ladakh.md` — stub · post-2019 reorganisation · community contribution welcomed

#### Refactored to reference state-config
- `skills/_district_pleading_base/SKILL.md` — Court header sourced from state-config.md Section 2; per-State court-designation taxonomy table replaces Maharashtra-only list; per-State Court-Fees Act table replaces Bombay-only example
- `skills/_drafting_common/SKILL.md` — Court header pattern softened with per-State taxonomy notes
- `agents/reader/reader.md` — Now requires `<case-folder>/state-config.md` (halts pipeline if absent)
- `README.md` — State-aware design description expanded to credit the 16 State exemplars
- `USAGE.md` — Setup flow updated to point users at `state-config/exemplars/<your-state>.md` for the canonical State-specific configuration

### Why this matters

Earlier today, the plugin had honest state-aware architecture but every example cited Maharashtra/Bombay defaults. That subtly misled non-Maharashtra users into thinking Civil Judge SD/JD, Bombay Court-Fees Act, and Bombay Civil Courts Act were pan-India defaults. They are NOT — Tamil Nadu uses Subordinate Judge/Munsiff, Kerala uses Sub Court/Munsiff Court, MP uses Civil Judge Class I/II, Bihar/Jharkhand use Sub Judge/Munsif, AP/Telangana use Senior/Junior Civil Judge. The 16 State exemplars now make each State's correct terminology explicit and the plugin renders accordingly via state-config.md.

This parallels the multi-bench rebuild done earlier today for `indian-hc-drafting`. The District plugin had milder symptoms (state-aware architecture was already correct; Maharashtra-as-default was only in examples) but the same disease.

### Quality-gate audit (per `05-quality-gates.md`)

- Gate 1 · Copyright firewall — ✅ PASS
- Gate 2 · Rule 36 BCI firewall — ✅ PASS
- Gate 3 · NOTICE.md doctrine — ✅ PASS
- Gate 4 · Statute currency (BNSS / BSA / BNS) — ✅ PASS
- Gate 5 · Bench / State scope honesty — ✅ PASS (per-State validation depth declared honestly; Maharashtra is the only deep-validated State)
- Gate 6 · Falsification triggers — ✅ PASS

### Provenance (new authorities researched this iteration)

State Civil Courts Acts: Bombay 1869 · Madras 1873 (historic) · Bengal 1887 · Punjab Courts Act 1918 · Karnataka 1964 · Rajasthan Ordinance 1950 · MP 1958 · Tamil Nadu Civil Courts Act · AP Civil Courts Act · Kerala Civil Courts Act 1957
State Court-Fees Acts: Bombay 1959 · Madras 1955 (Tamil Nadu) · Karnataka 1958 · Kerala 1959 · AP 1956 · West Bengal 1971 · Rajasthan 1961 · Court-Fees Act 1870 (Central, State-amended for Delhi · UP · Uttarakhand · MP · Chhattisgarh · Bihar · Jharkhand · Odisha · etc.)
State Stamp Acts: Maharashtra 1958 · Karnataka 1957 · Kerala 1959 · Rajasthan 1998 · Gujarat 1958 · Indian Stamp Act 1899 (State-amended in remaining States)

---

## [0.1.0-alpha · first iteration] — 2026-05-15

### Added

#### Plugin essentials
- `.claude-plugin/plugin.json` — plugin manifest
- `LICENSE` — MIT
- `NOTICE.md` — full provenance and privilege statement (10 sections)
- `README.md` — overview, skills inventory, agent pipeline, installation, roadmap
- `.gitignore` — standard exclusions plus District-specific artifact paths

#### Shared infrastructure
- `skills/_drafting_common/SKILL.md` — anti-pollution rules, District AI-use risk constraints, CPC + BNSS discipline, dual-citation for criminal cases, Court-Fees Schedule + Civil Manual + Vakalatnama State-supplied design
- `skills/_district_pleading_base/SKILL.md` — universal District Court pleading skeleton (12-section civil + criminal branching), Cause Title template by Judicial Officer Designation, Schedule of Property template, Verification (verbatim CPC Order VI Rule 15), Court-Fee block, Jurisdiction block, Limitation block

#### Civil case-type skills (5)
- `skills/specific-relief-suit-draft/` — Specific Performance / Declaration / Perpetual Injunction / Mandatory Injunction under Specific Relief Act 1963 + CPC Order VII; Section 16(c) Readiness-and-Willingness averment as load-bearing mandatory paragraph
- `skills/plaint-ws-draft/` — Plaint (Order VII CPC) or Written Statement (Order VIII CPC) — the core district-court civil pleading; Commercial Suit branch with Statement of Truth (Commercial Courts Act 2015)
- `skills/application-draft/` — Civil Interlocutory Application — Temporary Injunction (Order XXXIX three-limb test per *Dalpat Kumar v. Prahlad Singh*), Condonation of Delay (Section 5 Limitation Act), Restoration (Order IX), Amendment (Order VI Rule 17), Commission (Order XXVI), inherent powers (Section 151 fallback)
- `skills/petition-draft/` — Succession Certificate (Sections 370-381 ISA 1925), Probate (Section 276 ISA), Letters of Administration (Section 273 ISA), Guardianship (Guardians and Wards Act 1890), Matrimonial Mutual Divorce (Section 13B HMA 1955), Maintenance (Section 144 BNSS)
- `skills/pleadings-draft/` — generic civil pleadings (amendment pleadings, reply/rejoinder, particulars, supplemental pleadings)

#### Criminal case-type skills (5) — shipping under v0.1.0-alpha (note: original plan had criminal under v0.2.0; compressed into v0.1.0-alpha alongside civil)
- `skills/criminal-complaint-draft/` — Private Criminal Complaint under Section 223 BNSS 2023 (corresponding to Section 200 CrPC 1973); ingredients-of-offence mapping mandatory
- `skills/bail-draft/` — Regular Bail Application under Section 483 BNSS (Section 439 CrPC); custody-status paragraph mandatory; *Sanjay Chandra* + *Satender Antil* jurisprudence
- `skills/anticipatory-bail-draft/` — Anticipatory Bail Application under Section 482 BNSS (Section 438 CrPC); apprehension-of-arrest paragraph mandatory; *Sushila Aggarwal* Constitution Bench guidance
- `skills/criminal-revision-draft/` — Criminal Revision under Sections 438 + 442 BNSS (Sections 397 + 401 CrPC); interlocutory-order disclaimer; *Munna Devi* scope-of-revision discipline
- `skills/bnss-313-statement-draft/` — Statement of the Accused under Section 351 BNSS (Section 313 CrPC) — defence-team coaching document (NOT a filed pleading)

Each case-type skill includes a `SKILL.md` (structural metadata + hard rules + falsification triggers + provenance) and a `format-from-user.md` (style-reference template awaiting user paste).

#### Six-agent drafting pipeline (District-tuned)
- `agents/reader/reader.md` — case folder ingestion with civil/criminal branching, EXHIBIT-letter mapping, limitation computation per Limitation Act Schedule, custody-status extraction for criminal, Schedule of Property extraction for immovable-property civil, citation-discipline check
- `agents/format/format.md` — case-type SKILL.md loader, verbatim CPC Order VI Rule 15 verification insertion, dual-citation pre-flagging for criminal, Schedule of Property pre-population, Court-Fee + Jurisdiction + Limitation blocks pre-population
- `agents/drafter/drafter.md` — fresh prose authoring, case-type-specific mandatory paragraphs (Section 16(c) Readiness for SR, three-limb Temporary Injunction, ingredients-of-offence for Criminal Complaint, custody-details + grounds-for-bail for Bail, etc.), `[CITATION NEEDED]` discipline, pandoc `.docx` render
- `agents/verifier/verifier.md` — thirty District-specific anti-hallucination checks (F1-F30) covering universal civil/criminal pleading rules + Specific Relief Section 16(c) + Plaint cause-of-action + WS paragraph-wise traversal + Application three-limb + Commercial Suit Statement of Truth + Criminal Complaint ingredients-mapping + Bail custody-status + Anticipatory Bail apprehension + Criminal Revision interlocutory disclaimer + Section 351 BNSS internal consistency
- `agents/refiner/refiner.md` — applies Verifier flags, restores verbatim blocks, District formatting enforcement, citation normalisation, AI-style marker removal
- `agents/overseer/overseer.md` — opposing-counsel and Bench lens, case-type-specific attack-vector sweep, preliminary objections anticipation, Order VII Rule 11 rejection vectors, deemed-admission risk audit for WS, Section 22 SR Act alternative-relief check, final draft

### Quality-gate audit (per `05-quality-gates.md` of the India-Legal Corpus Pipeline)

- Gate 1 · Copyright firewall — ✅ PASS (zero corpus prose transcribed; only public-domain statutory recitations verbatim)
- Gate 2 · Rule 36 BCI firewall — ✅ PASS (no marketing, placeholders for advocate name, free MIT, no commercial channel)
- Gate 3 · NOTICE.md doctrine — ✅ PASS (all placeholders, structural patterns trace to CPC, BNSS, substantive statutes, public-domain authorities)
- Gate 4 · Statute currency — ✅ PASS (BNSS / BSA / BNS dual-citation enforced in `_drafting_common` and all five criminal skills)
- Gate 5 · Bench scope honesty — ✅ PASS (District Courts across India; State-specific overlay supplied by user via `format-from-user.md`)
- Gate 6 · Falsification triggers — ✅ PASS (each `SKILL.md` carries triggers and handling)

### Authorities cited (provenance trace)

**Civil statutes:** CPC 1908 (Orders VI, VII, VIII, IX, XI, XVIII, XIX, XXIII, XXVI, XXXIII, XXXIX, XL, XLVII, LVI; Sections 148A, 151), Specific Relief Act 1963 (Sections 10, 14, 16(c), 20, 22, 34, 35, 38, 39, 41), Indian Contract Act 1872, Transfer of Property Act 1882, Indian Succession Act 1925 (Sections 218, 222, 273, 276, 370-381), Hindu Marriage Act 1955 (Sections 9, 10, 13, 13B), Family Courts Act 1984, Representation of the People Act 1951 (Section 81), Guardians and Wards Act 1890, Limitation Act 1963, Court-Fees Act 1870, Suits Valuation Act 1887, Commercial Courts Act 2015, Indian Stamp Act 1899

**Criminal statutes:** BNSS 2023 (Sections 35, 144, 223, 225, 226, 227, 269+, 274-275, 297, 334, 348, 351, 406, 438, 442, 480, 481, 482, 483, 514-519) + CrPC 1973 (transitional pairs), BNS 2023 + IPC 1860 (transitional), BSA 2023 + IEA 1872 (transitional)

**Constitutional:** Articles 20(3), 21

**Jurisprudence (cited as authority only, zero prose lifted):** Dalpat Kumar v. Prahlad Singh (1992) 1 SCC 719 · Munna Devi v. State of Rajasthan (2001) 9 SCC 631 · Gurbaksh Singh Sibbia v. State of Punjab (1980) 2 SCC 565 · Sushila Aggarwal v. State (NCT of Delhi) (2020) 5 SCC 1 · Arnesh Kumar v. State of Bihar (2014) 8 SCC 273 · Satender Kumar Antil v. CBI (2022) 10 SCC 51 · Sanjay Chandra v. CBI (2012) 1 SCC 40

### Provenance

Patterns encoded in this plugin trace to public-domain procedural authority only. Cross-validation against the India-Legal Corpus Pipeline (Phase 03 reports for Specific Relief, Plaint/WS, Application, Petition, Pleadings, Criminal Pleadings) was used to confirm *which patterns are used in practice* — never to lift *language*.

No drafted prose has been transcribed from the corpus or from any third-party advocate's drafts.
