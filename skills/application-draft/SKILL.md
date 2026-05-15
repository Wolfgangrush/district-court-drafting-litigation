---
name: application-draft
description: Draft a Civil Interlocutory Application before a District Court — Application for Temporary Injunction (Order XXXIX Rules 1 and 2 CPC), Application for Restoration (Order IX Rules 4, 9, 13 CPC), Application for Condonation of Delay (Section 5 Limitation Act 1963), Application for Amendment of Pleadings (Order VI Rule 17 CPC), Application for Appointment of Commission (Order XXVI CPC), Application under Section 151 CPC (inherent powers — fallback). Produces .docx containing the application + supporting Affidavit + Prayer + (where applicable) Statement of Prima Facie Case / Balance of Convenience / Irreparable Loss. Auto-fires on "draft application" or "draft interlocutory" or "draft IA" or "civil application".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Civil Interlocutory Application — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: CIVIL APPLICATION
application_subtypes:
  - TEMPORARY_INJUNCTION       # Order XXXIX Rules 1 and 2 CPC
  - AD_INTERIM_INJUNCTION      # ex parte interim under Order XXXIX Rule 3 proviso
  - RESTORATION                # Order IX Rules 4 (ex parte decree) / 9 (dismissal for default) / 13 (decree set aside)
  - CONDONATION_OF_DELAY       # Section 5 Limitation Act 1963
  - AMENDMENT_OF_PLEADINGS     # Order VI Rule 17 CPC
  - APPOINTMENT_OF_COMMISSION  # Order XXVI Rules 9 and 10 CPC (local investigation)
  - APPOINTMENT_OF_RECEIVER    # Order XL CPC
  - STAY                       # general stay application
  - INHERENT_POWERS            # Section 151 CPC (fallback)
  - PRODUCTION_OF_DOCUMENT     # Order XI CPC
  - EXAMINATION_OF_WITNESS     # Order XVIII Rule 4 + Order XXVI CPC
  - WITHDRAWAL_OF_SUIT         # Order XXIII Rule 1 CPC
  - LEAVE_TO_SUE_AS_INDIGENT   # Order XXXIII CPC
court_designation: |
  Same court that is seised of the underlying suit / proceeding.
statutory_authority_template: |
  APPLICATION UNDER [SPECIFIC ORDER / RULE / SECTION] OF THE CODE OF CIVIL
  PROCEDURE, 1908 (or LIMITATION ACT, 1963 — for Sec 5 applications) FILED
  IN [Suit / Case Type] No. _____ OF [Year]
mandatory_paragraphs_by_subtype:
  TEMPORARY_INJUNCTION:
    - prima_facie_case: "Distinct numbered paragraph establishing the applicant's prima facie case (the substantive right and the apparent merit of the underlying claim)."
    - balance_of_convenience: "Distinct numbered paragraph showing that the balance of convenience favours grant of the injunction."
    - irreparable_loss: "Distinct numbered paragraph showing that, in the absence of the injunction, the applicant will suffer loss that cannot adequately be compensated in damages."
    - supporting_affidavit: "Mandatory Affidavit per Order XXXIX Rule 1 read with Order XIX Rule 3 CPC."
  CONDONATION_OF_DELAY:
    - sufficient_cause: "Distinct paragraph(s) explaining the cause for the delay, with specific dates and events — authored fresh from `case-facts.md`."
    - bona_fides: "Statement that the delay was not deliberate or due to want of bona fides."
    - prejudice_absence: "Statement that the opposing party will not be prejudiced by the condonation (where relevant)."
  RESTORATION:
    - sufficient_cause_for_default: "Distinct paragraph explaining the cause for the prior default (illness, communication failure, miscommunication of date, etc.) — authored fresh."
    - readiness_to_proceed: "Statement of the applicant's readiness and willingness to proceed expeditiously upon restoration."
  AMENDMENT_OF_PLEADINGS:
    - necessity_for_amendment: "Distinct paragraph explaining why the amendment is necessary for the just determination of the real controversy between the parties (per Order VI Rule 17 second proviso)."
    - no_change_of_cause_of_action: "Statement (where applicable) that the amendment does not introduce a new cause of action or a time-barred claim."
mandatory_supporting_affidavit: |
  Most interlocutory applications under Order XXXIX, Order XL, and discretionary
  applications are MANDATORY-AFFIDAVIT — the application is accompanied by an
  Affidavit sworn by the applicant verifying the contents.
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to
  [SPECIFIC RELIEF] in the interest of justice and equity."
  + catchall.
exhibit_order:
  - A: The order / decree being challenged / restored / amended (where applicable)
  - B: Documents establishing the prima facie case
  - C: Documents establishing the balance of convenience / sufficient cause
  - D-onwards: case-specific documents
```

## Hard rules (in addition to those inherited)

1. **Order / Rule / Section citation is mandatory in the application heading.** Generic "civil application" framing without the specific CPC citation will be rejected at the District Court Office.

2. **Temporary Injunction triad is non-negotiable.** Prima facie case + balance of convenience + irreparable loss are the three Dalpat Kumar v. Prahlad Singh (1992) 1 SCC 719 limbs (citation noted as authority; not quoted). All three must appear as distinct numbered paragraphs.

3. **Supporting Affidavit is mandatory for discretionary applications.** Order XXXIX, Order XL, Section 151 — supporting Affidavit per Order XIX Rule 3 CPC.

4. **Section 151 CPC is a FALLBACK, not a default.** The skill flags applications drafted under Section 151 alone and prompts the Drafter to check whether a specific Order / Rule applies first.

5. **Drafted prose anti-fabrication.** Every cause-of-delay narrative, every "balance of convenience" averment, every "prima facie case" assertion is authored fresh from `case-facts.md`. No corpus prose.

6. **Verification language is verbatim CPC Order XIX Rule 3** for the supporting Affidavit. The deponent's name, address, occupation are placeholders.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/application-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Order / Rule / Section citation absent in heading → Office rejection risk · halt.
- Temporary Injunction missing any of the three limbs → fatal · halt.
- Supporting Affidavit absent where mandatory → halt.
- Sufficient cause averment absent in Condonation / Restoration application → halt.

## Provenance

- Code of Civil Procedure 1908 — Orders VI, IX, XI, XVIII, XIX, XXIII, XXVI, XXXIII, XXXIX, XL; Sections 148A, 151
- Limitation Act 1963 — Section 5
- Indian Evidence Act 1872 / Bharatiya Sakshya Adhiniyam 2023 (Affidavit verification, transitional)
- Cross-validation report `validation-application.md` (Phase 03)
- Dalpat Kumar v. Prahlad Singh (1992) 1 SCC 719 (cited as authority for the three-limb injunction test; no prose lifted)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
