---
name: petition-draft
description: Draft a District Court Petition under specific procedural statutes — Succession Petition (Sections 370-381 Indian Succession Act 1925), Probate Petition (Section 276 ISA), Letters of Administration Petition (Section 273 ISA), Election Petition (Section 81 Representation of the People Act 1951), Matrimonial Petition (HMA Section 13-B for Mutual Divorce / other matrimonial reliefs — extends to Family Court companion skill-pack), Guardianship Petition under the Guardians and Wards Act 1890. Produces .docx containing the petition + Schedule of Assets / Statement of Particulars + Verification + supporting Affidavit. Auto-fires on "draft petition" or "draft succession petition" or "draft probate petition" or "draft matrimonial petition" or "letters of administration petition".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# District Court Petition — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: PETITION
petition_subtypes:
  - SUCCESSION_CERTIFICATE     # Sections 370-381 Indian Succession Act 1925
  - PROBATE                    # Sections 222-276 Indian Succession Act 1925
  - LETTERS_OF_ADMINISTRATION  # Sections 218-273 Indian Succession Act 1925
  - GUARDIANSHIP               # Guardians and Wards Act 1890
  - MATRIMONIAL_MUTUAL_DIVORCE # Section 13B Hindu Marriage Act 1955 (Family Court)
  - MATRIMONIAL_DIVORCE_CONTESTED  # Section 13 HMA 1955
  - MATRIMONIAL_JUDICIAL_SEPARATION # Section 10 HMA 1955
  - MATRIMONIAL_RCR            # Section 9 HMA 1955 (Restitution of Conjugal Rights)
  - MAINTENANCE                # Section 125 BNSS (corresponding to Section 125 CrPC) — Family Court / Magistrate
  - INDIGENT_PERMISSION        # Order XXXIII CPC (overlap with Application skill)
  - WINDING_UP                 # Companies Act 2013 (NCLT — separate forum)
  - OTHER_STATUTORY_PETITION   # any petition under a specific statute
court_designation: |
  Varies by petition type:
    · Succession / Probate / Letters of Administration → District Judge with
      ordinary original civil jurisdiction
    · Guardianship → District Court / Family Court
    · Matrimonial → Family Court (where established) OR District Court
    · Maintenance → Family Court OR Judicial Magistrate First Class (BNSS Sec 144)
statutory_authority_per_subtype:
  SUCCESSION_CERTIFICATE: |
    Sections 370 to 381 of the Indian Succession Act, 1925, read with the
    State Notification establishing pecuniary limits for District Judges
    in succession matters.
  PROBATE: |
    Sections 222 to 276 of the Indian Succession Act, 1925, read with the
    rules of the District Court / High Court (Original Side) governing
    grant of probate.
  LETTERS_OF_ADMINISTRATION: |
    Sections 218, 219, 220, 232 to 273 of the Indian Succession Act, 1925.
  GUARDIANSHIP: |
    Guardians and Wards Act, 1890 — Sections 4 (definitions), 7 (appointment),
    8 (procedure), 17 (welfare of minor as paramount consideration).
  MATRIMONIAL_MUTUAL_DIVORCE: |
    Section 13B of the Hindu Marriage Act, 1955 read with the Family Courts
    Act, 1984 and the relevant State Family Court Rules.
  MAINTENANCE: |
    Section 144 of the Bharatiya Nagarik Suraksha Sanhita, 2023 (corresponding
    to Section 125 of the Code of Criminal Procedure, 1973 — applicable where
    the proceeding was instituted prior to 1 July 2024), read with the Hindu
    Adoptions and Maintenance Act 1956 / personal-law provisions.
