# Layout Fidelity Reference

Mirror the **visual and structural layout** of the Chinese original. Translation changes language only — not document architecture.

## Analysis checklist (do before translating)

Study the source page-by-page, top-to-bottom:

1. **Page setup**: portrait/landscape, approximate margins, page count
2. **Block sequence**: exact order of titles, fields, tables, notes, seals
3. **Alignment**: center titles, left body, right dates, justified paragraphs
4. **Typography**: title vs body vs footer relative sizes; bold labels
5. **Tables**: column count/order, header rows, subtotal rows, merged cells, empty cells
6. **Field layout**: inline `Label: Value` vs table rows vs multi-column label grids
7. **Whitespace**: blank lines, section gaps, vertical spacing before signatures
8. **Repeated elements**: page headers/footers, watermarks, page numbers
9. **Certificate frames**: border text, centered stacked lines, signature block positions

Record findings in JSON `sections` — one block per visible source block, same order.

## Do / Don't

| Do | Don't |
|----|-------|
| Keep same page count | Collapse multi-page tables onto one page |
| Keep same table columns (translated headers) | Rebuild as "Field / Value" summary table |
| Keep label-value pairs on same row | Split into separate heading + paragraph |
| Preserve blank lines and spacing | Auto-add "Student Information" sections |
| Center certificate titles if source is centered | Left-align everything |
| Place seal/signature blocks where they appear | Move seals to document end by default |
| Translate footer/page number position | Drop page numbers |

## Common Chinese layouts → English mirror

### Transcript header (4-column grid)

Source pattern:
```
姓名  张三    学号  2020123456
专业  计算机  院系  信息学院
```

Mirror as `field_row` with two pairs, or 4-column table — match source structure.

### Transcript course table

Keep identical columns. Translate headers only:

| 课程名称 | 课程代码 | 学分 | 成绩 | 学期 |
→ | Course Title | Course Code | Credits | Grade | Semester |

Row count must match source exactly, including blank/spacer rows.

### Degree certificate (stacked center)

Typical vertical stack, all centered:
1. Country / ministry line (if present)
2. Institution name (large, bold)
3. Document title: "Degree Certificate"
4. Body paragraph(s)
5. Name (prominent)
6. Major / degree line
7. Conferral date
8. Certificate number (often smaller, bottom area)
9. Signatures (left/right or stacked)
10. Seal description at original position

Use `paragraph` blocks with `"align": "center"` and matching `font_size` / `bold`.

### Border / decorative frame

If source has a printed border, add at document start:
```json
{ "type": "note", "text": "[Decorative border as on original]", "align": "center", "font_size": 9 }
```

Do not draw borders unless user requests graphic reproduction.

## Alignment values

Use in JSON blocks: `left` | `center` | `right` | `justify`

## Font size guide (match relative prominence)

| Source role | suggested `font_size` |
|-------------|----------------------|
| Main certificate title | 18–22 |
| Institution name | 16–18 |
| Section title | 14 |
| Body text | 11–12 |
| Certificate no. / footer | 9–10 |

Adjust per source — these are defaults when exact size is unknown.

## Multi-page rules

- Insert `{ "type": "page_break" }` at the same breaks as the source
- Repeat page header blocks if every page repeats them in the original
- End each page with `{ "type": "footer", ... }` if source has footers

## Certification statement

Append on a **new page** after the mirrored translation only if the user wants a translator certificate. It is **not** part of the source layout — keep it separate from mirrored pages.
