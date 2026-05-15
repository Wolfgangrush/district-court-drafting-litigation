---
name: _district_pleading_base
description: Universal pleading skeleton for District Courts of India. Shared base referenced by every case-type-specific drafting skill in this plugin (specific-relief-suit-draft, plaint-ws-draft, application-draft, petition-draft, pleadings-draft, criminal-complaint-draft, bail-draft, anticipatory-bail-draft, etc.). Not auto-fired by user prompts; loaded via @-reference from case-type skills.
allowed-tools: Read, Write, Edit, Bash, Glob
---

# District Court Pleading — Universal Base

Every District Court drafting skill in this plugin extends this base. It encodes the structural skeleton mandated by:

- The Code of Civil Procedure 1908 (Orders VI, VII, VIII, XIX, XXVI, XXXIX)
- The Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding sections of the Code of Criminal Procedure 1973 for transitional matters)
- The Indian Evidence Act 1872 / Bharatiya Sakshya Adhiniyam 2023 (verification provisions)
- The Limitation Act 1963 and its Schedule
- The Court-Fees Act 1870 (as amended State-wise)
- The Suits Valuation Act 1887
- State-specific Civil Manuals and Criminal Manuals (user-supplied where applicable)

The skeleton is uncopyrightable (universal section headers mandated by procedural law). Placeholders are filled at runtime from a user-supplied case folder. **No drafted prose from any external source is encoded; every connective phrase is original or directly recited from statute / Rule.**

---

## 1. Universal section order (civil-side District Court pleadings)

A District Court civil pleading produced by this plugin contains the following sections in this exact order. Case-type skills override individual section content; the order does not change.

1. **CAUSE TITLE** — Court name, case-type and number, parties block.
2. **PARTIES BLOCK** — Plaintiff(s) and Defendant(s) with name, age, occupation, address. Placeholders only.
3. **STATEMENT OF FACTS** — Chronological narration of facts giving rise to the suit / proceeding. Each fact paragraph numbered.
4. **PARTICULARS OF CLAIM / CAUSE OF ACTION** — Specific identification of the right asserted, the breach / denial, and the date(s) of cause of action.
5. **JURISDICTION** — Statement of territorial and pecuniary basis for the chosen forum (Section 16 / 17 / 18 / 19 / 20 CPC for territorial; State pecuniary limits for pecuniary).
6. **LIMITATION** — Statement of the specific Article of the Schedule to the Limitation Act 1963 applicable + cause of action date + filing date + the period.
7. **COURT-FEE** — Statement of the value of suit for the purpose of court-fee and jurisdiction + the Schedule of the State Court-Fees Act under which fee is paid + receipt number / mode of payment.
8. **PRAYER** — Specific relief sought, enumerated (a), (b), (c) ... ending with a catchall.
9. **VERIFICATION** — Per CPC Order VI Rule 15 — verbatim Rule recitation. Separate sub-blocks for paragraphs verified on personal knowledge and paragraphs verified on information believed to be true (with citation of source of information).
10. **PLACE + DATE + ADVOCATE'S SIGNATURE BLOCK** — Plaintiff's advocate's signature line with name, Bar Council enrolment number, designation ("Advocate for the Plaintiff").
11. **SCHEDULE OF PROPERTY** — Mandatory where the suit involves immovable property. Format described in Section 4 below.
12. **LIST OF DOCUMENTS / EXHIBITS** — Consolidated table at the end (parallel to the inline EXHIBIT markers in the Statement of Facts).

For Written Statements (Order VIII CPC), Application drafts (interlocutory), Petition drafts, and the criminal-side skills (Bail, Anticipatory Bail, Criminal Complaint), individual case-type SKILL.md files override the section list. The universal CPC discipline (verification, signature, date) is preserved across all variants.

---

## 2. Cause Title — universal format

```
IN THE COURT OF THE [Judicial Officer Designation] AT [Place], DISTRICT [District Name]

  [Case Type] No. [        ] of [Year]

  IN THE MATTER OF:

  [Plaintiff Name]
  [Age], [Occupation]
  R/o: [Address]                                ... Plaintiff

                          VERSUS

  [Defendant Name]
  [Age], [Occupation]
  R/o: [Address]                                ... Defendant
```

The Judicial Officer Designation varies by State. **Pick the one that matches your `state-config.md` — see `${CLAUDE_PLUGIN_ROOT}/state-config/exemplars/` for the correct designation taxonomy per State.**

Civil hierarchy by State (illustrative, not exhaustive):

