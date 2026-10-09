# paper-writing-skills

A set of manual-invoke skills for an AI-assisted academic paper writing pipeline, designed for Chinese geosciences / resource-environment papers (water ecological footprint, spatial statistics, etc.).

## Skills

| Skill | Purpose | Trigger |
|---|---|---|
| **read** | Literature survey & batch close-reading. Reads a Zotero collection, produces per-paper markdown notes, a quick-locate card matrix, and research-gap synthesis. | User says "use read" / "/read" |
| **write** | Section-by-section drafting with per-sentence source traceability. Enforces two-language layering (author transitions vs. cited literature), anti-AI-tone rules, anti-hallucinated-citation rules, and a process file mapping every claim to a Zotero original. | User says "use write" / asks to draft a section |
| **engine** | One-command project skeleton. `ENGINE START` generates an 8-directory folder structure with templates (outline, terminology table, figure list, experiment log). | User types `ENGINE START` |
| **p** | Figure planning. Reads the thesis outline + reference-paper card matrix, inventories available data, and outputs a per-section figure plan with data-sufficiency assessment. | User says "what should I plot" / "draw a figure" |

## Repository Layout

```
├── zh/                    # Chinese versions (original)
│   ├── write/SKILL.md
│   ├── write/references/
│   ├── read/SKILL.md
│   ├── read/references/
│   ├── engine/SKILL.md
│   ├── engine/references/
│   └── p/SKILL.md
└── en/                    # English versions
    ├── write/SKILL.md
    ├── read/SKILL.md
    ├── engine/SKILL.md
    └── p/SKILL.md
```

## Design Principles

- **Manual-invoke only:** no skill auto-triggers; the user explicitly calls each one.
- **No fabrication:** every formula, parameter, and citation must trace to a real Zotero source read that session.
- **No full-text generation:** write produces one section at a time and stops for user review.
- **No analytical conclusions in drafts:** interpretation and experimental feedback go in conversation, not in the .md draft.
- **Data traceability:** every plotted data point must trace from figure → CSV → raw data.

## Dependencies

- [zotero-cli](https://github.com/) for reading literature from a local Zotero library.
- PDF extraction via Python (pdfplumber) for tables.
- Plotting tools used by the user: Origin, ArcMap, GeoDa (skills do not draw figures).
