# state-config-template.md

This is a **template**. Copy this file into your case folder, rename to `state-config.md`, and fill in the values for your specific State / Union Territory. The plugin's Format and Drafter agents read this file at run-time alongside the case-type SKILL templates to render District-Court pleadings in your State's idiom.

**Do not edit this template in the plugin folder.** Always work on a copy inside your case folder. Validated State exemplars for the major State jurisdictions are in `state-config/exemplars/` — copy the right one for your State, then customise per your specific bench (Pune Sessions vs Nagpur Sessions, etc.).

---

## Section 1 — State / UT identification

```yaml
state: "Maharashtra"                          # Your practice State / UT
local_language_of_pleadings: "English"        # Most States: English. Some accept regional language (Marathi in Maharashtra, Tamil in TN, etc.)
bar_council: "Bar Council of Maharashtra and Goa"
```

## Section 2 — Court hierarchy and designation

```yaml
court_designation_taxonomy:
  civil_apex_district: "District Judge"             # Most States: District Judge
  civil_senior: "Civil Judge, Senior Division"      # Maharashtra/Karnataka/UP/Bengal; OR "Subordinate Judge" (TN/Kerala); OR "Senior Civil Judge" (AP/TG/Rajasthan); OR "Sub Judge" (Bihar/Jharkhand/Punjab); OR "Civil Judge Class I" (MP)
  civil_junior: "Civil Judge, Junior Division"      # Maharashtra/Karnataka; OR "District Munsiff" (TN); OR "Munsiff" (Kerala); OR "Junior Civil Judge" (AP/TG); OR "Munsif" (Bihar/Jharkhand); OR "Civil Judge Class II" (MP)
  criminal_apex_district: "Sessions Judge"
  criminal_chief_magistrate: "Chief Judicial Magistrate"
  criminal_first_class: "Judicial Magistrate First Class"
  criminal_second_class: "Judicial Magistrate Second Class"
  family_court_judge: "Family Court Judge"          # Where Family Courts established under Family Courts Act 1984
```

The court-header line for your court will use the designation that matches your State's terminology.

## Section 3 — Pecuniary jurisdiction

```yaml
state_civil_courts_act: |
  Bombay Civil Courts Act, 1869                     # Maharashtra; also Gujarat (pre-1960 origin)
                                                    # Madras Civil Courts Act 1873 (TN/AP/TG/Kerala — historic; superseded by State amendments)
                                                    # Bengal Civil Courts Act 1887 (West Bengal/Bihar/Jharkhand/Odisha)
                                                    # Punjab Courts Act 1918 (Punjab/Haryana/Delhi/HP)
                                                    # Karnataka Civil Courts Act 1964
                                                    # Rajasthan Civil Courts Ordinance 1950
                                                    # MP Civil Courts Act 1958
                                                    # Others: see your State Civil Courts Act
pecuniary_jurisdiction_limits:                       # Current as of 2026 — verify against latest State notifications
  civil_judge_junior: "₹X (per State notification)"
  civil_judge_senior: "₹Y (per State notification)"
  district_judge: "Unlimited (per State notification)"
```

## Section 4 — Court-Fees Act

```yaml
state_court_fees_act: |
  Bombay Court-Fees Act, 1959 (Maharashtra/Gujarat — inherited from Bombay State)
                                                    # Madras Court-Fees and Suits Valuation Act 1955 (TN)
                                                    # Karnataka Court-Fees and Suits Valuation Act 1958
                                                    # Kerala Court-Fees and Suits Valuation Act 1959
                                                    # Andhra Pradesh Court-Fees and Suits Valuation Act 1956 (AP/TG)
                                                    # West Bengal Court-Fees Act 1971
                                                    # Court-Fees Act 1870 (Central — Delhi/UP/MP/Bihar/some others with State amendments)
                                                    # Rajasthan Court-Fees and Suits Valuation Act 1961
default_court_fee_schedule_citation: |
  Schedule I, Article [N] of the [State Court-Fees Act, Year], as amended.
```

## Section 5 — State Stamp Act

```yaml
state_stamp_act: "Indian Stamp Act, 1899 (as amended by [State])"
                                                    # Many States have own amendments; some have own State Stamp Acts (e.g., Maharashtra Stamp Act 1958)
```

## Section 6 — Civil Manual / Criminal Manual

```yaml
civil_manual: "Bombay Civil Manual"                  # Maharashtra
                                                    # Madras Civil Manual (TN)
                                                    # Karnataka Civil Manual
                                                    # etc.
criminal_manual: "Bombay Criminal Manual"            # Maharashtra
                                                    # etc.
```

## Section 7 — Vakalatnama

```yaml
vakalatnama_format: |
  Standard advocate vakalatnama per State Bar Council; signed by client + advocate; stamped per State Stamp Schedule.
state_specific_vakalatnama_notes: |
  Maharashtra: Bar Council of Maharashtra and Goa standard vakalatnama
  Tamil Nadu: Bar Council of Tamil Nadu and Puducherry standard
  Each State Bar Council publishes its own vakalatnama template
```

## Section 8 — Exhibit / Annexure marker convention

```yaml
exhibit_prefix: "EXHIBIT-"                          # Common pan-India default
exhibit_prefix_alternatives:
  - "ANNEXURE-A"                                    # Some State practices use ANNEXURE prefix
  - "Doc. No. 1"                                    # Some Civil Manuals prefer Doc. No.
```

## Section 9 — Family Court / specialised tribunal references

```yaml
family_courts_established: yes                      # Maharashtra has Family Courts; many States do
family_court_rules: "Maharashtra Family Court Rules" # State-specific
juvenile_justice_board: yes
state_consumer_disputes_commission: yes
```

## Section 10 — Language and paper

```yaml
paper_size: A4                                       # Most States: A4. Some Original Side practices use Legal
font_family: "Times New Roman"
font_size_body: 14                                   # Or 12 per some Civil Manuals
line_spacing: 1.5                                    # Most States; Tamil Nadu Civil Manual prefers double in some courts
```

## Section 11 — Limitation references

```yaml
limitation_act: "Limitation Act, 1963 (Central · pan-India)"
state_specific_limitation_notes: |
  Limitation Act 1963 applies pan-India. Some State enactments (Rent Control Acts, Land Reforms Acts, Tenancy Acts) carry their own shorter limitation periods.
```

## Section 12 — State-specific procedural quirks

```yaml
notes: |
  (List any State-specific quirks that affect drafting — e.g., Tamil Nadu civil courts accept Tamil-language pleadings under Section 137 CPC State amendment; Maharashtra permits Marathi in select courts; UP requires Hindi in some District Courts; Bengal Civil Courts Act has distinct hierarchy from Bombay; etc.)
```

---

## How to use this file

1. Pick the right State exemplar from `state-config/exemplars/` (15 major State exemplars provided).
2. Copy to your case folder: `cp state-config/exemplars/<your-state>.md <case-folder>/state-config.md`
3. Customise the values per your specific court (Pune Sessions vs Nagpur Sessions vs Mumbai City Civil, etc.).
4. The plugin's Format and Drafter agents read this file alongside the case-type SKILL to render in your State's idiom.

## Contributing state-config exemplars

Advocates practising in States not yet exemplified are invited to submit filled exemplars via GitHub Issue. Each State exemplar that lands in the public folder benefits every advocate in that State.

Do NOT include any case-specific information — only the State's standard procedural conventions.