| State | Apex District | Senior civil | Junior civil |
|---|---|---|---|
| Maharashtra · Gujarat · West Bengal · Odisha | District Judge | Civil Judge, Senior Division | Civil Judge, Junior Division |
| Karnataka | District Judge | Senior Civil Judge | Civil Judge |
| Tamil Nadu (and Puducherry) | District Judge | **Subordinate Judge** | **District Munsiff** |
| Kerala (and Lakshadweep) | District Judge | **Sub Court** (Subordinate Judge) | **Munsiff Court** (Munsiff) |
| Andhra Pradesh · Telangana | District Judge | Senior Civil Judge | Junior Civil Judge |
| Delhi (NCT) | District Judge | Senior Civil Judge | Civil Judge |
| Uttar Pradesh · Uttarakhand · Bihar pre-2000 lineage | District Judge | Civil Judge, Senior Division | Civil Judge, Junior Division |
| Bihar · Jharkhand | District Judge | **Sub Judge** | **Munsif** |
| Madhya Pradesh · Chhattisgarh | District Judge | **Civil Judge Class I** | **Civil Judge Class II** |
| Punjab · Haryana · Chandigarh | District Judge | Senior Sub Judge | Sub Judge |
| Rajasthan | District Judge | Senior Civil Judge | Civil Judge |
| J&K and Ladakh | District Judge | Sub Judge (Senior) | Munsiff |
| NE States (Assam etc.) | District Judge | varies (Sub Judge / Civil Judge SD) | varies (Munsiff / Civil Judge JD) |

Criminal hierarchy across most States:
- Sessions Judge / Additional Sessions Judge (sessions trial)
- Chief Judicial Magistrate (CJM) — or **Chief Metropolitan Magistrate (CMM)** in metropolitan areas (Mumbai, Kolkata, Chennai, Delhi)
- Judicial Magistrate First Class (JMFC) — or **Metropolitan Magistrate (MM)** in metropolitan areas
- Judicial Magistrate Second Class (where established)

Specialised courts:
- Family Court Judge (where Family Courts Act 1984 establishes them)
- Court of Small Causes (Mumbai, Chennai, Kolkata, Bangalore — separate jurisdiction)
- Commercial Court (Commercial Courts Act 2015 — established at District Judge level in major commercial centres)
- Other tribunal as user-specified

**The Drafter renders the cause-title designation per the user's `state-config.md` Section 2 (`court_designation_taxonomy`). The plugin does NOT default to any State's terminology.**

The case-type skill, combined with the user's case folder, identifies the correct designation.

---

## 3. Schedule of Property — mandatory for immovable-property suits

```
SCHEDULE OF PROPERTY

(Place this block at the very end of the plaint, after the Verification.)

PROPERTY: [Description of property in general — house / agricultural land / commercial premises / etc.]

LOCATION:
  Village / Mohalla: [Village or Mohalla]
  Taluka / Tehsil: [Taluka or Tehsil]
  District: [District]
  State: [State]

SURVEY DETAILS:
  Survey Number: [Survey No. / Khasra No. / Mutation entry as relevant]
  Sub-division: [if any]
  Area: [Area in acres / hectares / square metres / square feet — STATE the unit explicitly]
  Boundaries:
    NORTH: [N boundary]
    SOUTH: [S boundary]
    EAST: [E boundary]
    WEST: [W boundary]

MUTATION / RECORD OF RIGHTS:
  Owner of record: [as per village record / 7/12 extract / mutation entry as relevant]
  Mutation entry number and date: [number] dated <DD.MM.YYYY>

[Add additional rows for parking / appurtenant land / encumbrances / etc. as the case requires.]
```

Placeholders only. The Drafter fills these from `case-facts.md` Section 1 if the user supplies an extract of the village record / sub-registry document / 7/12 / khasra; otherwise the field reads `[Value Required from Case Folder]`.

---

## 4. Verification block — verbatim CPC Order VI Rule 15

```
VERIFICATION

I, [Plaintiff Name], the Plaintiff above-named, do hereby verify on solemn
affirmation that the contents of the foregoing paragraphs of the plaint are
true to my personal knowledge / belief / and that I have not suppressed any
material fact whatsoever, as follows:

Paragraphs [Para Numbers] are verified on the basis of my personal knowledge.

Paragraphs [Para Numbers] are verified on the basis of information received
by me from [Source of information], which I believe to be true.

Verified at [Place] this [Day] day of [Month, Year].

                                                  ____________________
                                                   (Plaintiff / Deponent)
```

