---
name: read
description: "Manual-invoke only. Load when user explicitly says 'use read,' 'use read to read literature,' '/read.' Literature survey and batch close-reading for Chinese geosciences/resource-environment papers: (A) Before topic selection, ask discipline/direction/target journal/reference quality, then search and rank literature; (B) Batch-read papers from a Zotero collection, each producing a structured note (language, section tree, tables/figures, data sources, methods, conclusions, citable points) to local folders; (C) Cross-paper comparison matrix and research gap identification. Depends on zotero-cli and PDF extraction."
---

# read — Literature Survey & Batch Close-Reading Notes

Breaks "reading a batch of papers before writing" into two controllable phases: **A. Pre-topic survey → B. Batch close-reading to notes → C. Cross-paper synthesis.** Each paper read gets a local `.md` note — persistent, grep-able, not dependent on AI memory.

## Iron Rules

1. **No fabrication.** Section headings, data, formulas, conclusions must come from text actually read in the PDF; if unreadable, mark "not extracted / needs manual review," do not guess from abstracts.
2. **Stop if zotero-cli unavailable.** Run `zotero-cli config` first; if local or web mode fails, tell the user — do not substitute "what I remember about this field."
3. **Every note is written to disk.** After each paper, write `Article/<title>.md` and append a card to `ArticleMatrices.md`; never summarize only in conversation after a batch.
4. **Batch processing.** 3–5 papers per batch; write to disk between batches to avoid context overflow losing prior reads.
5. **Ask before doing.** Phase A requires discipline/direction/target journal/quality first; Phase B requires collection Key and output folder confirmed.

## Output Structure (fixed)

```
<notes folder>/
├── Article/
│   ├── <PaperTitleA>.md      # One per paper; tables embedded as markdown; figures described in text only (no PNG extraction)
│   └── ...
├── ArticleMatrices.md         # [Directory cards] rough index per paper; AI scans this first
└── _阅读进度.md               # Read / to-read / abandoned
```

- **Article/<title>.md:** fill per [references/paper-note-template.md](references/paper-note-template.md). Filename = full paper title (truncate to 30 chars if long; remove `\ / : * ? " < > |`).
- **Tables:** extract with pdfplumber, convert to markdown, embed inline under the corresponding section; do not save as separate CSV.
- **Figures:** **do NOT extract PNGs or embed image files.** In the md, write: "Fig N: <title> — <brief description/analysis, 2–3 sentences>." If unreadable: "Fig N: <title> (p. N, auto-read failed, needs manual review)." Do not guess.
- **ArticleMatrices.md:** **not a deep comparison table — a "quick-locate card."** Append a paragraph per paper (see [references/article-matrices-template.md](references/article-matrices-template.md)). When AI later needs "who did Henan water ecological footprint" or "who used spatial autocorrelation," it scans this 20-line-per-paper card first, then decides which Article file to open — avoiding stuffing 50 PDFs into context.
- **_阅读进度.md:** one line per paper (title / status / note path).

## Three Entry Points

**A. Pre-topic survey (no Zotero collection yet)**

Ask four things; ask for whichever is missing:
1. Discipline & research direction (e.g. water ecological footprint / Henan)
2. Target journal (e.g. *Journal of Cleaner Production*) — **reference quality aligns with target journal**
3. Time range (e.g. last 10 years / classic papers unlimited)
4. Approximate screening count (e.g. 30–50 into collection, close-read 10–15)

Then:
- Search existing library with `zotero-cli search` / `search --mode semantic`;
- If insufficient, **provide Boolean search strings in Chinese and English** (below) for the user to export RIS/BibTeX from Google Scholar / Web of Science and import to Zotero (do not click around the web for the user);
- Provide **initial ranking**: score on five dimensions below (read / optional / skip) and let user confirm the close-reading list.

### Boolean Search String Template

**Chinese (CNKI / Wanfang):**
```
主题=(水资源生态足迹 OR 水足迹) AND (河南 OR 河南省) AND (时空 OR 空间 OR 动态)
```

**English (Web of Science / Scopus):**
```
("water ecological footprint" OR "water footprint")
AND (Henan OR "Henan Province")
AND (spatial OR "spatiotemporal" OR "temporal variation")
```

Rules:
- Synonyms grouped with OR, different concepts connected with AND;
- Time range set via database filters (e.g. PY=2015–2025), not in the string;
- Start broad; narrow with AND (e.g. add method name) if too many results, or remove an OR synonym if too few.

### Five Dimensions for "Should I Read This" (ignore journal brand)

Score each candidate 1–5 on five dimensions; total ≥18 must-read, 12–17 optional, <12 skip:

