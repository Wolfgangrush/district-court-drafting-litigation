---
name: plaint-ws-draft
description: Draft a Plaint (Order VII CPC) or a Written Statement (Order VIII CPC) before a District Court — the core district-court civil pleading. Handles generic civil suits (suits for money, suits for recovery, suits for declaration of title, suits for partition, suits for damages, etc.). Produces .docx containing the pleading + Verification + Court-Fee block + List of Documents + (Plaint) Schedule of Property where applicable + (WS) paragraph-wise traversal. Auto-fires on "draft plaint" or "draft written statement" or "draft ws" or "plaint draft" or "ws draft".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Plaint / Written Statement — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: PLAINT (or WRITTEN STATEMENT)
case_short_codes:
  - PLAINT                     # CPC Order VII
  - WRITTEN STATEMENT          # CPC Order VIII
  - COMMERCIAL SUIT PLAINT     # Commercial Courts Act 2015 — additional Statement of Truth
  - COMMERCIAL SUIT WS         # Commercial Courts Act 2015 — additional Statement of Truth
court_designation: |
  Civil Judge, Senior Division / Junior Division (based on pecuniary value)
  OR District Judge for matters exceeding Civil Judge pecuniary limit
  OR Commercial Court of competent jurisdiction (for Commercial Suits per
     the Commercial Courts Act 2015)
statutory_authority: |
  Code of Civil Procedure, 1908 — primarily:
    · Order VI (Pleadings generally) — Rules 1, 2, 14, 15, 17
    · Order VII (Plaint) — Rules 1, 2, 3, 6, 7, 9, 10, 11, 14
    · Order VIII (Written Statement and Set-off) — Rules 1, 1A, 2, 3, 4, 5, 6, 6A, 7, 9, 10
    · Order XIX (Affidavits) — Rule 3
  Commercial Courts Act, 2015 — for Commercial Suits:
    · Section 2(1)(c) (definition of "Commercial Dispute")
    · CPC Order VI Rule 15A and Order XI (amended for Commercial Suits)
    · Statement of Truth in lieu of standard verification
limitation_basis: |
  Limitation Act 1963, Schedule. Specific Article depends on the cause:
    · Money suits — Article 19 / 22 / 23 / 24 (depending on the underlying transaction)
    · Suits on contracts — Article 55
    · Suits for declaration of title — Article 58 (3 years)
    · Suits for possession — Article 65 (12 years from adverse possession)
    · Suits for partition — Article 110 (12 years)
    · Suits for damages for breach of contract — Article 55
  Drafter computes from `case-facts.md` Section 4.
