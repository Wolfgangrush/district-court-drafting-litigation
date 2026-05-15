# format-from-user.md — District Court Petition · canonical baseline

**Status:** Pre-populated baseline. Adapt or override per the specific petition subtype (Succession / Probate / Letters of Administration / Guardianship / Matrimonial / Maintenance).

---

## Section 1 — Cause Title (Subtype-specific)

```
For Succession Certificate petition:
IN THE COURT OF THE [DISTRICT JUDGE per state-config.md] AT [Place], DISTRICT [District Name]

SUCCESSION CASE NO. _____ OF [YEAR]

In the matter of:
PETITION UNDER SECTIONS 370-381 OF THE INDIAN SUCCESSION ACT, 1925
For grant of a Succession Certificate in respect of the debts and securities of the
LATE [Name of Deceased], deceased

[Petitioner Name]                                ... Petitioner

For Probate petition:
PETITION UNDER SECTIONS 222-276 OF THE INDIAN SUCCESSION ACT, 1925 FOR GRANT OF PROBATE
of the Last Will and Testament dated <DD.MM.YYYY> executed by the LATE [Name of Testator]

For Matrimonial Mutual Divorce:
[Before Family Court Judge OR District Judge per state-config.md]
PETITION UNDER SECTION 13B OF THE HINDU MARRIAGE ACT, 1955 FOR DECREE OF DIVORCE BY MUTUAL CONSENT
```

## Section 2 — Subtype-specific mandatory paragraphs

```
For Succession Certificate:
- Particulars of the deceased: name, last residence, date and place of death.
- Relationship of the Petitioner to the deceased.
- Schedule of Debts and Securities (mandatory under Section 372 ISA, annexed at end of petition).
- Names and addresses of other heirs entitled.
- Statement of no prior application by other heirs (where applicable).

For Probate:
- Particulars of the testator and the date / place of execution of the Will.
- Names of executors appointed in the Will.
- Schedule of Assets covered by the Will (mandatory).
- Names and addresses of all persons entitled to take under the Will.

For Matrimonial Mutual Divorce:
- Date of marriage; place of marriage; marriage certificate reference.
- Statement of separation for one year or more (Section 13B(1) HMA).
- Statement of mutual consent.
- Arrangements for any children (custody, maintenance, education).
- Settlement on alimony / streedhan / property.
```

## Section 3 — Prayer (subtype-specific)

```
For Succession Certificate:
Grant a Succession Certificate to the Petitioner in respect of the debts and securities specified in the Schedule attached hereto.

For Probate:
Grant Probate of the last Will and Testament of the deceased dated <DD.MM.YYYY> to the Petitioner-Executor named therein.

For Matrimonial Mutual Divorce:
Pass a decree of divorce by mutual consent under Section 13B of the Hindu Marriage Act, 1955 dissolving the marriage solemnized on <DD.MM.YYYY> at [Place].
```

## Sections 4-5 — Supporting Affidavit + Advocate Signature Block

(Same patterns as `specific-relief-suit-draft/format-from-user.md` Sections 6-7.)

---

**Customization:** override any section. Drafter falls back to these baselines.
