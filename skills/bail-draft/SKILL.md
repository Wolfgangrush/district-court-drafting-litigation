---
name: bail-draft
description: Draft a Regular Bail Application before a District Court / Sessions Court under Section 483 of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Section 439 of the Code of Criminal Procedure 1973). For an accused already in custody seeking release on bail. Produces .docx containing the application + supporting Affidavit + List of Documents. Auto-fires on "draft bail application" or "draft regular bail" or "draft section 483 bail" or "bail application".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Regular Bail Application — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: APPLICATION FOR REGULAR BAIL
case_short_code: BA / BAIL APPL.
court_designation: |
  Sessions Court (for offences triable by Sessions / where bail is committed
  to Sessions discretion under BNSS Section 483(1)(a))
  OR Court of Magistrate (for offences within Magistrate's bail jurisdiction
  per BNSS Section 480 / 483)
  OR (where applicable) the Court where the trial / committal proceeding is pending
statutory_authority: |
  Section 483 of the Bharatiya Nagarik Suraksha Sanhita, 2023
  (corresponding to Section 439 of the Code of Criminal Procedure, 1973 —
   applicable where the FIR / proceeding was instituted prior to 1 July 2024).
  Read with:
    · Section 480 BNSS / Section 436 CrPC — bail in bailable offences
    · Section 481 BNSS / Section 441 CrPC — bond and surety
    · Section 482 BNSS / Section 438 CrPC — anticipatory bail (separate skill)
    · Article 21 of the Constitution of India — personal liberty
jurisprudential_anchors:
  - Gurbaksh Singh Sibbia v. State of Punjab (1980) 2 SCC 565 (anticipatory bail; cited for the broad approach to bail jurisdiction)
  - Sanjay Chandra v. CBI (2012) 1 SCC 40 (bail is the rule, jail is the exception in non-violent economic offences)
  - Arnesh Kumar v. State of Bihar (2014) 8 SCC 273 (mandatory checklist for arrest under offences ≤ 7 years)
  - Satender Kumar Antil v. CBI (2022) 10 SCC 51 (consolidated guidelines on bail across offence categories)
  (Cited as authority; no prose lifted.)
mandatory_paragraphs:
  - particulars_of_applicant: "Name, age, parentage, occupation, address of the accused-applicant."
  - custody_details: |
      Date of arrest, place of arrest, the police station where the FIR is
      registered, the FIR number and date, the sections invoked, the period
      of custody as on the date of the application, and the present place of
      custody (judicial custody / jail / hospital, etc.).
  - fir_details: |
      FIR number + date + police station + sections invoked + the complainant
      (informant), with the dual-citation pattern.
  - facts_alleged: |
      Brief summary of the prosecution case as set out in the FIR / charge-sheet
      (where filed). Authored fresh; never lifted from the FIR text directly.
  - grounds_for_bail: |
      Each ground is a distinct numbered paragraph. Common grounds:
      (i) The applicant is innocent and has been falsely implicated;
      (ii) The applicant has been in custody for [N] days / months, and
           investigation is complete / nearly complete / charge-sheet has been
           filed (where applicable);
      (iii) The applicant is not a flight risk — permanent address, family ties,
            roots in the community;
      (iv) The applicant will not tamper with evidence or influence witnesses;
      (v) The applicant has no criminal antecedents (where applicable);
      (vi) The applicant is in poor health / has medical conditions / etc.
           (where applicable, supported by medical records);
      (vii) The role attributed to the applicant is peripheral / overstated /
            inconsistent with the documentary record;
      (viii) Co-accused on similar / lesser footing has been released on bail
             — parity argument (where applicable);
      (ix) The offence is bailable / non-violent / economic / triable by
           Magistrate (where applicable per Satender Antil framework).
  - undertaking_paragraph: |
      "The applicant undertakes to attend the Trial Court on every date of
       hearing, not to leave the local jurisdiction without prior permission
       of the Court, not to tamper with evidence or threaten / influence any
       witness, and to abide by such other conditions as this Hon'ble Court
       may impose."
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to:
   (a) Enlarge the Applicant on regular bail in FIR No. [____] of [Year]
       registered at [Police Station] under Sections [____] of the BNS 2023
       (corresponding to Sections [____] of the IPC 1860, where applicable) /
       Sections [____] of the [Special Act, where applicable];
   (b) Impose such conditions as this Hon'ble Court deems fit;
   (c) Pass any further order(s) in the interest of justice."
exhibit_order:
  - A: Copy of the FIR
  - B: Copy of the arrest memo
  - C: Copy of the charge-sheet (where filed)
  - D: Medical records (where the medical-ground is invoked)
  - E: Documents establishing roots in community (Aadhaar, voter ID, ration card, residence proof)
  - F: Documents establishing co-accused parity (bail orders of co-accused, where applicable)
  - G-onwards: case-specific
```

## Hard rules (in addition to those inherited)

1. **Dual citation MANDATORY.** Section 483 BNSS paired with Section 439 CrPC. Substantive sections (BNS / IPC) paired likewise.

2. **Custody details are non-negotiable.** Without precise date of arrest, place of custody, period in custody, the bail application is liable to be remitted for re-filing.

3. **FIR details are non-negotiable.** FIR number + date + police station + sections — placeholders until the case folder supplies actuals.

4. **Grounds for bail must be specific.** Generic "bail is the rule" recitation alone is insufficient. Each ground is a distinct paragraph tied to facts in `case-facts.md`.

5. **Parity argument requires the cited order.** Where co-accused parity is invoked, the bail order of the co-accused must be filed as Exhibit F.

6. **Undertaking paragraph is mandatory.** Standard 4-condition undertaking authored fresh.

7. **No tampering with FIR text.** The summary of prosecution case is authored fresh — never copies FIR language verbatim. (Copying FIR language risks an Order VI Rule 2 analogue — pleading evidence instead of facts.)

8. **Drafted prose anti-fabrication.** Every fact, every custody detail, every date traces to `case-facts.md`.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/bail-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Single-citation BNSS / CrPC → fatal.
- Custody details absent → halt.
- FIR details absent → halt.
- Grounds for bail not specifically pleaded → halt.

## Provenance

- Bharatiya Nagarik Suraksha Sanhita 2023 — Sections 480, 481, 482, 483
- Code of Criminal Procedure 1973 — Sections 436, 438, 439, 441 (transitional)
- Constitution of India — Article 21
- Gurbaksh Singh Sibbia, Sanjay Chandra, Arnesh Kumar, Satender Kumar Antil (cited as authority; no prose lifted)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
