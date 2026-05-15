# USAGE — `district-court-drafting`

End-user guide for any advocate who wants to use this plugin to draft District Court pleadings (civil or criminal).

---

## 1. What you need before you start

| Requirement | Why |
|---|---|
| **A Claude-compatible runtime** | This plugin runs inside any Anthropic-compatible runtime that parses the standard SKILL.md plugin format |
| **An active Anthropic API account** | The plugin invokes the Anthropic API through your chosen runtime; usage is billed to your account |
| **Microsoft Word or LibreOffice** | To open and review the `.docx` output |
| **`pandoc`** (recommended) | `brew install pandoc` on Mac, `sudo apt install pandoc` on Linux |
| **Your case folder** | A directory containing the case documents |
| **Your State's Court-Fees Act PDF** | Each State amends the Court-Fees Act differently — supply yours in `state-instruments/` |
| **Your State's Civil Manual / Criminal Manual** (where applicable) | For State-specific formatting overlay |

---

## 2. Install the plugin

The plugin is a directory of markdown skill files + agent files. Clone it to wherever your Anthropic runtime discovers plugins:

```bash
git clone https://github.com/wolfgang-rush/district-court-drafting \
  <your-anthropic-plugins-folder>/district-court-drafting
```

Verify the plugin loads in your runtime according to your runtime's standard plugin-listing mechanism.

---

## 3. One-time setup — paste your drafting style references

Open each `format-from-user.md` file and paste your preferred drafting style for each section. Crucially, **declare your State at the top** so the plugin uses the right Court-Fees Schedule and pecuniary jurisdiction citation:

```bash
cd ~/.claude/plugins/district-court-drafting/skills
$EDITOR specific-relief-suit-draft/format-from-user.md
$EDITOR plaint-ws-draft/format-from-user.md
$EDITOR application-draft/format-from-user.md
$EDITOR petition-draft/format-from-user.md
$EDITOR pleadings-draft/format-from-user.md
$EDITOR criminal-complaint-draft/format-from-user.md
$EDITOR bail-draft/format-from-user.md
$EDITOR anticipatory-bail-draft/format-from-user.md
$EDITOR criminal-revision-draft/format-from-user.md
$EDITOR bnss-313-statement-draft/format-from-user.md
```

Each `format-from-user.md` Section 1 requests minimal State identification. **For the full per-State configuration** (court designation taxonomy, Court-Fees Act citation, State Civil Courts Act, Stamp Act, Vakalatnama format, language, paper conventions), copy the right exemplar from `state-config/exemplars/` into your case folder:

```bash
# Copy the State exemplar matching your practice State to your case folder
cp <plugin-folder>/state-config/exemplars/<your-state>.md \
   <your-case-folder>/state-config.md
```

State exemplars available:
- `maharashtra.md` — deep · battle-tested
- `karnataka.md`, `tamil-nadu.md`, `kerala.md`, `andhra-pradesh-telangana.md`, `gujarat.md`, `delhi.md`, `uttar-pradesh-uttarakhand.md`, `madhya-pradesh-chhattisgarh.md`, `west-bengal.md`, `bihar-jharkhand.md`, `punjab-haryana-chandigarh.md`, `rajasthan.md`, `odisha.md` — researched · awaiting Registry validation
- `north-east-states.md`, `jammu-kashmir-ladakh.md` — stub · community contribution welcomed

Without `state-config.md` AND `format-from-user.md` populated, the Drafter cannot render in your State's idiom.

---

## 4. Prepare your case folder

```
~/cases/<client-code-or-matter-name>/
├── CLAUDE.md                      # case context (case type · side · State · court designation · parties · dates)
├── state-config.md                # YOUR State exemplar (copied from state-config/exemplars/<your-state>.md)
├── citations.md                   # YOUR confirmed list of case citations
├── laws/                          # PDFs of statutes not in training data
│   ├── Hindu-Marriage-Act-1955.pdf
│   ├── Negotiable-Instruments-Act-1881.pdf
│   └── (etc.)
├── state-instruments/             # YOUR State-specific PDFs (Court-Fees Act, Civil Manual, etc.)
│   ├── <State>-Court-Fees-Act.pdf
│   ├── <State>-Civil-Manual.pdf
│   └── (etc.)
├── 01-agreement-to-sell.pdf       # principal document
├── 02-legal-notice.pdf
├── 03-reply.pdf
├── 04-title-documents.pdf
└── (other case documents)
```

### What goes in `CLAUDE.md`