| Dimension | Question |
|---|---|
| **Method similarity** | Does it use methods/models you plan to use (footprint? spatial autocorrelation? LMDI?) |
| **Architecture borrowability** | Can you directly adapt its section organization and analytical path? |
| **Explanation style** | How does it connect data phenomena to meaning — can you learn that bridge? |
| **Novelty** | What new thing did it introduce (new indicator/data/method)? |
| **Field foundationalness** | Is it a classic / widely cited source paper in this direction? |

**Dimensions NOT scored:** impact factor, top-journal status, author titles. These are irrelevant to "should I learn from it."

**B. Batch close-reading (Zotero collection ready)**

Ask:
- Collection Key (e.g. `LWF2MDA4`);
- Local notes output folder;
- Which papers this batch (or "start from the front of the collection").

Then run the per-paper extraction process below.

**C. Cross-paper synthesis (optional after ≥5 papers)**

`ArticleMatrices.md` is a shallow card appended per paper. For deep synthesis, generate `_综合矩阵.md`: rows = methods/data/conclusions/gaps, columns = papers, ending with "research gap and this paper's entry point." Template: [references/collection-synthesis-template.md](references/collection-synthesis-template.md). If user does not explicitly request deep synthesis, maintain only ArticleMatrices.md.

**Research gaps must map to six types, not made up:**

| Type | Definition | How to find from notes |
|---|---|---|
| Knowledge gap | X known, Y unknown | All did A, none did B |
| Empirical gap | Theory exists, missing region/time/sample | All on Yangtze/national scale, none on Henan 18-city long series |
| Method gap | Method not yet applied | All only accounted, none used LMDI/spatial autocorrelation |
| Object gap | Research concentrated on certain objects | All basin/province scale, missing city scale |
| Context gap | Other regions done, local not | Footprint research in North China over-exploitation areas scarce |
| Practical gap | Theory-policy gap | Accounting done, but management zoning unclear |

When writing the gap paragraph, first state which types this paper occupies, then "this paper approaches from X angle."

## Per-Paper Extraction Process (run for each)

For a PDF:

1. **Metadata:** authors, year, title, journal, DOI, Zotero Key, language (ZH/EN).
2. **Section tree (H1/H2/H3):** use `zotero-cli outline <key>` or PDF bookmarks; heuristically split by font size/bold if no TOC. This is the index for later fast AI positioning.
3. **Per-section summary:** introduction / data & methods / results / discussion / conclusion, 3–5 sentences each.
4. **Tables & figures (all embedded as text in this md):**
   - Tables: pdfplumber → markdown, embedded inline;
   - Figures: **no PNG extraction**; write "Fig N: <title> — <description/analysis, 2–3 sentences>"; unreadable → "Fig N: (p. N, auto-read failed, needs manual review)."
5. **Data & data sources:** What data? What years, spatial extent, source (yearbook / remote sensing / model output / survey)? Units? This is what the user most needs for reproduction.
6. **Formulas & methods:** key formulas (single-line LaTeX), parameter values, criteria.
7. **Conclusions & citable points:** What is the author's final claim? Which sentences can be cited as "existing studies have shown…" — **excerpt original sentences** (ZH or EN); write skill uses these directly as Language 2 material.
8. **Critique / borrowable / attackable:** Method limitations? Data problems? Where can the user "do better"?
9. **Which section of user's paper this serves:** introduction / methods / results comparison / discussion — tag it.

Template: [references/paper-note-template.md](references/paper-note-template.md).

## Workflow

1. **Confirm entry:** A (no collection) or B (collection ready).
2. **Phase A:** clarify discipline/direction/journal/quality/quantity → search + rank → user confirms close-reading list.
3. **Phase B:** confirm collection Key and output folder → per batch of 3–5:
   - `zotero-cli get metadata <key>`;
   - `zotero-cli get fulltext <key>` or `read <key> --start-page --end-page`;
   - Use Python (pdfplumber) to extract tables as markdown; describe figures in text only.
   - Write `Article/<title>.md` to disk;
   - Append card to `ArticleMatrices.md`;
   - Update `_阅读进度.md`.
4. **Phase C (optional):** after ≥5 papers and user requests deep synthesis, generate `_综合矩阵.md` and gap paragraph.
5. **Deliver:** tell user notes folder path, papers read, ArticleMatrices.md location.

## Pre-Delivery Self-Check

- [ ] Every note written to `Article/<title>.md`, not just summarized in conversation.
- [ ] Each note includes: section tree, data sources, key formulas, citable originals, critique/entry points.
- [ ] After each paper, ArticleMatrices.md card appended.
- [ ] Tables embedded as markdown; figures described in text (no PNGs); unreadable figures marked "needs manual review."
- [ ] _阅读进度.md updated.
- [ ] No conclusions guessed from abstracts; all conclusory sentences marked with page or original-sentence location.