Source: Order VI Rule 15 of the Code of Civil Procedure 1908. The text is statutory; the layout is conventional and may be adjusted via `format-from-user.md` (Section 9 of the case-type skill's style file).

---

## 5. Court-Fee block — State-aware

```
COURT-FEE

Value of the suit for the purpose of court-fee and jurisdiction:
₹ [Value in Indian Rupees]

Court-fee paid: ₹ [Amount Paid] (in cash / by stamp / by online challan / as applicable)

Statutory basis: Schedule [Schedule Number] of the [State Court-Fees Act, Year]
(e.g., Schedule I, Article 1 of the Bombay Court-Fees Act, 1959, as amended)
```

Each State has its own Court-Fees Act. The user's `state-config.md` Section 4 (`state_court_fees_act` + `default_court_fee_schedule_citation`) supplies the exact citation. See `${CLAUDE_PLUGIN_ROOT}/state-config/exemplars/` for validated per-State exemplars:

| State group | Court-Fees Act |
|---|---|
| Maharashtra · Gujarat | Bombay Court-Fees Act, 1959 (inherited from Bombay State 1960 bifurcation) |
| Tamil Nadu (and Puducherry) | Tamil Nadu Court-Fees and Suits Valuation Act, 1955 |
| Karnataka | Karnataka Court-Fees and Suits Valuation Act, 1958 |
| Kerala (and Lakshadweep) | Kerala Court-Fees and Suits Valuation Act, 1959 |
| Andhra Pradesh · Telangana | Andhra Pradesh Court-Fees and Suits Valuation Act, 1956 |
| West Bengal · Andaman & Nicobar | West Bengal Court-Fees Act, 1971 |
| Delhi · UP · Uttarakhand · MP · Chhattisgarh · Bihar · Jharkhand · Punjab · Haryana · Chandigarh | Court-Fees Act, 1870 (Central, as extended to each State with State amendments) |
| Rajasthan | Rajasthan Court-Fees and Suits Valuation Act, 1961 |
| Odisha | Court-Fees Act, 1870 (as extended to Odisha with State amendments) |
| J&K · Ladakh (post-2019) | Court-Fees Act, 1870 (as extended post-reorganisation) |
| NE States (Assam etc.) | varies — per State amendments to the Court-Fees Act 1870 |

---

## 6. Jurisdiction block — multi-limb

```
JURISDICTION

(a) Territorial jurisdiction:
    The cause of action wholly / partly arose at [Place], which falls within
    the territorial limits of this Hon'ble Court (Section 20 CPC).
    [OR, where Section 16/17/18/19 applies: state the specific section.]

(b) Pecuniary jurisdiction:
    The value of the suit, being ₹ [Value], falls within the pecuniary limits
    of this Hon'ble Court as fixed by the [State Civil Courts Act / High Court
    Notification dated <DD.MM.YYYY>].

(c) Subject-matter jurisdiction:
    The subject-matter of the suit is [brief description — specific performance
    of agreement / declaration of title / partition / etc.], which is within
    the subject-matter jurisdiction of this Hon'ble Court.
```

Each limb is filled by the Drafter from `case-facts.md` and the State-specific pecuniary-limit notification (user-supplied).

---

## 7. Limitation block

```
LIMITATION

The cause of action for the present suit arose on <DD.MM.YYYY> when [event].

The present suit is filed on <DD.MM.YYYY>, well within the period of
[Period — e.g., three years] prescribed by Article [Article Number]
of the Schedule to the Limitation Act, 1963.
```

Where the suit is filed within the limitation window — verbatim block. Where filed beyond the window — accompanying Application for Condonation of Delay under Section 5 of the Limitation Act 1963 is auto-included; the limitation block then says `The suit is being filed [N] days beyond the prescribed period; reasons for the delay are stated in the accompanying application for condonation under Section 5 of the Limitation Act, 1963.`

---

## 8. Hard rules (inherited from `_drafting_common`)

All rules from `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md` apply, plus the District-Court-specific rules above. The CPC discipline (verification, court-fee, jurisdiction, limitation, schedule of property) is non-negotiable for civil-side pleadings.

---

## 9. Authority hierarchy

When the case-type skills encode patterns, the order of authority is:

1. Code of Civil Procedure 1908 (with its Schedules and Appendix A Forms)
2. Bharatiya Nagarik Suraksha Sanhita 2023 / Code of Criminal Procedure 1973 (transitional)
3. Substantive statute applicable to the cause (Specific Relief Act 1963 / Indian Contract Act 1872 / Transfer of Property Act 1882 / etc.)
4. Limitation Act 1963 (Schedule)
5. Court-Fees Act (State-specific)
6. State Civil Manual / State Criminal Manual
7. Leading procedural textbook (Mulla on CPC / Sarkar / etc.) — for understanding only, not for verbatim copy
8. Corpus pattern — **never authoritative; only confirms which forms are used in practice**

Patterns must trace to one of layers 1-7. Layer 8 alone is insufficient per the encoding-rules of the India-Legal Corpus Pipeline.

---

## 10. Provenance

This base file was authored fresh in May 2026 with reference only to:
- Public statutory text (CPC, BNSS, Specific Relief Act, Limitation Act, Court-Fees Act, Suits Valuation Act)
- Public-domain procedural textbooks (citation-only, no verbatim copy)
- Cross-validation reports `validation-specific-relief.md`, `validation-plaint-ws.md`, `validation-application.md`, `validation-pleadings.md`, `validation-criminal-pleadings.md` from the India-Legal Corpus Pipeline Phase 03

No drafted prose from any external source has been transcribed into this file. The author certifies compliance with the NOTICE.md doctrine of this plugin.
