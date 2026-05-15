---
name: bnss-313-statement-draft
description: Draft the Statement of the Accused under Section 351 of the Bharatiya Nagarik Suraksha Sanhita 2023 (corresponding to Section 313 of the Code of Criminal Procedure 1973). The accused's statement recorded after prosecution evidence, where the accused is examined by the court on the incriminating circumstances appearing in the evidence. Produces .docx containing the structured question-by-question response draft + reference to the prosecution evidence + the accused's affirmative version. Auto-fires on "draft 313 statement" or "draft 351 statement" or "draft section 313 statement" or "accused statement bnss".
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Section 351 BNSS / Section 313 CrPC Statement — Draft Skill

Extends: `${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/SKILL.md`
Common rules: `${CLAUDE_PLUGIN_ROOT}/skills/_drafting_common/SKILL.md`

## What this draft is — and is not

The Section 351 BNSS / Section 313 CrPC statement is **not a pleading filed by the accused**. It is the accused's statement **recorded by the trial court** after the conclusion of prosecution evidence. The judge puts questions to the accused on each incriminating circumstance in the prosecution evidence; the accused answers; the court records.

**This skill drafts the ACCUSED'S PREPARED ANSWERS** — a working document the defence advocate prepares in advance to ensure the accused's responses are:
- Specific to each incriminating circumstance
- Internally consistent with the defence theory
- Capable of explaining or denying every adverse fact in the prosecution case
- Not exposing the accused to fresh prejudice through inadvertent admission

The court will frame the actual questions; this skill produces the accused's structured answers in advance for the defence advocate's coaching of the accused.

## Case-type metadata

```yaml
case_type_line: STATEMENT OF THE ACCUSED — SECTION 351 BNSS / 313 CrPC
case_short_code: 351 / 313
court_designation: |
  Court of the trial Magistrate (for warrant cases / summons cases) OR
  Court of Sessions (for sessions trials) — where the trial is pending.
statutory_authority: |
  Section 351 of the Bharatiya Nagarik Suraksha Sanhita, 2023
  (corresponding to Section 313 of the Code of Criminal Procedure, 1973 —
   applicable where the trial commenced under the old code).
nature_of_statement: |
  Not on oath. The accused is not subject to cross-examination on the
  statement. False answers do not attract perjury (Section 351 BNSS is
  a substitute for oath-bearing). However, false or inconsistent answers
  may be relied upon by the prosecution as adverse circumstances.
sequence_in_trial: |
  · After prosecution evidence is closed (BNSS Section 348(2));
  · Before defence evidence is led;
  · The court generally takes the statement before charge in summons cases
    (BNSS Section 274 / 275) and after charge in warrant cases (BNSS Section 269+).
mandatory_paragraphs:
  - identification_of_each_incriminating_circumstance: |
      For each incriminating circumstance in the prosecution evidence, the
      defence-prepared answers identify the witness, the deposition reference,
      and the precise content of the adverse fact.
  - prepared_answer_per_circumstance: |
      The accused's answer to each circumstance. Options:
        (a) Denial — "It is not true that..."
        (b) Explanation — "The fact that... is admitted, but the inference
            that I committed the offence is not warranted because..."
        (c) Counter-narrative — alternative account of the same fact
        (d) "I do not know" — for facts genuinely outside the accused's knowledge
        (e) Right to silence — for matters where any answer may prejudice the
            accused unfavourably (the accused has the right to remain silent
            per the Constitution)
  - accused_affirmative_version: |
      Where the accused has an affirmative defence (alibi / self-defence /
      grave and sudden provocation / good-faith / etc.), the structured
      presentation of that defence — to be repeated through the answers.
  - reference_to_defence_witnesses: |
      Where defence witnesses are to be examined, the accused's statement
      indicates the topics on which each defence witness will testify.
  - readiness_to_lead_defence_evidence: |
      Statement that the accused wishes to lead defence evidence (where
      defence evidence will be led) OR that the accused does not wish to
      lead evidence and rests on the prosecution's failure to prove the
      case beyond reasonable doubt.
right_to_silence_doctrine: |
  BNSS Section 351 / CrPC Section 313 examination is not on oath. The
  accused has the right to refuse to answer any question. Refusal cannot
  be the sole basis for conviction. The Drafter inserts a placeholder note
  for the defence advocate where the recommended strategy is silence.
no_self_incrimination: |
  Article 20(3) Constitution of India — protection against testimonial
  compulsion. The Section 351 / 313 statement is not "compelled testimony"
  as such, but the Drafter is instructed to ensure no answer needlessly
  exposes the accused to fresh prejudice on a circumstance not already
  in the prosecution evidence.
```

