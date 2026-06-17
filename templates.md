# Output Templates (English-only, layout-mirrored → Word + PDF)

Deliverables: **English-only** `.docx` + `.pdf` that **strictly mirror the Chinese original layout**.

## Workflow

1. Analyze source layout → [layout-reference.md](layout-reference.md)
2. Build `translation.json` with blocks in **identical order** to the original
3. Export:
   ```bash
   python3 -m pip install python-docx fpdf2
   python ~/.cursor/skills/translate-academic-credentials/scripts/build_translation_docs.py translation.json
   ```

## Top-level JSON fields

| Field | Purpose |
|-------|---------|
| `mirror_source_layout` | `true` (default) — no auto headings; render blocks only |
| `suppress_default_title` | `true` — omit generic "OFFICIAL TRANSLATION" banner |
| `basename` | Output filename stem |
| `output_dir` | Save directory |
| `page` | Optional: `orientation`, margins |
| `sections` | Ordered layout blocks (required) |
| `certification_statement` | Optional; **new page after** mirrored content |

## Block types

### `paragraph`

```json
{
  "type": "paragraph",
  "text": "Degree Certificate",
  "align": "center",
  "bold": true,
  "font_size": 18,
  "space_before": 12,
  "space_after": 6
}
```

### `field_row` (label–value pairs on one line)

Mirrors rows like `姓名：张三  学号：2020123456`.

```json
{
  "type": "field_row",
  "pairs": [
    { "label": "Name", "value": "Zhang, Wei" },
    { "label": "Student ID", "value": "2020123456" }
  ],
  "separator": "    "
}
```

### `table`

Match source column count and row order. Set `"headers": null` if the source has no header row.

```json
{
  "type": "table",
  "headers": ["Course Title", "Course Code", "Credits", "Grade", "Semester"],
  "rows": [["Advanced Mathematics", "MATH101", "4", "92", "2020-2021, Sem 1"]],
  "column_widths": [30, 15, 10, 10, 35],
  "align": "center"
}
```

### `spacer`

```json
{ "type": "spacer", "lines": 2 }
```

### `page_break`

```json
{ "type": "page_break" }
```

### `seal`

```json
{
  "type": "seal",
  "text": "[Official Seal: Peking University]",
  "align": "center",
  "space_before": 24
}
```

### `footer` / `note`

```json
{
  "type": "footer",
  "text": "Page 1 of 3",
  "align": "center",
  "font_size": 9
}
```

## Example: mirrored degree certificate

```json
{
  "mirror_source_layout": true,
  "suppress_default_title": true,
  "basename": "Zhang_Wei_Degree_Certificate_EN",
  "output_dir": "/path/to/output",
  "sections": [
    { "type": "paragraph", "text": "People's Republic of China", "align": "center", "font_size": 12 },
    { "type": "paragraph", "text": "Peking University", "align": "center", "bold": true, "font_size": 18, "space_before": 6 },
    { "type": "spacer", "lines": 1 },
    { "type": "paragraph", "text": "Degree Certificate", "align": "center", "bold": true, "font_size": 16, "space_after": 12 },
    { "type": "paragraph", "text": "This is to certify that", "align": "center", "font_size": 12 },
    { "type": "paragraph", "text": "Zhang, Wei", "align": "center", "bold": true, "font_size": 14, "space_before": 6, "space_after": 6 },
    { "type": "paragraph", "text": "having completed the requirements of the Computer Science and Technology program, is conferred the degree of", "align": "center", "font_size": 12 },
    { "type": "paragraph", "text": "Bachelor of Engineering", "align": "center", "bold": true, "font_size": 14, "space_before": 6, "space_after": 12 },
    { "type": "paragraph", "text": "Date of conferral: June 30, 2024", "align": "center", "font_size": 12 },
    { "type": "spacer", "lines": 2 },
    { "type": "paragraph", "text": "Degree Certificate No.: 102011234567890", "align": "center", "font_size": 10 },
    { "type": "spacer", "lines": 3 },
    { "type": "field_row", "pairs": [
      { "label": "President", "value": "Gong Qihuang" },
      { "label": "Chair, Degree Evaluation Committee", "value": "Li Ming" }
    ]},
    { "type": "seal", "text": "[Official Seal: Peking University]", "align": "center" }
  ]
}
```

## Example: mirrored transcript (excerpt)

```json
{
  "mirror_source_layout": true,
  "suppress_default_title": true,
  "basename": "Zhang_Wei_Academic_Transcript_EN",
  "output_dir": "/path/to/output",
  "sections": [
    { "type": "paragraph", "text": "Peking University", "align": "center", "bold": true, "font_size": 16 },
    { "type": "paragraph", "text": "Official Academic Transcript", "align": "center", "font_size": 14, "space_after": 12 },
    { "type": "field_row", "pairs": [
      { "label": "Name", "value": "Zhang, Wei" },
      { "label": "Student ID", "value": "2020123456" }
    ]},
    { "type": "field_row", "pairs": [
      { "label": "Major", "value": "Computer Science and Technology" },
      { "label": "Department", "value": "School of Information" }
    ]},
    { "type": "spacer", "lines": 1 },
    {
      "type": "table",
      "headers": ["Course Title", "Course Code", "Credits", "Grade", "Semester"],
      "rows": []
    },
    { "type": "field_row", "pairs": [
      { "label": "Total Credits", "value": "160" },
      { "label": "GPA", "value": "3.72" }
    ]},
    { "type": "footer", "text": "Page 1 of 2", "align": "center", "font_size": 9 }
  ]
}
```

## File naming

| Document | basename |
|----------|----------|
| Transcript | `{Name}_Academic_Transcript_EN` |
| Degree certificate | `{Name}_Degree_Certificate_EN` |
| Graduation certificate | `{Name}_Graduation_Certificate_EN` |
