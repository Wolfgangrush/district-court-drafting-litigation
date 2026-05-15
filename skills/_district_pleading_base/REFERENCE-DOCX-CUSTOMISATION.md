# `reference.docx` — Customisation Guide

The `reference.docx` in this folder is the **pandoc template** the Drafter agent uses when converting markdown drafts to `.docx`. Customise once for District-Court formatting alignment with your State's conventions.

---

## Why customise

District-Court formatting conventions vary by State and by the practice of the local Registry / Office. Pandoc's default `.docx` styles are office-document-generic. Customising `reference.docx` once aligns every future Drafter output with the conventions you actually use.

---

## District Court formatting (typical, varies by State)

| Element | Typical specification |
|---|---|
| Paper size | A4 (some practices use Legal — verify locally) |
| Print | One-side (preferred) |
| Font (body) | Times New Roman, 12-14 point |
| Line spacing | 1.5 (some State practices use double) |
| Left margin (binding side) | 4 cm |
| Top / bottom / right margin | 2.5 cm |
| Section headers | Title Case, bold |
| Page numbers | Centred at bottom of page |

**State-specific variations:** check your State's Civil Manual / Criminal Manual for the locally-prescribed format. Examples:
- Maharashtra (Bombay Civil Manual) — TNR 12, 1.5 spacing, A4
- Karnataka — TNR 14, 1.5 spacing, A4
- Tamil Nadu (Madras Civil Manual) — TNR 12, double spacing, A4
- Delhi — TNR 14, 1.5 spacing, A4

Verify against your practice State before finalising.

---

## How to customise (one-time, ~10 minutes)

1. **Open the file in Microsoft Word or LibreOffice:**
   ```bash
   open "<your-anthropic-plugins-folder>/district-court-drafting/skills/_district_pleading_base/reference.docx"
   ```

2. **Set page size and margins** (Layout / Page Setup) to your State's specification.

3. **Set body style** (Home / Styles / Normal) — font, point size, line spacing.

4. **Set heading styles** (Heading 1, 2, 3) — bold, point size, spacing before/after.

5. **Add page numbers** (Insert / Page Number).

6. **Save the file** (Cmd-S / Ctrl-S). Keep filename `reference.docx`.

---

## What NOT to customise

- No header / footer text content. The reference is a STYLE template only.
- Do not change the file location or filename.
- Do not commit a `reference.docx` with case-specific content.

---

## How the Drafter uses it

```bash
pandoc draft-v1.md -o draft-v1.docx \
  --reference-doc="${CLAUDE_PLUGIN_ROOT}/skills/_district_pleading_base/reference.docx"
```

Pandoc reads the styles from your customised `reference.docx` and applies them to every section of the draft.

---

## Fallback if pandoc is unavailable

The Drafter falls back to `python-docx`. Install pandoc for best results:

```bash
# macOS
brew install pandoc

# Linux (Debian/Ubuntu)
sudo apt install pandoc
```
