---
name: pleadings-draft
description: Draft a generic civil pleading before a District Court — used for pleading shapes that do not fit the more specific Plaint / WS / Application / Petition / Specific Relief Suit skills. Covers amendment pleadings (Order VI Rule 17 CPC), additional pleadings, reply-rejoinder pleadings, and miscellaneous pleadings filed in the course of a suit. Produces .docx containing the pleading + Verification + List of Documents. Auto-fires on "draft pleading" or "draft amendment pleading" or "draft reply pleading" or "draft rejoinder".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Generic Civil Pleading — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## When to use this skill

Use this skill when the pleading shape does not cleanly fit:
- `plaint-ws-draft` (plaint or written statement)
- `specific-relief-suit-draft` (specific performance / declaration / injunction)
- `application-draft` (interlocutory application under a CPC Order/Rule)
- `petition-draft` (succession / probate / matrimonial / guardianship)

Typical use cases:
- Amendment pleading under Order VI Rule 17 CPC (a fresh pleading filed after leave is granted)
- Additional pleading under Order VIII Rule 9 CPC (reply / rejoinder / sur-rejoinder)
- Particulars (Order VI Rule 4 CPC) when ordered by the court
- Statement of Truth (Commercial Courts Act 2015) where filed as a stand-alone supplement
- Miscellaneous civil pleadings filed in the course of an existing suit

## Case-type metadata

```yaml
case_type_line: CIVIL PLEADING
pleading_subtypes:
  - AMENDMENT_PLEADING            # Order VI Rule 17 CPC — fresh pleading post-leave-to-amend
  - REPLY_TO_WRITTEN_STATEMENT    # Order VIII Rule 9 CPC
  - REJOINDER                     # plaintiff's response to defendant's reply
  - SUR_REJOINDER                 # further reply by leave of court
  - PARTICULARS                   # Order VI Rule 4 CPC — when ordered
  - SUPPLEMENTAL_PLEADING         # Order VI Rule 17 (events post-filing)
  - STATEMENT_OF_TRUTH            # Commercial Courts Act 2015 — stand-alone supplement
  - MISC_PLEADING                 # any other pleading filed in the suit
court_designation: |
  Same court that is seised of the underlying suit.
statutory_authority_template: |
  PLEADING UNDER [SPECIFIC ORDER / RULE] OF THE CODE OF CIVIL PROCEDURE, 1908
  FILED IN [Suit / Case Type] No. _____ OF [Year]
mandatory_paragraphs_per_subtype:
  AMENDMENT_PLEADING:
    - reference_to_leave_order: "Reference to the order granting leave to amend (with date and case number)."
    - amended_paragraphs: "Numbered paragraphs as amended — using strike-through and italics or other convention to mark the amendments."
    - no_new_cause_of_action_or_limitation_bar: "Statement (where applicable) that the amendment does not introduce a new cause of action or a time-barred claim — per the proviso to Order VI Rule 17."
  REPLY_TO_WS:
    - paragraph_wise_response: "Plaintiff's response to each paragraph of the WS — admission / denial / additional facts."
    - new_facts_arising: "Any new facts arising since the plaint was filed (where applicable)."
  REJOINDER:
    - response_to_reply: "Plaintiff's rejoinder to the defendant's reply — admission / denial / further facts."
  PARTICULARS:
    - specific_particulars_ordered: "The specific particulars as directed by the court order — verbatim per the court's directions."
  SUPPLEMENTAL_PLEADING:
    - events_arising_post_filing: "Material events arising after the original pleading was filed."
    - leave_obtained: "Reference to the leave order permitting the supplemental pleading."
verification_required: yes  # all pleadings carry CPC Order VI Rule 15 verification
prayer_pattern: |
  "It is, therefore, prayed that this Hon'ble Court may be pleased to
  [SPECIFIC RELIEF — e.g., take the amended pleading on record / accept the rejoinder / record the particulars filed]."
  + catchall.
exhibit_order:
  - A-onwards: documents in support of the new facts pleaded or in response to opposing party's pleadings.
```

## Hard rules (in addition to those inherited)

1. **Order VI Rule 2 — material facts only.** Generic pleadings carry the same discipline as plaints / WS — material facts, no evidence.

2. **Amendment pleadings must reference the leave order.** Without the leave-order reference, the amendment is liable to be ignored by the court.

3. **No fresh cause of action via amendment** unless the proviso to Order VI Rule 17 is satisfied (or the amendment is permitted by special leave).

4. **Verification per CPC Order VI Rule 15.** Verbatim — same as plaint / WS.

5. **Pleading-subtype-specific paragraph structure.** Amendment pleadings show the changes; reply / rejoinder pleadings respond paragraph-by-paragraph; particulars supply the specific information ordered.

6. **Drafted prose anti-fabrication.** Every fact, every reference, every date traces to `case-facts.md`.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/pleadings-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Amendment pleading missing leave-order reference → flag.
- Reply / rejoinder skips paragraphs of the opposing pleading → flag.
- Verification missing → halt.

## Provenance

- Code of Civil Procedure 1908 — Order VI (Rules 1, 2, 4, 15, 17), Order VIII (Rule 9)
- Commercial Courts Act 2015 (Statement of Truth provisions)
- Cross-validation report `validation-pleadings.md` (Phase 03)

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