court_fee_basis: |
  State Court-Fees Act. Ad valorem on the suit value, except where the State
  Schedule fixes a specific fee for the suit type. Drafter cites the specific
  Schedule entry from `format-from-user.md` Section 7 (user's State Court-Fees Act).
mandatory_paragraphs_plaint:
  - cause_of_action: |
      Order VII Rule 1(e) CPC — distinct numbered paragraph stating when
      and where the cause of action arose. Without this, the plaint is
      liable to be rejected under Order VII Rule 11(a).
  - jurisdiction: |
      Order VII Rule 1(f), (g) CPC — distinct paragraph stating the basis
      of territorial and pecuniary jurisdiction.
  - valuation: |
      Order VII Rule 1(i) CPC — distinct paragraph stating the value of
      the suit for the purpose of court-fee and jurisdiction.
  - statement_of_truth: |
      For Commercial Suits (Commercial Courts Act 2015) — mandatory Statement
      of Truth in the form specified by Order VI Rule 15A CPC (as amended).
mandatory_paragraphs_ws:
  - paragraph_wise_traversal: |
      Order VIII Rule 3 CPC — every paragraph of the plaint must be
      specifically denied or admitted; evasive denial is treated as
      admission (Order VIII Rule 5).
  - deemed_admission_caution: |
      Drafter inserts a placeholder note: facts not specifically denied
      are deemed to be admitted. WS must therefore traverse every plaint
      paragraph.
  - additional_pleas: |
      Set-off (Order VIII Rule 6) / Counter-claim (Order VIII Rule 6A)
      where applicable. Filed within the WS or as a separate document.
accompanying_applications_plaint:
  - application_for_temporary_injunction          # Order XXXIX Rules 1 and 2 CPC
  - application_for_court_receiver                # Order XL CPC
  - application_for_local_commission              # Order XXVI Rules 9 and 10 CPC
  - application_for_condonation_of_delay          # Section 5 Limitation Act
  - application_for_leave_to_sue_as_indigent      # Order XXXIII CPC
  - application_for_amendment                     # Order VI Rule 17 CPC
accompanying_applications_ws:
  - application_for_extension_of_time             # for filing WS where filed beyond 30/90 days
  - application_for_counter_claim                 # where pleaded separately
typical_exhibit_order_plaint:
  - A: The principal document evidencing the claim (agreement / promissory note / receipt / etc.)
  - B: Title documents (where applicable)
  - C: Legal notice issued by the plaintiff
  - D: Reply to the legal notice (if received)
  - E: Documents establishing damage / loss / breach
  - F-onwards: case-specific documents
typical_exhibit_order_ws:
  - A: Documents the defendant relies upon to deny the plaint's claim
  - B: Documents establishing the defendant's own claim (set-off / counter-claim)
  - C-onwards: case-specific
```

## Section-by-section structure

The universal section order from `_district_pleading_base` applies. Plaint vs WS branches:

### PLAINT branch

| # | Section | Plaint specifics |
|---|---|---|
| 3 | Statement of Facts | Numbered paragraphs · each paragraph contains material facts only · NO evidence (per Order VI Rule 2). |
| 4 | Particulars of Claim | Cause-of-action paragraph (mandatory per Order VII Rule 1(e)). |
| 5 | Jurisdiction | Territorial + pecuniary + subject-matter (per Order VII Rule 1(f), (g)). |
| 6 | Limitation | Article of the Schedule to the Limitation Act 1963 cited. |
| 7 | Court-Fee | Suit value + State Court-Fees Act schedule citation. |
| 8 | Prayer | Specific relief enumerated (a), (b), (c) ... + catchall. |
| 9 | Verification | CPC Order VI Rule 15 verbatim (OR Statement of Truth per Order VI Rule 15A for Commercial Suits). |

### WRITTEN STATEMENT branch

| # | Section | WS specifics |
|---|---|---|
| 3 | Preliminary Objections | Maintainability · limitation · jurisdiction objections (if any). |
| 4 | Paragraph-wise Traversal | "With reference to paragraph [X] of the plaint, the contents thereof are denied / admitted / not denied to the extent that..." — every plaint paragraph addressed. |
| 5 | Defendant's Version of Facts | Defendant's affirmative case (where the defendant denies + asserts an alternative version). |
| 6 | Set-off / Counter-claim | Where pleaded — drafted as separate sub-section or accompanying document. |
| 7 | Prayer (for WS) | "The Defendant prays that the suit may be dismissed with costs." |
| 8 | Verification | CPC Order VI Rule 15 verbatim. |

## Hard rules (in addition to those inherited)

1. **PLAINT: Cause of Action paragraph is mandatory.** Order VII Rule 11(a) provides for rejection of plaint where no cause of action is disclosed. The Drafter inserts a distinct numbered paragraph stating *when* and *where* the cause of action arose.

2. **PLAINT: Material facts only.** Order VI Rule 2(1) — only material facts, no evidence. The Drafter is instructed to plead the fact, not the proof.

3. **WS: Specific denial is mandatory.** Order VIII Rule 3 + Rule 5 — evasive denial = deemed admission. The Drafter generates a paragraph-wise traversal addressing every plaint paragraph by number.

4. **Commercial Suit: Statement of Truth.** If the case folder marks the suit as a Commercial Dispute (Commercial Courts Act 2015 Section 2(1)(c)), the Drafter substitutes the Statement of Truth (Order VI Rule 15A as amended) for the standard verification.

5. **Dual citation (where evidence law is referenced).** Indian Evidence Act 1872 references paired with Bharatiya Sakshya Adhiniyam 2023.

6. **Schedule of Property is mandatory** for any plaint involving immovable property.

7. **Court-Fee citation is mandatory.** State Court-Fees Schedule cited explicitly.

8. **Drafted prose anti-fabrication.** Every fact, every exhibit, every date traces to `case-facts.md`. Citations trace to `citations.md`.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/plaint-ws-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Cause-of-action paragraph absent in plaint → fatal · halt · risk of Order VII Rule 11(a) rejection.
- WS paragraph-wise traversal incomplete (some plaint paragraphs unaddressed) → deemed admission risk · halt.
- Verification language drift → load-bearing bug.
- Court-Fee citation absent → halt for user State info.
- Statement of Truth absent in Commercial Suit → Commercial Courts Act violation.

## Provenance

- Code of Civil Procedure 1908 — Orders VI, VII, VIII, XIX, XXVI, XXXIII, XXXIX, XL
- Commercial Courts Act 2015 — Section 2(1)(c), CPC Order VI Rule 15A (amended)
- Indian Evidence Act 1872 / Bharatiya Sakshya Adhiniyam 2023 (transitional)
- Limitation Act 1963 — Schedule
- State Court-Fees Acts (user-supplied per State)
- Cross-validation report `validation-plaint-ws.md` + `validation-pleadings.md` (Phase 03)
- Mulla on the Code of Civil Procedure (cited as authority; no prose lifted)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