## Section-by-section structure (different from pleading skills)

This is a coaching document, not a filed pleading. Structure:

```
SECTION 351 BNSS STATEMENT — DEFENCE-PREPARED ANSWERS
For: [Accused Name], in Sessions Trial No. _____ of [Year] / Criminal Case
No. _____ of [Year] pending before the Court of [Designation] at [Place]

PROSECUTION'S CASE — SUMMARY OF INCRIMINATING CIRCUMSTANCES

Circumstance 1: [witness PW1, deposition reference, content of adverse fact]
   PREPARED ANSWER: [denial / explanation / counter-narrative / "I do not know" / silence]
   RATIONALE: [why this answer is recommended — defence-theory consistency, etc.]

Circumstance 2: [witness PW2, ...]
   PREPARED ANSWER: ...
   RATIONALE: ...

... [for each circumstance the court is likely to put]

ACCUSED'S AFFIRMATIVE VERSION
[the alternative narrative the defence wishes the accused to present consistently]

DEFENCE WITNESSES PROPOSED
[list of defence witnesses + topics of testimony]

OPENING STATEMENT (where allowed by court)
[brief statement of defence position, where the court invites or permits]
```

## Hard rules (in addition to those inherited)

1. **This is a COACHING DOCUMENT, not a filed pleading.** The output is for the defence advocate's use in preparing the accused for court examination. The court frames the actual questions.

2. **Dual citation.** Section 351 BNSS paired with Section 313 CrPC. Where the trial began under CrPC and continues under BNSS, the dual citation is essential.

3. **Right to silence is preserved.** Where the recommended strategy is silence (e.g., for circumstances where any answer prejudices), the Drafter explicitly notes "Silence — invoke Article 20(3) Constitution / no comment".

4. **Internal consistency check.** All prepared answers across all circumstances must be internally consistent with the defence theory pleaded in the WS / replies / proposed evidence. The Drafter performs a consistency pass.

5. **No new admissions.** No prepared answer admits a circumstance not already in the prosecution evidence. The Drafter is instructed to avoid widening the accused's exposure.

6. **The accused's own words may differ.** This is a prepared document — the actual statement is in the accused's own words at trial. The Drafter notes this caveat in the output.

7. **NEVER attempt to file or formally submit the prepared Section 351 statement.** This document is privileged work product of the defence team. The plugin's output is for the defence advocate's offline use; it is NOT filed.

## Format reference

🟡 **Place the case-type-specific format text in:**
```
${CLAUDE_PLUGIN_ROOT}/skills/bnss-313-statement-draft/format-from-user.md
```
Fail-stop until populated.

## Falsification triggers

- Single-citation BNSS / CrPC → fatal.
- Prepared answer admits a circumstance not in prosecution evidence → halt · re-author.
- Inconsistency between prepared answers across circumstances → halt · re-audit.
- Defence theory not surfaced anywhere in the answers → flag.

## Provenance

- Bharatiya Nagarik Suraksha Sanhita 2023 — Sections 348, 351
- Code of Criminal Procedure 1973 — Section 313 (transitional)
- Constitution of India — Article 20(3)
- General principles of criminal trial procedure

No drafted prose transcribed from corpus.

---

## Status: SKELETON v0.0.1 — 2026-05-15