mandatory_paragraphs_per_subtype:
  SUCCESSION_CERTIFICATE:
    - particulars_of_deceased: "Name, last residence, date and place of death — Section 372 ISA."
    - relationship: "Petitioner's relationship to the deceased."
    - schedule_of_debts_and_securities: "MANDATORY — list of debts and securities for which the certificate is sought, with values."
    - other_heirs: "Names and addresses of other heirs entitled, if any."
    - no_prior_application: "No prior application for succession certificate has been made by any other heir (where applicable)."
  PROBATE:
    - particulars_of_testator: "Name, last residence, date and place of death."
    - particulars_of_will: "Date of execution of will, attestation, witnesses."
    - executors: "Name(s) of executor(s) appointed in the will."
    - schedule_of_assets: "MANDATORY — full inventory of the deceased's assets covered by the will."
    - other_persons_entitled: "Names and addresses of all persons entitled to take under the will."
  MATRIMONIAL_MUTUAL_DIVORCE:
    - marriage_particulars: "Date of marriage, place of marriage, marriage certificate / registration."
    - separation_period: "Statement that the parties have been living separately for one year or more (Section 13B(1) HMA)."
    - mutual_consent: "Statement of mutual consent to dissolve the marriage."
    - issue_arrangements: "Statement of arrangements for any children (custody, maintenance, education)."
    - alimony_arrangements: "Statement of any settlement on alimony / streedhan / property."
mandatory_supporting_affidavit: yes  # all petitions require supporting Affidavit
prayer_pattern_per_subtype:
  SUCCESSION_CERTIFICATE: "Grant a Succession Certificate to the Petitioner in respect of the debts and securities specified in the Schedule attached hereto."
  PROBATE: "Grant Probate of the last Will and Testament of the deceased [Name], dated <DD.MM.YYYY>, to the Petitioner-Executor named therein."
  MATRIMONIAL_MUTUAL_DIVORCE: "Pass a decree of divorce by mutual consent under Section 13B of the Hindu Marriage Act, 1955 dissolving the marriage between the Petitioners solemnized on <DD.MM.YYYY> at <Place>."
exhibit_order:
  SUCCESSION_CERTIFICATE:
    - A: Death certificate of deceased
    - B: Petitioner's identity / heirship proof
    - C: Documents evidencing the debts and securities (bank passbook copies, share certificates, fixed deposit receipts)
  PROBATE:
    - A: The original will (or attested true copy where original is with the District Registry)
    - B: Death certificate of testator
    - C: Affidavits of attesting witnesses (where applicable)
    - D: Inventory documents for assets
  MATRIMONIAL_MUTUAL_DIVORCE:
    - A: Marriage certificate / proof of marriage
    - B: Aadhaar / identity proof of both Petitioners
    - C: Memorandum of Understanding on alimony / property settlement (where executed)
    - D: Affidavits of both Petitioners affirming mutual consent
```

## Hard rules (in addition to those inherited)

1. **Petition heading must cite the specific statutory provision.** Generic "Petition" without the statute citation will be returned by the Office.

2. **Schedule blocks are mandatory.** Succession Certificate requires Schedule of Debts and Securities; Probate requires Schedule of Assets; Matrimonial Mutual Divorce requires Schedule of Issues (children, alimony) if applicable.

3. **Court Designation must match jurisdiction.** District Court Petitions for Succession / Probate / LoA — District Judge. Matrimonial — Family Court (where established). Drafter verifies via the user's case folder.

4. **Limitation rules differ per statute.** Probate has no fixed limitation but the Court applies the laches doctrine. Succession Certificate similarly. Election Petition has 45-day strict limitation under Section 81(1) RP Act 1951. The Drafter uses the petition-subtype-specific rule.

5. **Mutual Consent Petitions: 6-month cooling-off period.** Section 13B(2) HMA 1955 requires a second motion after 6 months and within 18 months of the first motion. The Drafter inserts the placeholder framework for both motions.

6. **Drafted prose anti-fabrication.** Every fact, every relationship averment, every property description traces to `case-facts.md`. No corpus prose.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/petition-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Subtype-specific mandatory paragraph absent → halt.
- Schedule of Debts / Assets / Issues absent → halt.
- Court designation mismatch → flag for user clarification.
- Mutual Divorce: second motion drafted before 6-month cooling period → flag.

## Provenance

- Indian Succession Act 1925 — Sections 218, 222, 273, 276, 370-381
- Guardians and Wards Act 1890 — Sections 4, 7, 8, 17
- Hindu Marriage Act 1955 — Sections 9, 10, 13, 13B
- Family Courts Act 1984
- Representation of the People Act 1951 — Section 81
- Bharatiya Nagarik Suraksha Sanhita 2023 — Section 144 (corresponding to CrPC Section 125)
- Cross-validation report `validation-petition.md` (Phase 03)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
