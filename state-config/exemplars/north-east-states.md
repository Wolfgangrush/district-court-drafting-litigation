# state-config — North-East States (Assam, Nagaland, Mizoram, Arunachal Pradesh, Manipur, Meghalaya, Tripura, Sikkim)

**Validation depth:** Stub · community contribution welcomed.

```yaml
state: "(NE State — specify your State: Assam / Nagaland / Mizoram / Arunachal Pradesh / Manipur / Meghalaya / Tripura / Sikkim)"
local_language_of_pleadings: "English"
bar_council: "Bar Council of Assam / Nagaland / etc. (per State)"

court_designation_taxonomy:
  civil_apex_district: "Principal District Judge / District Judge"
  civil_senior: "Civil Judge (Senior Division) / Sub-Judge / Sub Court"   # varies by State
  civil_junior: "Civil Judge (Junior Division) / Munsiff / Munsif"        # varies by State
  criminal_apex_district: "Sessions Judge"
  criminal_chief_magistrate: "Chief Judicial Magistrate"
  family_court_judge: "Family Court Judge"

state_civil_courts_act: |
  (Bengal, Agra and Assam Civil Courts Act 1887 in some; later State-specific Acts in newer States)

state_court_fees_act: |
  Court-Fees Act 1870 (as amended by each NE State) OR Assam Court-Fees Act / State equivalents

state_stamp_act: "Indian Stamp Act 1899 (as amended by each State)"

civil_manual: "(State-specific)"
criminal_manual: "(State-specific)"

vakalatnama_format: |
  State Bar Council standard vakalatnama.

exhibit_prefix: "EXHIBIT-"

family_courts_established: "(varies by State)"

paper_size: A4
font_family: "Times New Roman"
font_size_body: 14
line_spacing: 1.5

notes: |
  The North-East States have varied procedural frameworks. Assam, Nagaland, Mizoram, and Arunachal Pradesh fall under Gauhati HC jurisdiction. Manipur, Meghalaya, and Tripura have their own HCs (post-2013). Sikkim has its own HC (post-1975). Some NE States have unique customary-law overlays that affect District Court procedure (Sixth Schedule autonomous district councils in Assam, Meghalaya, Tripura, Mizoram). Advocates practising in NE States are invited to contribute filled state-config exemplars.
```