```markdown
# Case Context

## Case type
specific-relief-suit              # OR plaint-ws / application / petition / pleadings
                                  # / criminal-complaint / bail / anticipatory-bail
                                  # / criminal-revision / bnss-313-statement

## Side
civil                             # OR criminal

## State (must match your state-config.md)
<your State>                      # e.g., Maharashtra, Karnataka, Tamil Nadu, Kerala, etc.

## Court designation (use your State's terminology — see state-config.md Section 2)
<e.g., "Civil Judge, Senior Division at [Place], District [District Name]" for Maharashtra/Karnataka-style
 OR "Subordinate Judge at [Place]" for Tamil Nadu
 OR "Sub Judge at [Place]" for Bihar/Jharkhand
 OR "Senior Civil Judge at [Place]" for AP/Telangana/Rajasthan/Delhi
 OR similar per your State's hierarchy>
                                  # Criminal: District Judge / JMFC / CMM / Sessions / etc.

## Parties
Plaintiff: [Name], [Age], [Occupation], R/o [Address]
Defendant: [Name], [Age], [Occupation], R/o [Address]

## Cause of action
Cause of action arose on: <DD.MM.YYYY> at [Place]

## Limitation Article (Schedule to Limitation Act 1963)
Article 54                        # for specific performance — verify against your fact

## Suit value
Value for court-fee and jurisdiction: ₹ [Value]

## Schedule of Property (for immovable-property suits)
Located in: Village/Mohalla [____], Taluka [____], District [____], State [____]
Survey No.: [____]
Area: [____ sq.m / acres / hectares]
Boundaries — N: [____] | S: [____] | E: [____] | W: [____]

## Advocate engaged
Name: [Advocate Name]
Bar Council enrolment number: [Enrolment No.]
```

### What goes in `citations.md`

```markdown
# Confirmed Case Citations

| Case | Citation | Cited for |
|---|---|---|
| Dalpat Kumar v. Prahlad Singh | (1992) 1 SCC 719 | Three-limb test for temporary injunction |
| (add your case-specific citations) | | |
```

The Drafter never generates a citation from training memory. Every citation in the output traces to this file. If a ground requires support not in this list, the Drafter inserts `[CITATION NEEDED]`.

---

## 5. Run the pipeline

Open your Claude-compatible session inside your case folder. Invoke the case-type skill:

```
draft specific performance suit
```

or:

```
/specific-relief-suit-draft
```

Other invocation phrases:

| Skill | Trigger phrases |
|---|---|
| `specific-relief-suit-draft` | "draft specific performance suit" · "draft injunction suit" · "draft declaration suit" |
| `plaint-ws-draft` | "draft plaint" · "draft written statement" · "draft ws" |
| `application-draft` | "draft application" · "draft interlocutory" · "draft IA" |
| `petition-draft` | "draft petition" · "draft succession petition" · "draft matrimonial petition" |
| `pleadings-draft` | "draft pleading" · "draft amendment pleading" · "draft rejoinder" |
| `criminal-complaint-draft` | "draft criminal complaint" · "draft private complaint" · "draft section 223 bnss complaint" |
| `bail-draft` | "draft bail application" · "draft regular bail" · "draft section 483 bail" |
| `anticipatory-bail-draft` | "draft anticipatory bail" · "draft section 482 bail" · "pre-arrest bail" |
| `criminal-revision-draft` | "draft criminal revision" · "draft revision application" |
| `bnss-313-statement-draft` | "draft 313 statement" · "draft 351 statement" · "accused statement bnss" |

The six-agent pipeline runs (Reader → Format → Drafter → Verifier → Refiner → Overseer) and produces:

```
case-folder/
├── case-facts.md
├── format-shell.md
├── draft-v1.md           draft-v1.docx
├── verification-report.md
├── draft-v2.md           draft-v2.docx
├── opposing-notes.md
└── final-draft.docx      ← OPEN THIS IN WORD
```

---

## 6. Review and finalise

Open `final-draft.docx` in Microsoft Word. Apply tracked-changes review.

Read carefully:
- **Statement of Facts** — every paragraph matches your case folder
- **Schedule of Property** (civil immovable suits) — boundaries / survey / area correct
- **Court-Fee block** — value and State Schedule citation correct
- **Verification** — your name, place, date filled in
- **Dual-citation pattern** (criminal cases) — every BNSS reference paired with CrPC, every BNS with IPC
- **Custody-status paragraph** (bail / anticipatory bail) — arrest date, place, period correct

`opposing-notes.md` contains the Overseer's anticipation of preliminary objections — read this before filing.

You remain the responsible advocate. The plugin produces starting material. Sign only after full review.

---

## 7. State-specific notes

This plugin is designed to be portable across Indian States. State-specific elements that the plugin DOES NOT assume:
- Court-Fees Act schedule (varies — Bombay 1959 / Madras 1955 / Karnataka 1958 / Delhi 1962 / etc.)
- Civil Manual / Criminal Manual conventions (each State's High Court)
- Vakalatnama format (varies)
- Pecuniary jurisdiction limits (set by State High Court notifications)
- Family Court Rules (where Family Courts are established)

Supply these via `state-instruments/` in your case folder, and declare your State in `format-from-user.md` Section 1.

---

## 8. The plugin in one sentence

**This plugin converts your District Court case folder into a court-ready `.docx` pleading — civil or criminal — with every fact traceable to your case folder, every citation traceable to your confirmed list, the CPC discipline (verification, court-fee, jurisdiction, limitation) honoured, the BNSS dual-citation pattern enforced, and you remain the responsible advocate at every stage.**

---

## 9. Help and contact

- Bug reports / feature requests / State-validation feedback (especially for non-Maharashtra States): open a GitHub Issue at https://github.com/wolfgang-rush/district-court-drafting/issues
- This project does not have an email contact channel and does not accept private legal-services inquiries.
- The plugin is released under the MIT licence — see `LICENSE`.
- For provenance and Bar Council Rule 36 compliance details, see `NOTICE.md`.
