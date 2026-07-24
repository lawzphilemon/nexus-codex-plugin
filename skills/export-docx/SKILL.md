---
name: export-docx
description: Export a finalized or converted NEXUS article to a verified Word DOCX with an SEO metadata table, real heading styles for the article, and JSON-LD schema at the end. Use after $finaldraft or $convert, or when the user asks to save a NEXUS article as .docx.
---

# NEX-X - DOCX Export

**Mission:** Turn the latest complete article into a clean, verified DOCX without changing its wording, links, hierarchy, CTA blocks, metadata, or schema.

**Dependency:** Require complete NEX-Fn or NEX-U output containing the article, On-Page SEO Pack, and Schema Markup. If any part is missing, recover it from the current conversation or ask the user for the missing output.

Retrieve and follow the available `documents` skill before creating the file. Use its workspace runtime, DOCX authoring, render, and visual QA workflow. Do not create a renamed Markdown or HTML file.

## Source

- Use the latest article version. If `$convert` ran, export the converted article.
- Extract `Title tag`, `URL slug`, and `Meta description` from the On-Page SEO Pack.
- Use the H1 as the article title only when `Title tag` is missing.
- Preserve the Schema Markup exactly as valid JSON-LD.

## DOCX structure

1. Start with one two-column metadata table:

   | Field | Value |
   |---|---|
   | Judul Artikel | Title tag |
   | Slug | URL slug |
   | Meta Description | Meta description |

2. Continue with the complete article:
   - Apply real Word styles: article title as `Title`, H1 as `Heading 1`, H2 as `Heading 2`, H3 as `Heading 3`, and so on.
   - Preserve paragraphs, bold and italic emphasis, real lists, tables, links, and their original order.
   - Keep injected CTA HTML as plaintext code blocks in the exact inline positions so it remains paste-ready for WordPress Classic Editor.
   - Do not include the remaining On-Page SEO Pack fields as a second metadata section.

3. End with a `Schema Markup (JSON-LD)` heading followed by the complete schema in a monospaced code block.

## File and QA

- Name the file from the slug without its leading slash, falling back to a filesystem-safe title.
- Save the final `.docx` in the user-facing output directory.
- Render the DOCX to page images and inspect every page. Fix broken tables, clipped text, heading hierarchy, code wrapping, and awkward page breaks, then re-render.
- If rendering is unavailable only because LibreOffice is missing, perform structural QA and disclose that visual QA was skipped.
- Deliver only the final DOCX link. Do not deliver render images or temporary files.

End with: "DOCX exported - metadata table, formatted article, and schema included."
