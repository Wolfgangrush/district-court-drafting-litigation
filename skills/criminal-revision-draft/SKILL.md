---
name: criminal-revision-draft
description: Draft a Criminal Revision Application before a Sessions Court under Sections 438 and 442 of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Sections 397 and 401 of the Code of Criminal Procedure 1973). For challenging an order of a subordinate criminal court on grounds of illegality, irregularity, or impropriety in the exercise of jurisdiction. Produces .docx containing the revision + supporting Affidavit + List of Documents. Auto-fires on "draft criminal revision" or "draft revision application" or "section 438 bnss revision" or "criminal revision".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Criminal Revision Application — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: CRIMINAL REVISION APPLICATION
case_short_code: CRA / CRIM. REV.
court_designation: |
  Sessions Court (BNSS Section 438) — for revisions against orders of
  Magistrates and other subordinate criminal courts.
  Alternative: High Court (BNSS Section 442) — concurrent revisional
  jurisdiction; this District plugin's skill handles the Sessions Court
  variant. High Court revisions would route to the High Court plugin.
statutory_authority: |
  Sections 438 and 442 of the Bharatiya Nagarik Suraksha Sanhita, 2023
  (corresponding to Sections 397 and 401 of the Code of Criminal Procedure,
   1973 — applicable where the proceeding was instituted prior to 1 July 2024).
  Note: Where the order under revision is interlocutory (BNSS Section 438(2) /
  CrPC Section 397(2)), revision does NOT lie. The Drafter halts and prompts
  the user to consider an inherent-powers route or wait until final order.
scope_of_revision: |
  BNSS Section 438 / CrPC Section 397 — the revisional court may call for
  and examine the record of any proceeding before any inferior criminal court
  to satisfy itself as to the correctness, legality, propriety, or regularity
  of any finding, sentence, or order. The court may:
    · Confirm
    · Modify
    · Reverse
    · Direct re-hearing / re-investigation
  Revision is NOT a full appeal — limited to errors of jurisdiction, illegality,
  irregularity, propriety. (Munna Devi v. State of Rajasthan (2001) 9 SCC 631
  cited as authority; no prose lifted.)
limitation: |
  Limitation Act 1963, Article 131 — 90 days from the date of the order under
  revision. Condonation of Delay under Section 5 Limitation Act available
  with sufficient cause.
mandatory_paragraphs:
  - particulars_of_applicant: "Name, age, parentage, occupation, address of the applicant (accused / complainant / aggrieved third party as applicable)."
  - particulars_of_impugned_order: |
      Court that passed the order + case number + date + the operative part
      of the order. A certified copy of the order is mandatory Exhibit A.
  - facts_chronology: |
      Brief chronology of the underlying criminal proceeding leading to the
      impugned order. Authored fresh.
  - grounds_for_revision: |
      Each ground identifies a specific error of jurisdiction / illegality /
      irregularity / impropriety. Common grounds:
      (i) The subordinate court exercised jurisdiction not vested by law;
      (ii) The subordinate court failed to exercise jurisdiction vested by law;
      (iii) The subordinate court acted in the exercise of jurisdiction
            illegally or with material irregularity;
      (iv) The order is contrary to settled law on the point;
      (v) The order is contrary to the evidence on record;
      (vi) The order results in manifest injustice / abuse of process;
      (vii) Sentencing error (where the order is a sentencing order);
      (viii) Cognisance / process / committal error (where applicable).
  - interlocutory_order_disclaimer: |
      Statement (where applicable) that the order under revision is NOT an
      interlocutory order within the meaning of BNSS Section 438(2) /
      CrPC Section 397(2). (If the order IS interlocutory, the skill halts.)
  - no_other_remedy_paragraph: |
      Statement that no other equally efficacious remedy by way of appeal is
      available against the impugned order (where applicable). Where appeal
      IS available but not chosen, the Drafter inserts a brief explanation
      paragraph addressing the choice.
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to:
   (a) Call for the record of [Case Number] from the Court of [Subordinate
       Court Designation] at [Place];
   (b) Set aside / modify / reverse the impugned order dated <DD.MM.YYYY>;
   (c) Pass such further order(s) as this Hon'ble Court may deem fit and
       proper in the interest of justice and to prevent abuse of the
       process of law."
accompanying_applications:
  - application_for_stay_of_impugned_order        # pending disposal of the revision
  - application_for_condonation_of_delay          # where filed beyond 90 days
  - application_for_exemption_from_filing_certified_copy
exhibit_order:
  - A: Certified copy of the impugned order
  - B: Copy of the pleadings before the subordinate court (where relevant)
  - C: Material from the record establishing the alleged error
  - D-onwards: case-specific
```

## Hard rules (in addition to those inherited)

1. **Dual citation MANDATORY.** Sections 438 / 442 BNSS paired with Sections 397 / 401 CrPC.

2. **Interlocutory-order disclaimer.** BNSS Section 438(2) bars revision against interlocutory orders. The Drafter checks the nature of the impugned order and halts if it appears interlocutory; user must confirm before proceeding.

3. **90-day limitation.** Where filed beyond, Condonation of Delay application auto-included.

4. **Certified copy of impugned order is mandatory Exhibit A.** No exception.

5. **Grounds for revision must identify a specific error.** Revision is NOT an appeal — pleading "the order is wrong" alone is insufficient. The error must be one of jurisdiction / illegality / irregularity / impropriety.

6. **Sentencing revisions are bounded.** Where the revision challenges sentencing, the ground must show the sentence is contrary to the statutory scheme / mandatory minimum / proportionality (Bachan Singh, Mithu, etc., cited as authority where invoked).

7. **No re-appreciation of evidence.** Revision does not permit fresh appreciation of evidence — the ground must be one of legal error visible on the face of the record.

8. **Drafted prose anti-fabrication.** Every fact, every reference, every date traces to `case-facts.md`.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/criminal-revision-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Single-citation BNSS / CrPC → fatal.
- Impugned order is interlocutory → halt unless user explicitly confirms revision lies.
- Certified copy absent → halt.
- Grounds frame the revision as merit re-hearing → flag.

## Provenance

- Bharatiya Nagarik Suraksha Sanhita 2023 — Sections 438, 442
- Code of Criminal Procedure 1973 — Sections 397, 401 (transitional)
- Limitation Act 1963 — Article 131
- Munna Devi v. State of Rajasthan (2001) 9 SCC 631 (cited as authority on scope of revision; no prose lifted)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
