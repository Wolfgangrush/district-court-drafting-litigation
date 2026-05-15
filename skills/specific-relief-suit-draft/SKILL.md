---
name: specific-relief-suit-draft
description: Draft a Suit for Specific Relief before a District Court — Suit for Specific Performance of an agreement (Section 10 Specific Relief Act 1963), Suit for Declaration of status or right (Section 34), Suit for Perpetual Injunction (Section 38), or Suit for Mandatory Injunction (Section 39). Produces .docx containing Plaint + Schedule of Property + Verification + List of Documents + Vakalatnama placeholder + (optional) Application for Temporary Injunction under Order XXXIX Rules 1 and 2 CPC. Auto-fires on "draft specific performance suit" or "draft injunction suit" or "draft declaration suit" or "specific relief plaint".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Suit for Specific Relief — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## Case-type metadata

```yaml
case_type_line: SUIT FOR SPECIFIC RELIEF
case_short_codes:
  - SUIT FOR SPECIFIC PERFORMANCE      # Section 10 Specific Relief Act 1963
  - SUIT FOR DECLARATION               # Section 34 Specific Relief Act 1963
  - SUIT FOR PERPETUAL INJUNCTION      # Section 38 Specific Relief Act 1963
  - SUIT FOR MANDATORY INJUNCTION      # Section 39 Specific Relief Act 1963
  - COMPOSITE SUIT                     # any combination of the above
court_designation: |
  Civil Judge (Senior Division / Junior Division depending on pecuniary value)
  OR District Judge where the pecuniary value exceeds the Civil Judge limit
  in the relevant State.
statutory_authority: |
  Specific Relief Act, 1963 — primarily:
    · Section 10  — Specific performance of contracts
    · Section 14  — Contracts not specifically enforceable (negative discipline)
    · Section 16(c) — Readiness and willingness requirement (mandatory averment)
    · Section 20  — Discretion of court as to decreeing specific performance
    · Section 22  — Power to grant alternative relief (refund + interest)
    · Section 34  — Declaration of status or right
    · Section 35  — Effect of declaration
    · Section 38  — Perpetual injunction when granted
    · Section 39  — Mandatory injunctions
    · Section 41  — Injunction when refused
  Code of Civil Procedure, 1908:
    · Order VII Rules 1, 2, 3, 14 — Particulars to be contained in plaint
    · Order XXXIX Rules 1 and 2 — Temporary injunctions
    · Order XXVI Rules 9 and 10 — Local investigation / commissions (where relevant)
limitation_period: |
  Limitation Act 1963, Schedule —
    · Suit for specific performance: 3 years from the date fixed for performance,
      or if no such date is fixed, when the plaintiff has notice that performance
      is refused (Article 54).
    · Suit for declaration: 3 years from when the right to sue first accrues
      (Article 58) — and certain specific declarations have different periods.
    · Suit for perpetual injunction: 3 years from when the cause of action accrues
      (Article 113, residuary).
    · Suit for mandatory injunction: same — 3 years from the cause of action.
court_fee: |
  Court-Fees Act (State-specific). For specific performance suits the value of
  the suit for court-fee is generally the consideration named in the agreement
  (or the value of property where consideration is non-monetary). For declaration
  suits, the State Court-Fees Schedule fixes a specific or ad valorem fee per the
  nature of the declaration. For injunction suits, the State Schedule fixes a
  fixed-amount fee where injunction is the sole relief; if combined with
  declaration / specific performance, ad valorem applies to the principal relief.
mandatory_paragraphs:
  - readiness_and_willingness: |
      For Specific Performance suits, Section 16(c) of the Specific Relief Act
      mandates an express averment that the plaintiff has at all times been —
      and continues to be — ready and willing to perform the essential terms
      of the contract on his / her / its part. This averment is LOAD-BEARING;
      its absence is fatal to the suit.
  - tender_of_money: |
      Where the plaintiff's obligation is to pay money, an express averment
      that the plaintiff has tendered (or is willing and able to tender) the
      contractual amount is required.
  - readiness_specific_acts: |
      Where the plaintiff's obligation involves specific acts (transfer of
      title documents, execution of conveyance, mutation cooperation, etc.),
      each such act is enumerated with the date or readiness window.
accompanying_applications:
  - application_for_temporary_injunction          # Order XXXIX Rules 1 and 2 CPC, where stay of alienation / restraint on possession is needed
  - application_for_ad_interim_injunction         # ex parte interim, where the urgency requires
  - application_for_appointment_of_court_receiver # Order XL CPC, where the property requires preservation
  - application_for_local_commission              # Order XXVI Rules 9 and 10 CPC, where property boundary or possession is contested
  - application_for_condonation_of_delay          # Section 5 Limitation Act, where filed beyond limitation
typical_exhibit_order:
  - A: The agreement / contract sought to be specifically enforced (Specific Performance suit)
  - B: Title documents of the suit property (sale deed / gift deed / will / 7/12 / mutation)
  - C: Legal notice issued by the plaintiff calling upon performance / objecting to denial
  - D: Reply to the legal notice (if received)
  - E: Documents of readiness and willingness (e.g., bank statements showing the plaintiff's financial capacity, draft sale deed prepared by the plaintiff, etc.)
  - F: Documents showing the defendant's breach / repudiation (correspondence, third-party-sale notice, etc.)
  - G-onwards: case-specific documentary support
```

## Section-by-section structure (extends `_district_pleading_base` Section 1)

The universal section order from the base applies. Slot-specific notes for Specific Relief Suits:

