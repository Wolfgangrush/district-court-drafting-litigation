# state-config — Bihar and Jharkhand

**Validation depth:** Researched · awaiting Registry validation.

```yaml
state: "Bihar / Jharkhand (bifurcated 2000)"
local_language_of_pleadings: "English / Hindi"
bar_council: "Bar Council of Bihar / Bar Council of Jharkhand"

court_designation_taxonomy:
  civil_apex_district: "Principal District Judge / District Judge"
  civil_senior: "Sub Judge"                            # CRITICAL: Bihar/Jharkhand use older "Sub Judge" terminology
  civil_junior: "Munsif"                                # CRITICAL: Bihar/Jharkhand "Munsif"
  additional_district_judge: "Additional District Judge"
  criminal_apex_district: "Sessions Judge"
  criminal_chief_magistrate: "Chief Judicial Magistrate"
  criminal_first_class: "Judicial Magistrate First Class"
  family_court_judge: "Family Court Judge"

state_civil_courts_act: |
  Bengal, Agra and Assam Civil Courts Act, 1887 (continues in Bihar and Jharkhand)

state_court_fees_act: |
  Court-Fees Act, 1870 (as extended to Bihar with State amendments — continues in Jharkhand)

state_stamp_act: "Indian Stamp Act 1899 (as amended by Bihar — continues in Jharkhand)"

civil_manual: "Bihar Civil Manual / Jharkhand Civil Manual"
criminal_manual: "Bihar Criminal Manual / Jharkhand Criminal Manual"

vakalatnama_format: |
  Bar Council of Bihar / Jharkhand standard vakalatnama.

exhibit_prefix: "EXHIBIT-"

family_courts_established: yes (partial coverage)

paper_size: A4
font_family: "Times New Roman"
font_size_body: 14
line_spacing: 1.5

notes: |
  CRITICAL: Bihar and Jharkhand retain the older "Sub Judge" and "Munsif" terminology (distinct from Maharashtra's "Civil Judge SD/JD" or TN's "Subordinate Judge / Munsiff"). Jharkhand bifurcated from Bihar in 2000 and inherited the same framework. Hindi pleadings widely accepted at District Court level.
```
