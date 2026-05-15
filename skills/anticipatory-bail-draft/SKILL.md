---
name: anticipatory-bail-draft
description: Draft an Application for Anticipatory Bail before a District Court / Sessions Court under Section 482 of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Section 438 of the Code of Criminal Procedure 1973). For a person apprehending arrest who seeks pre-arrest bail. Produces .docx containing the application + supporting Affidavit + List of Documents. Auto-fires on "draft anticipatory bail" or "draft section 482 bail" or "ABA draft" or "pre-arrest bail".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Anticipatory Bail Application — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: APPLICATION FOR ANTICIPATORY BAIL
case_short_code: ABA / ANT. BAIL APPL.
court_designation: |
  Sessions Court (primarily — BNSS Section 482 vests jurisdiction in Sessions
  and High Court). Where the matter is escalated to High Court, the High Court
  plugin's anticipatory-bail-draft skill applies. This skill is the
  District / Sessions variant.
statutory_authority: |
  Section 482 of the Bharatiya Nagarik Suraksha Sanhita, 2023
  (corresponding to Section 438 of the Code of Criminal Procedure, 1973 —
   applicable where the FIR / proceeding is instituted prior to 1 July 2024
   AND under the old code).
jurisprudential_anchors:
  - Gurbaksh Singh Sibbia v. State of Punjab (1980) 2 SCC 565 (foundational — broad approach; anticipatory bail not to be hedged with artificial conditions)
  - Sushila Aggarwal v. State (NCT of Delhi) (2020) 5 SCC 1 (Constitution Bench — anticipatory bail not to be time-limited as a rule; can continue till end of trial)
  - Arnesh Kumar v. State of Bihar (2014) 8 SCC 273 (mandatory checklist for arrest under offences ≤ 7 years)
  - Satender Kumar Antil v. CBI (2022) 10 SCC 51 (consolidated guidelines)
  (Cited as authority; no prose lifted.)
mandatory_paragraphs:
  - particulars_of_applicant: "Name, age, parentage, occupation, address of the applicant."
  - apprehension_of_arrest: |
      Specific facts giving rise to the reasonable apprehension of arrest:
      (i) FIR number + date + police station + sections invoked + complainant
          (where FIR is registered);
      (ii) Notice under Section 35 BNSS (formerly Section 41A CrPC) received,
           with date (where notice is the trigger);
      (iii) Threats of arrest by the police / complainant (with specific facts);
      (iv) Discovery from police records / RTI / complainant's statement that
           arrest is imminent.
  - facts_alleged: |
      Brief summary of the allegations made against the applicant.
      Authored fresh; never lifted verbatim.
  - applicant_innocence_paragraph: |
      Distinct paragraph asserting innocence + brief defence narrative
      (where the applicant can disclose the defence without prejudice).
  - cooperation_paragraph: |
      Statement that the applicant has cooperated / will cooperate with
      the investigation — attending the police station as and when required,
      handing over relevant documents, etc.
  - grounds_for_anticipatory_bail: |
      Each ground is a distinct numbered paragraph. Common grounds:
      (i) The applicant has roots in the community — permanent address,
          family ties, employment, no flight risk;
      (ii) The applicant will not abscond — assurance of presence;
      (iii) The applicant will not tamper with evidence or witnesses;
      (iv) The applicant has no criminal antecedents (where applicable);
      (v) The accusations are false / motivated / fabricated by the
          complainant (with specific facts supporting falsity);
      (vi) The offence(s) alleged are bailable / triable by Magistrate /
           non-violent (where applicable);
      (vii) Custodial interrogation is not necessary — investigation
            material is documentary / electronic and can be procured
            without arrest;
      (viii) Investigative urgency / public interest does not require arrest;
      (ix) The applicant is in poor health / has medical conditions
           (where applicable).
  - undertaking_paragraph: |
      "The Applicant undertakes:
       (a) to make himself available for interrogation by the Investigating
           Officer as and when required;
       (b) not to tamper with evidence or threaten / influence any witness;
       (c) not to leave the local jurisdiction or the country without prior
           permission of this Hon'ble Court;
       (d) to abide by such conditions as this Hon'ble Court may impose."
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to:
   (a) Grant anticipatory bail to the Applicant in connection with
       FIR No. [____] of [Year] registered at [Police Station] under
       Sections [____] of the BNS 2023 (corresponding to Sections [____]
       of the IPC 1860, where applicable) / Sections [____] of the
       [Special Act, where applicable];
   (b) Direct that in the event of arrest, the Applicant be released on
       bail on such conditions as this Hon'ble Court may impose;
   (c) Pass any further order(s) in the interest of justice."
exhibit_order:
  - A: Copy of the FIR (where registered)
  - B: Copy of the Section 35 BNSS notice (where received)
  - C: Documents establishing roots in community
  - D: Documents establishing absence of criminal antecedents (where applicable)
  - E: Documents supporting innocence / defence narrative
  - F: Medical records (where the medical-ground is invoked)
  - G-onwards: case-specific
```

## Hard rules (in addition to those inherited)

1. **Dual citation MANDATORY.** Section 482 BNSS paired with Section 438 CrPC. Substantive sections paired likewise.

2. **Apprehension of arrest must be specifically pleaded.** Vague apprehension is insufficient. The triggering fact (FIR registration / Section 35 notice / police visit / etc.) must be pleaded with date and reference.

3. **Cooperation paragraph is non-negotiable.** Sushila Aggarwal Constitution Bench guidance requires the applicant to demonstrate willingness to cooperate.

4. **Custodial-interrogation-not-required ground** is the load-bearing argument for non-violent / economic / documentary cases per Satender Antil framework.

5. **Undertaking paragraph is mandatory.** 4-condition standard undertaking authored fresh.

6. **Where no FIR is yet registered** (purely apprehended), the application must establish the apprehension on facts other than FIR — police complaint, public threat by complainant, etc.

7. **Drafted prose anti-fabrication.** Every fact traces to `case-facts.md`.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/anticipatory-bail-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Single-citation BNSS / CrPC → fatal.
- Apprehension-of-arrest paragraph absent or vague → halt.
- Cooperation paragraph absent → halt.
- Where FIR is not yet registered: no alternative basis for apprehension pleaded → halt.

## Provenance

- Bharatiya Nagarik Suraksha Sanhita 2023 — Sections 35, 482
- Code of Criminal Procedure 1973 — Sections 41A, 438 (transitional)
- Constitution of India — Article 21
- Gurbaksh Singh Sibbia, Sushila Aggarwal, Arnesh Kumar, Satender Kumar Antil (cited as authority; no prose lifted)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
