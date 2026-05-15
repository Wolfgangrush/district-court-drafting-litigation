# NOTICE — Provenance and Privilege Statement

This document is part of the public release of the `district-court-drafting` plugin (v0.1.0 and onwards). It declares the provenance of the plugin's content, in order to address any question about advocate-client privilege, client confidentiality, professional ethics, and personal-data protection that may be raised by any reader, complainant, regulator, Bar Council disciplinary authority, or District Court Registry / Office.

This NOTICE is published in plain language so that any reader — practising advocate, judge, Bar Council officer, regulator, member of the public, fellow developer — can understand the position without ambiguity.

---

## 1. What this plugin contains

This plugin contains the following categories of content, and **only** the following categories of content:

(a) **Procedural skeletons** — the structural shape of District Court pleadings as mandated by Indian procedural law (Cause Title, Parties Block, Statement of Facts, Particulars of Claim, Jurisdiction, Limitation, Court-Fee, Prayer, Verification, Advocate's Signature Block, Schedule of Property, List of Documents / Exhibits, accompanying applications).

(b) **Formatting conventions** — standard District Court formatting under the Code of Civil Procedure 1908 and the Bharatiya Nagarik Suraksha Sanhita 2023, supplemented by State Civil and Criminal Manuals (where State-specific format details are required, they are supplied by the user via `format-from-user.md`).

(c) **Statutory references** — citations to public statutes (Constitution of India, Code of Civil Procedure 1908, Specific Relief Act 1963, Indian Contract Act 1872, Transfer of Property Act 1882, Indian Succession Act 1925, Bharatiya Nyaya Sanhita 2023 / Indian Penal Code 1860, Bharatiya Nagarik Suraksha Sanhita 2023 / Code of Criminal Procedure 1973, Bharatiya Sakshya Adhiniyam 2023 / Indian Evidence Act 1872, Limitation Act 1963, Court-Fees Act 1870, Suits Valuation Act 1887, and other public enactments).

(d) **Procedural rule references** — citations to public rules of court (CPC Schedules and Appendix A Forms, BNSS Schedules, State Civil Manuals, State Criminal Manuals, Bar Council of India Standards of Professional Conduct and Etiquette under Section 49(1)(c) of the Advocates Act 1961).

(e) **Generic placeholders** — every variable in every template is marked with a placeholder such as `[Plaintiff Name]`, `[Defendant Name]`, `<DD.MM.YYYY>`, `[Property Description]`, `[Survey Number]`, `[Advocate Name]`, `[Bar Council Enrolment No.]`, `[Court-Fee Amount]`, `[State Court-Fees Act citation]`, `[EXHIBIT A]`. No placeholder is filled with any specific person's name, any specific date, any specific case number, any specific property description, or any specific identifying information.

(f) **Anti-hallucination workflow** — six agents (Reader, Format, Drafter, Verifier, Refiner, Overseer) that operate on a case folder supplied by the user. The plugin itself contains no case folder.

(g) **Case-citation discipline** — the Drafter agent is constrained from generating case names + citations from training memory; every case citation in the output must trace to a user-supplied source.

(h) **State-aware design** — the plugin recognises that District Court practice is heavily State-specific (Court-Fees Schedules, Civil Manuals, Vakalatnama formats, pecuniary jurisdiction limits) and the `format-from-user.md` files explicitly request the user's State and the user's State-specific procedural references. The plugin does not assume any State's specifics; the user supplies them.

---

## 2. What this plugin does NOT contain

This plugin does **not** contain any of the following, and has never contained any of the following at any point in any committed version:

(a) The text, language, or substance of any drafted pleading by any advocate, compiler, or third party. Every connective phrase, every authored line, every skeleton paragraph in every skill file has been written fresh by the author for this plugin.

(b) Any text taken verbatim from any commercially compiled or privately compiled corpus of pleadings. Where the India-Legal Corpus Pipeline has been used to cross-validate structural patterns, the pipeline operates under the express `corpus-as-verifier-not-source` doctrine: the corpus tells us *which patterns are used in practice* but never supplies the *language* that is encoded.

(c) Any specific client's name, address, contact information, case number, FIR number, judgment text, deposition text, property survey number, mutation entry, or any other identifier that could link the plugin to any real proceeding before any District Court or subordinate court of India.

(d) Any privileged communication between any advocate and any client.

(e) Any document or material that would attract Section 132 of the Bharatiya Sakshya Adhiniyam 2023 (corresponding to Section 126 of the Indian Evidence Act 1872) on professional communications.

(f) Any solicitation of legal services, any commercial offering, any advertisement of the author's practice, or any inducement to retain the author. The plugin is published under Bar Council of India Rule 36 (Conduct and Etiquette — restriction on advertising and solicitation) as open-source infrastructure released free of cost, without any commercial channel.

---

## 3. The legal distinction this NOTICE relies on

The substantive legal distinction is between:

**(a) Privileged client communication** — protected under Section 132 of the Bharatiya Sakshya Adhiniyam 2023 (corresponding to Section 126 of the Indian Evidence Act 1872). This is the communication itself between the advocate and the client, the advice given, the instructions received, and the documents shared in the course of the retainer.

**(b) Public procedural knowledge** — the rules of procedure prescribed by the CPC, BNSS, substantive statutes (Specific Relief Act, Contract Act, TPA), the State Civil and Criminal Manuals, and the Court-Fees Acts. This is the same knowledge that appears in every legal-drafting textbook (Mulla on CPC, Sarkar, AIR Civil Drafting Manual, Universal's). It is not anyone's private property.

This plugin contains only category (b). It does not contain, and has never contained, anything from category (a).

The plugin is structurally incapable of containing category (a), because the plugin's design separates the case folder (which is the user's own working directory, never committed to this repository) from the plugin's skill files (which contain only public procedural knowledge and placeholders).

---

## 4. Provenance of the plugin's content

Every section of every skill file in this plugin traces to one or more of the following public sources:

- The Constitution of India.
- The Code of Civil Procedure 1908 and its Schedules.
- The Bharatiya Nagarik Suraksha Sanhita 2023 (and the Code of Criminal Procedure 1973 for transitional reference).
- Substantive statutes (Specific Relief Act 1963, Contract Act 1872, TPA 1882, Indian Succession Act 1925, BNS 2023 / IPC 1860, BSA 2023 / IEA 1872).
- The Limitation Act 1963.
- The Court-Fees Act 1870 (and State amendments — referenced generally; user supplies the State-specific text).
- Leading procedural textbooks (cited only as authority for understanding; never quoted verbatim).
- The validation reports from the India-Legal Corpus Pipeline Phase 03, which themselves cite only public-domain authority.

The author of this plugin has not transcribed any drafted prose from any third-party source into this plugin.

---

## 5. Anti-hallucination architecture

(a) The Reader agent halts the pipeline if any statute referenced in the case folder is not supplied as a PDF and is not within the training-data-allowed list.

(b) The Drafter agent does not generate case names + citations from memory. Every citation traces to the user-supplied list, or appears as a `[CITATION NEEDED]` placeholder for the advocate to fill manually.

(c) The Verifier agent runs a fact-by-fact comparison against the case-facts file produced by the Reader, flagging any assertion in the draft that does not trace to a fact in the case-facts file.

(d) The Overseer agent reviews the draft with an opposing-counsel lens and flags weak prayers, attackable defects, and missing limbs of argument.

(e) The audit chain is `case-facts.md` → `format-shell.md` → `draft-v1.docx` → `verification-report.md` → `draft-v2.docx` → `opposing-notes.md` → `final-draft.docx`.

(f) The advocate remains the responsible signatory. The plugin produces a draft; the human advocate reviews, verifies, and signs.

---

## 6. The advocate-of-record discipline

District Court practice does not require an Advocate-on-Record (that is a Supreme Court-specific role). Any advocate enrolled with a State Bar Council may appear before a District Court. The plugin recognises this and the Advocate's Signature Block is filled with the appearing advocate's name and Bar Council enrolment number — placeholders until the advocate's actual identity at filing.

The plugin does not act on behalf of any party. The plugin does not file the pleading. The advocate remains the responsible advocate of record.

---

## 7. Bar Council of India Rule 36 compliance

This plugin is published in conformity with Bar Council of India Rules, Part VI, Chapter II, Section IV, Rule 36 (restriction on advertising and solicitation):

(a) The plugin is open-source, released under the MIT licence, free of cost.
(b) The plugin does not offer paid services through this repository.
(c) The plugin does not advertise the author's practice.
(d) The plugin does not solicit retainer engagements.
(e) The plugin is published under the **Wolfgang Rush** open-source brand, a name used to publish open-source legal-tech infrastructure separately from the author's professional advocacy practice.

---

## 8. Liability and warranty disclaimer

The plugin is released under the MIT licence. The licence text reproduces the standard "no warranty" disclaimer.

In addition: the plugin is a drafting aid. The user-advocate retains full professional responsibility for the verification of facts, the accuracy of citations, the correctness of legal grounds, the propriety of the prayer, the validity of the court-fee paid, the correctness of the Schedule of Property, and the signature on every pleading filed before any District Court or subordinate court.

The author of this plugin disclaims liability for any consequence arising from the use of the plugin by any user-advocate.

---

## 9. Contact

All inquiries about this plugin are handled via **GitHub Issues** on the project repository.

This project does not have an email contact channel and does not accept private legal-services inquiries through this repository.

---

## 10. Author

**Rushikesh R. Mahajan**, Advocate, enrolled with the Bar Council of Maharashtra and Goa, practising before the Bombay High Court (Nagpur Bench).

Project published under the **Wolfgang Rush** open-source brand for legal-tech infrastructure.