| # | Section | Specific Relief specifics |
|---|---|---|
| 1 | Cause Title | `IN THE COURT OF THE [Civil Judge, Senior/Junior Division / District Judge] AT [Place] / [Suit Type] No. _____ of [Year] / IN THE MATTER OF: [Plaintiff] ... VERSUS ... [Defendant]` |
| 3 | Statement of Facts | Chronology — the agreement / cause-creating event, the plaintiff's performance / readiness, the defendant's breach / denial, the legal notice + reply, the present cause for suit. Each fact paragraph numbered. Each exhibit reference uses the inline EXHIBIT-A / B / C convention. |
| 3a | (Mandatory) Readiness and Willingness paragraph | Express averment under Section 16(c) Specific Relief Act — LOAD-BEARING. Authored fresh from the case folder; no corpus prose lifted. |
| 4 | Particulars of Claim | Identifies the specific contract / right / property / status, the breach, and the relief sought. |
| 5 | Jurisdiction | Section 16(d) CPC — where the suit is for specific performance of contract for sale / lease / mortgage of immovable property, the suit is filed where the property is situated. For pure declaration / injunction suits, Section 20 CPC (where defendant resides or cause of action arose). |
| 6 | Limitation | Article 54 (Specific Performance — 3 years from date fixed for performance or from notice of refusal) / Article 58 (Declaration) / Article 113 (Injunction). Drafter computes from `case-facts.md` Section 4. |
| 7 | Court-Fee | Per State Court-Fees Schedule and the suit value. |
| 8 | Prayer | Specific relief enumerated in numbered clauses: (a) decree of specific performance directing the defendant to execute and register the sale deed; (b) in the alternative, decree for refund + interest + damages under Section 22; (c) permanent injunction restraining the defendant from alienating / encumbering / parting with possession of the suit property pending the disposal; (d) costs of the suit; (e) catchall. |
| 9 | Verification | CPC Order VI Rule 15, verbatim. |
| 10 | Place + Date + Advocate signature | Plaintiff's advocate with Bar Council enrolment number. |
| 11 | Schedule of Property | Mandatory for immovable-property suits — full description per `_district_pleading_base` Section 3. |
| 12 | List of Documents / Exhibits | Mirror of inline EXHIBIT markers. |

## Hard rules (in addition to those inherited)

1. **Readiness and Willingness is load-bearing.** For Specific Performance suits, the express averment under Section 16(c) MUST appear in the Statement of Facts. The Drafter inserts it; the Verifier halts the pipeline if absent.

2. **Tender of money clause.** Where the plaintiff's obligation is to pay money (purchase price for sale, premium for lease, etc.), an express averment of tender / readiness to tender is required.

3. **Alternative relief under Section 22.** The Prayer must offer the alternative relief of refund + interest + damages, in case specific performance is refused as a matter of discretion under Section 20.

4. **Court-Fee citation.** The plaint must explicitly state the Court-Fees Act schedule under which the fee is paid. State-specific; the user supplies the State in `format-from-user.md` Section 7.

5. **Schedule of Property is mandatory for immovable-property suits.** The Drafter halts the .docx render if the case folder lacks the property description.

6. **Verification is verbatim CPC Order VI Rule 15.** No paraphrasing.

7. **Drafted prose anti-fabrication.** Every fact, every exhibit reference, every date traces to `case-facts.md`. Case citations trace to `citations.md`.

8. **Where the agreement is unstamped or insufficiently stamped:** the Drafter inserts a placeholder note alerting the plaintiff's advocate of the Indian Stamp Act objection that may be raised under Section 35 of the Stamp Act — but does not draft around it (this is a substantive judgment for the advocate).

## Format reference (case-type prose)

🟡 **Place the case-type-specific format text in the file referenced below.**

```
${CLAUDE_PLUGIN_ROOT}/skills/specific-relief-suit-draft/format-from-user.md
```

User pastes preferred style references. **Fail-stop until populated.**

## Falsification triggers

- Readiness-and-Willingness averment absent in Specific Performance suit → fatal · halt before render.
- Schedule of Property absent in immovable-property suit → render halt.
- Court-Fee Schedule citation absent → flag for user to supply State info.
- Verification language drift from Order VI Rule 15 → load-bearing bug · patch.
- Limitation Article cited does not match the type of suit → re-compute.

## Provenance

Patterns encoded in this file trace to:
- Specific Relief Act, 1963 — Sections 10, 14, 16(c), 20, 22, 34, 35, 38, 39, 41
- Code of Civil Procedure, 1908 — Order VI Rule 15, Order VII Rules 1-3, 14, Order XXVI Rules 9-10, Order XXXIX Rules 1-2, Order XL
- Limitation Act, 1963 — Articles 54, 58, 113
- Court-Fees Act, 1870 (and State amendments — user-supplied)
- Cross-validation report `validation-specific-relief.md` (Phase 03 of the India-Legal Corpus Pipeline)
- Mulla on the Code of Civil Procedure (cited as authority for the structural skeleton; no prose lifted)

No drafted prose has been transcribed from the corpus or from any third-party advocate's drafts.

---

## Status: SKELETON v0.0.1 — 2026-05-15 (Sprint 2 Day 0)

Pending pre-ship:
- [ ] `format-from-user.md` template (one-line user-paste flow)
- [ ] Drafter agent Specific-Relief branch (Readiness-and-Willingness paragraph generation)
- [ ] Verifier checks (Section 16(c) presence, Schedule of Property presence, Court-Fee citation, Verification verbatim)
- [ ] Worked-example case-folder template under `examples/specific-relief-example/` (placeholders only)
- [ ] Quality gate pass per `05-quality-gates.md`
