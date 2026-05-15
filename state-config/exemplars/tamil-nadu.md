# state-config — Tamil Nadu (and Puducherry)

**Validation depth:** Researched · awaiting Registry validation.

```yaml
state: "Tamil Nadu (and Puducherry UT)"
local_language_of_pleadings: "English (Tamil permitted under Tamil Nadu State amendment to CPC Section 137 — many District Courts hear/accept in Tamil)"
bar_council: "Bar Council of Tamil Nadu and Puducherry"

court_designation_taxonomy:
  civil_apex_district: "Principal District Judge / District Judge"
  civil_senior: "Subordinate Judge"                  # CRITICAL: TN uses Subordinate Judge, NOT Civil Judge SD
  civil_junior: "District Munsiff"                    # CRITICAL: TN uses District Munsiff, NOT Civil Judge JD
  criminal_apex_district: "Sessions Judge"
  criminal_chief_magistrate: "Chief Judicial Magistrate"
  criminal_first_class: "Judicial Magistrate First Class"
  small_causes: "Court of Small Causes (Chennai)"
  family_court_judge: "Family Court Judge"

state_civil_courts_act: |
  Tamil Nadu Civil Courts Act (read with Madras Civil Courts Act 1873 — historic origin)

state_court_fees_act: |
  Tamil Nadu Court-Fees and Suits Valuation Act, 1955
default_court_fee_schedule_citation: |
  Schedule I of the Tamil Nadu Court-Fees and Suits Valuation Act, 1955.

state_stamp_act: "Indian Stamp Act, 1899 (as amended by Tamil Nadu)"

civil_manual: "Madras Civil Manual"                  # Still named "Madras"
criminal_manual: "Madras Criminal Manual"

vakalatnama_format: |
  Bar Council of Tamil Nadu and Puducherry standard vakalatnama.

exhibit_prefix: "EXHIBIT-"
exhibit_prefix_alternative: "Document No. "          # Common in TN civil courts

family_courts_established: yes
family_court_rules: "Tamil Nadu Family Court Rules"

paper_size: A4
font_family: "Times New Roman"
font_size_body: 14
line_spacing: 1.5                                    # Some Madras Civil Manual practices prefer double-spacing in lower courts

notes: |
  CRITICAL: Tamil Nadu uses "Subordinate Judge" and "District Munsiff" terminology, NOT "Civil Judge Senior/Junior Division". Court header must match. Tamil-language pleadings accepted at District-Court level under the State amendment. The Madras Civil Manual is the foundational State procedural manual (still named "Madras" despite the State being renamed Tamil Nadu in 1969). Puducherry (UT) is under TN HC jurisdiction; District Court practice follows TN conventions.
```
