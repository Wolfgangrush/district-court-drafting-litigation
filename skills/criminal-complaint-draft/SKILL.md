---
name: criminal-complaint-draft
description: Draft a Private Criminal Complaint before a Magistrate under Section 223 of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Section 200 of the Code of Criminal Procedure 1973). Used where the complainant prosecutes a cognisable or non-cognisable offence directly before a Magistrate without going through the police. Produces .docx containing the complaint + Statement of Witnesses (Section 225 BNSS / 202 CrPC) skeleton + List of Documents + supporting Affidavit. Auto-fires on "draft criminal complaint" or "draft private complaint" or "draft section 223 bnss complaint" or "magistrate complaint".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Private Criminal Complaint — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: CRIMINAL COMPLAINT (PRIVATE)
case_short_code: COMP / R.C.C. / S.C.C. (per State)
court_designation: |
  Judicial Magistrate First Class (for offences triable by Magistrate)
  OR Chief Judicial Magistrate (where the offence requires CJM cognisance)
  OR Special Court (where the offence is covered by a special statute — POCSO,
  NDPS, PMLA, etc., user-supplied)
statutory_authority: |
  Section 223 of the Bharatiya Nagarik Suraksha Sanhita, 2023
  (corresponding to Section 200 of the Code of Criminal Procedure, 1973 —
   applicable where the complaint is instituted in respect of an offence
   alleged to have been committed prior to 1 July 2024 AND the proceeding
   was instituted under the old code).
post_cognisance_procedure: |
  Section 225 BNSS (corresponding to Section 202 CrPC) — postponement of
  issue of process for inquiry / direction for investigation.
  Section 226 BNSS (corresponding to Section 203 CrPC) — dismissal of
  complaint if no sufficient ground for proceeding.
  Section 227 BNSS (corresponding to Section 204 CrPC) — issue of process
  if sufficient ground.
limitation_basis: |
  Limitation under Sections 514 to 519 of the BNSS 2023 (corresponding to
  Sections 467 to 472 of the CrPC 1973). Limitation period depends on the
  punishment prescribed:
    · Offences punishable with fine only — 6 months
    · Offences punishable with imprisonment up to 1 year — 1 year
    · Offences punishable with imprisonment exceeding 1 year but up to 3 years — 3 years
    · Offences punishable with imprisonment exceeding 3 years — no limitation
  Drafter computes from the offences identified in `case-facts.md`.
mandatory_paragraphs:
  - particulars_of_complainant: |
      Name, age, occupation, address of the complainant. Where the complainant
      is a public servant acting in official capacity, name and designation.
  - particulars_of_accused: |
      Name, parentage, age, occupation, address of each accused. Where the
      accused is unknown, description sufficient to identify on apprehension.
  - date_and_place_of_occurrence: |
      Specific date and place where the offence is alleged to have occurred.
      (Note: NOT "cause of action" — criminal pleadings use "date and place
       of occurrence" per BNSS terminology.)
  - facts_constituting_the_offence: |
      Chronological narration of the facts that constitute the offence. Each
      fact paragraph numbered. Each ingredient of the alleged offence
      explicitly mapped to a specific fact.
  - ingredients_of_offence: |
      Distinct paragraph mapping the facts pleaded to each ingredient of each
      offence alleged. (Example: for an offence under Section 318 BNS 2023
      (cheating, corresponding to Section 420 IPC), the ingredients of
      deception + dishonest inducement + delivery of property are each
      mapped to specific facts pleaded.)
  - sections_invoked: |
      Specific sections of BNS 2023 (with parenthetical IPC 1860 fallback
      where the offence is registered before 1 July 2024 or the conduct
      occurred under the old code). Dual citation pattern mandatory.
  - list_of_witnesses: |
      Names and addresses of witnesses whose statements the complainant
      proposes to be recorded under Section 225 BNSS. Each witness's
      brief description of evidence stated.
  - prayer: |
      "Take cognisance of the offences alleged + record the complainant's
       statement + record statements of witnesses + issue process to the
       accused under Section 227 BNSS."
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to:
   (a) Take cognisance of the offences alleged in this complaint;
   (b) Record the statement of the Complainant on solemn affirmation;
   (c) Record the statements of the witnesses named in the list filed
       herewith under Section 225 of the BNSS 2023;
   (d) Issue process to the Accused / Respondent under Section 227 of
       the BNSS 2023 to answer the charge;
   (e) Pass any further order(s) in the interest of justice."
exhibit_order:
  - A: Documents constituting the principal evidence of the offence (cheque returned, agreement breached, FIR if any, etc.)
  - B: Correspondence between the parties relevant to the offence
  - C: Identity / official-capacity proof of the complainant (where relevant)
  - D-onwards: case-specific documents
```

## Hard rules (in addition to those inherited)

1. **Dual citation is MANDATORY.** Every BNSS section paired with the corresponding CrPC section (with the "applicable where the offence is registered before 1 July 2024" qualifier). Every BNS section paired with the corresponding IPC section. Every BSA section paired with the corresponding IEA section.

2. **Date and place of occurrence is mandatory** — not "cause of action". Criminal pleadings use the criminal-procedure terminology.

3. **Ingredients of offence must be mapped to facts.** A complaint that pleads facts but does not map them to the ingredients of the alleged offence(s) is liable to dismissal under Section 226 BNSS.

4. **Limitation discipline.** Drafter computes limitation per Sections 514-519 BNSS based on punishment of alleged offence(s). Where the offence is punishable by imprisonment exceeding 3 years, no limitation applies; this is stated explicitly.

5. **List of Witnesses is mandatory** for Section 225 inquiry. Each witness's brief description of evidence is noted (not the evidence itself — just the topic).

6. **No FIR-bypass framing.** The complaint stands on its own; it does not require an FIR. Where an FIR has been refused by the police, the complaint should narrate that fact + the date(s) the complainant approached the police.

7. **Special Court routing.** If the offence is covered by a special statute (POCSO, NDPS, PMLA, etc.), the Drafter routes the complaint to the Special Court instead of the Magistrate. The user-supplied statute PDF triggers this routing.

8. **Drafted prose anti-fabrication.** Every fact, every date, every party detail, every witness identification traces to `case-facts.md`. No corpus prose.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/criminal-complaint-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Single-citation (BNSS without CrPC pair, or vice versa) → fatal · halt.
- Ingredients of offence not mapped to facts → halt · risk of Section 226 dismissal.
- Date and place of occurrence absent → halt.
- Limitation absent → halt.
- List of Witnesses absent (where Section 225 inquiry is anticipated) → halt.

## Provenance

- Bharatiya Nagarik Suraksha Sanhita 2023 — Sections 223, 225, 226, 227, 514-519
- Code of Criminal Procedure 1973 — Sections 200, 202, 203, 204, 467-472 (transitional)
- Bharatiya Nyaya Sanhita 2023 / Indian Penal Code 1860 (substantive offences, transitional)
- Bharatiya Sakshya Adhiniyam 2023 / Indian Evidence Act 1872 (evidence, transitional)
- Cross-validation report `validation-criminal-pleadings.md` (Phase 03)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
