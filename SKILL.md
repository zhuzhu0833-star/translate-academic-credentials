---
name: translate-academic-credentials
description: >-
  Translates Chinese academic transcripts (成绩单), degree certificates (学位证),
  graduation certificates (毕业证), and related credential documents into English-only
  Word and PDF files that strictly mirror the original Chinese layout and formatting.
  Use when the user mentions 成绩单, 学位证, 毕业证, 学历证明, academic transcript,
  degree certificate, credential translation, layout-matched translation, or saving
  translation as Word/PDF.
---

# Translate Academic Credentials (ZH → EN)

## Quick start

1. **Identify document type**: transcript, degree certificate, graduation certificate, or verification letter.
2. **Analyze layout first** — page-by-page, top-to-bottom. See [layout-reference.md](layout-reference.md).
3. **Ask if missing**: output folder, file basename, and official English institution names.
4. **Read the source** (image/PDF/text). Extract every field, table cell, stamp, watermark, footer, and blank line.
5. **Translate to English only** using [reference.md](reference.md). **Do not include Chinese text** in output files.
6. **Build layout-mirrored JSON** — blocks in the **same order, alignment, and structure** as the original. See [templates.md](templates.md).
7. **Export**:
   ```bash
   python3 -m pip install python-docx fpdf2
   python ~/.cursor/skills/translate-academic-credentials/scripts/build_translation_docs.py translation.json
   ```
8. **Verify layout fidelity** against the checklist below, then deliver `.docx` and `.pdf` paths.

## Layout fidelity (strict — required)

The English document must **mirror the Chinese original format**, not be reorganized into a generic template.

### Structure
- Same **page count** and **page breaks**
- Same **block order** (titles → fields → tables → signatures → seals → footers)
- Same **table dimensions** (columns, rows, including empty rows)
- Same **label–value pairing** (if two fields share one row in the source, keep them on one row)
- Preserve **blank lines** and vertical spacing with `spacer` blocks

### Typography & alignment
- Match **center / left / right / justify** per block
- Match relative **font size** and **bold** (titles larger, certificate numbers smaller)
- Certificate text: preserve **stacked centered** lines if that is how the source appears

### Forbidden layout changes
- Do **not** convert source tables into summary "Field | Value" tables unless the source uses that layout
- Do **not** add section headings (e.g., "Student Information") absent from the source
- Do **not** merge or split rows/columns for convenience
- Do **not** move signatures, dates, or seals to different positions
- Do **not** add a generic "OFFICIAL TRANSLATION" banner unless the user requests it (`suppress_default_title: true` by default)

## Output rules

### English-only deliverables
- Word/PDF contain **English translation only** — no Chinese, no bilingual columns
- Institution names: official English only

### Required file outputs
Every translation produces **both**:
- `{basename}.docx`
- `{basename}.pdf`

Default naming: `{FamilyName}_{GivenName}_{DocumentType}_EN`

### Translator certification
If included, `certification_statement` goes on a **separate page after** the mirrored translation — it is not part of the source layout.

## Core translation rules

- Translate **all** visible text; use `[Illegible]` when unreadable
- Do **not** summarize, reinterpret grades, or convert GPA unless asked
- **Names**: Pinyin, `Family Name, Given Name`
- **Dates**: `Month DD, YYYY` unless source format must be preserved
- **Seals**: `[Official Seal: Institution Name]` at the original position
- Warn about ID numbers; offer redaction if needed

## Document-specific layout notes

### 成绩单
- Mirror header grid (often multi-column label rows, not a single table)
- Course table: translate column headers; keep every data row
- Summary row (总学分, GPA): same position and pairing as source
- Repeat per-page headers if the source repeats them

### 学位证 / 毕业证
- Mirror vertical certificate stack (centered lines, spacing, signature block)
- Keep certificate number placement (often lower on page)
- 学位证 vs 毕业证: use correct English title; do not swap document types

## Export workflow

1. Set `"mirror_source_layout": true` and `"suppress_default_title": true` in JSON
2. Run `build_translation_docs.py`
3. PDF is generated from DOCX when LibreOffice/Pandoc is available (best layout match); otherwise fpdf2 fallback
4. Confirm both files exist

## Quality checklist

### Layout
- [ ] Page count matches source
- [ ] Block order matches source top-to-bottom
- [ ] Alignments (center/left/right) match source
- [ ] Table column count and row count match source
- [ ] Label–value pairs on same rows as source
- [ ] Spacing, page breaks, footers, seals at correct positions
- [ ] No generic reformatting or added section headings

### Content
- [ ] English only in output files
- [ ] Both `.docx` and `.pdf` generated
- [ ] All fields translated; numbers/IDs exact
- [ ] Terminology matches [reference.md](reference.md)
- [ ] No unauthorized GPA conversion

## Additional resources

- Layout patterns: [layout-reference.md](layout-reference.md)
- Terminology: [reference.md](reference.md)
- JSON schema & examples: [templates.md](templates.md)
