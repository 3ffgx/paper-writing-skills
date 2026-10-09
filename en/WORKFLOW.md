# Academic Paper AI-Assisted Writing Workflow

> A custom skill set designed for Chinese geography / resource & environment SCI papers.
> Core principle: AI assists, it does not think for you; every citation must be traceable; write one section at a time, never generate the full paper.

---

## 1. What This Workflow Is

When you talk to an AI (Doubao / Codex / any skill-enabled client), you manually type specific commands to trigger 4 independent skills. Each skill owns one stage of the paper-writing pipeline and never oversteps into another:

```
ENGINE START          → scaffold project folders (one-time)
"use read" / "/read"  → read literature, write notes
"use write" / "write X.X section"  → draft one section
"draw figures" / "what can this data plot"  → plan figures, assess data
```

**Key design: all skills are manually triggered.** If you don't issue the command, the AI won't start writing or plotting on its own.

---

## 2. Trigger Commands → What Happens

| You say | Skill triggered | What AI does |
|---|---|---|
| `ENGINE START` | **engine** | Generate 8 English-named folders + README + 3 templates (outline / terminology / figure list) in the current directory |
| "use read" / "/read" | **read** | Ask your field / target journal → give search strings → you import RIS into Zotero → AI batch-reads papers, writes one .md note per paper |
| "use write, write section 3.1" | **write** | First ask what to write, boundary, required citations → read Zotero originals → produce draft .md + traceability data.md → **stop and wait for your review** |
| "draw figures" / "what can this data plot" | **p** | Read your outline + reference notes + available data → output a summary table (what to plot per section, do you have the data, what's missing) |

### What does NOT trigger

- Vague "help me write my paper" → **does not trigger write**; AI asks which skill to use
- Casual "what does this paper say" → **does not trigger read**; that's normal chat
- No `ENGINE START` typed → **no folders created**

---

## 3. The Full Pipeline (7 stages)

### Stage 0: Project initialization (engine, one-time)

```
You type in project root: ENGINE START
```

AI creates:

```
project-root/
├── 01_data/raw/              ← raw data (read-only, AI must not touch)
├── 02_literature/            ← search strings, exported RIS/BibTeX
├── 03_papers/                ← read skill workspace
│   ├── Article/              ←   one .md per paper
│   ├── ArticleMatrices.md    ←   quick-locate card index
│   └── _reading-progress.md
├── 04_experiments/
│   ├── code/                 ← your Python/R scripts
│   ├── results/              ← script outputs
│   └── logs/                 ← append a log per parameter change
├── 05_outline/               ← thesis core argument, section tree
│   ├── outline.md
│   ├── terminology.md
│   └── figure-list.md
├── 06_temp/                  ← AI scratch files, clean before submission
├── 07_draft/                 ← write skill draft output
└── 08_figures/               ← p skill figure workspace
    ├── origin/
    ├── arcmap/
    ├── geoda/
    └── python/
```

### Stage 1: Literature search (read, phase A)

```
You say: "use read, I want to write about Henan water ecological footprint, target journal JCP"
```

AI asks 4 things: field / target journal / time range / how many papers to deep-read. Then:

1. Search your existing Zotero library
2. If insufficient, give you Boolean search strings (CNKI / Web of Science); you export RIS yourself and import into Zotero
3. Score candidates on 5 dimensions (method similarity / architecture borrowability / explanation style / novelty / field foundational) — **impact factor is NOT a criterion**
4. Tier into must-read / should-read / skim; you confirm which go into the deep-read list

### Stage 2: Batch deep reading (read, phase B)

```
You say: "use read, collection key LWF2MDA4, start from the front"
```

AI reads 3-5 papers per batch. For each:

1. Use zotero-cli to read the PDF in Zotero
2. Use pdfplumber to extract tables as markdown
3. Do NOT extract PNG images; write in .md: "Fig N: <caption> — <2-3 sentence description>"
4. Save to `03_papers/Article/<title>.md`
5. Append a card to `ArticleMatrices.md` (≤10 lines, includes "what figures/tables used")
6. Update `_reading-progress.md`

### Stage 3: Outline & core argument (you + AI discussion)

In `05_outline/outline.md`, write:
- Paper title
- **Core argument (one sentence)**: what this paper proves
- Section tree (level 1/2/3)
- What each section does

This step is mainly yours. AI can help you extract research gaps from read notes (6 categories: knowledge / empirical / method / object / context / practice gap).

### Stage 4: Figure planning (p)

```
You say: "draw figures"
```

AI does full-figure planning:

1. Read `05_outline/outline.md`: how many Results subsections
2. Read `03_papers/ArticleMatrices.md`: how many figures reference papers used, what types
3. Inventory `01_data/raw/` and `04_experiments/results/`: what data you have
4. Output a summary table:

| Section | Suggested figure | Reference | Data needed | Have it? | Missing |
|---|---|---|---|---|---|
| 4.1 | per-capita WEF time-series line | He2024 Fig2 | yearly EF/EC | ✅ | none |
| 4.3 | Moran scatter + LISA | Rey2006 | spatial weights | ❌ | build queen weights in GeoDa |

5. You confirm section by section which to adopt
6. After confirmation, AI creates `FigX_name/` folder under `08_figures/`

### Stage 5: Section-by-section writing (write)

```
You say: "use write, section 3.1 water ecological footprint model"
```

AI's standard flow:

1. **Clarify first**: what to write, output filename, which formulas, required citations
2. **Set section boundary**: list "what this section covers / excludes" — you confirm
3. **Read terminology** (`05_outline/terminology.md`) for naming consistency
4. **Inventory data**: read your calculation workbook (e.g. main2.xlsx)
5. **Pull literature**: zotero-cli reads Zotero originals (if unavailable, stop; do not fabricate)
6. **Learn narrative + tone**: read 2-3 English methods papers for sentence structure, 2-3 Chinese core journals for transitions
7. **Write draft** to `07_draft/3.1/3.1.md`:
   - Continuous paragraphs, no numbering, no bullet lists
   - **Language 1** (author transitions, not bold): what data, how converted
   - **Language 2** (cited literature, **all bold**): definitions, formulas, parameters
8. **Write traceability** to `07_draft/3.1/3.1data.md`:
   - Every Language 2 sentence → original quote + Zotero Key
   - Formula ↔ evidence mapping
   - "Content considered but not used, and why"
9. **Stop**: hand both files to you, wait for "ok, next section" or "fix this"

### Stage 6: You draw figures + assemble in Word

- You plot in Origin / ArcMap / GeoDa per p's plan
- Figures saved to `08_figures/<platform>/FigX_name/`
- Final assembly in Word: insert figures, use Zotero Word plugin to insert citations (the `[n]` in .md is just a placeholder)

---

## 4. Environment & Tool Dependencies

### Required

| Tool | Purpose | Setup |
|---|---|---|
| **Zotero desktop** | Literature library, source of all citations | Running, local mode |
| **zotero-cli** | AI reads Zotero from command line | Installed; verify with `zotero-cli config` |
| **Windows** | OS | — |
| **Python + pdfplumber** | Extract tables from PDFs to markdown | pip install pdfplumber |
| **Microsoft Word** | Final typesetting | Zotero Word plugin for citations |

### Plotting tools (you use these; AI does NOT draw)

| Tool | What it plots |
|---|---|
| **Origin** | Line charts, bar charts, stacked bars |
| **ArcMap** | Spatial distribution maps, LISA cluster maps |
| **GeoDa** | Moran's I, spatial weight matrices |
| Python (optional) | Quick plots, forecast figures |

### NOT required

- No GPU
- No server (runs locally)
- No API key (ChatGPT Plus web works; these skills live locally)
- No LaTeX (formulas use single-line LaTeX, Word equation editor recognizes them)

---

## 5. Hard Rules per Skill

### write (strictest)

1. **No fabrication** — formulas / parameters / numbers must have a real source; mark uncertain items "TBD"
2. **Every Language 2 sentence must have a pasted original quote** — no quote = no citation marker
3. **Data口径 follows your Excel** — do not apply textbook defaults
4. **Analytical conclusions do NOT go into .md** — sentences with "therefore / cannot / easily / should not" are feedback for you, not draft text
5. **One section at a time** — must stop after each section; never chain-write multiple sections
6. **Results connect to core argument but do not jump to policy** — bridge via literature phrasing

### read

1. Do not infer conclusions from abstracts
2. If zotero-cli can't connect, stop
3. Every paper's note must be saved to disk
4. Batch 3-5, save before continuing

### engine

1. Only triggers on `ENGINE START`
2. If scaffold exists, ask before overwriting
3. `01_data/raw/` is read-only

### p

1. Does not draw figures
2. Assess data sufficiency before recommending a plot
3. Every recommendation is traceable (which CSV, which folder, which section)

---

## 6. Data Flow Between Files

```
Zotero PDF
   │ zotero-cli reads
   ▼
03_papers/Article/<title>.md  ──→  ArticleMatrices.md (card index)
   │                                    │
   │                                    │ after reading
   ▼                                    ▼
05_outline/outline.md  ←── you write core argument + section tree
   │
   │ p reads outline + cards + inventories data
   ▼
05_outline/figure-list.md  ←── figure registry
   │
   │ write reads outline + terminology + cards + Zotero originals
   ▼
07_draft/<section>/<section>.md       ← draft
07_draft/<section>/<section>data.md   ← traceability (citation ↔ quote ↔ Key)
   │
   │ you assemble in Word:
   ▼
Final paper.docx (Zotero plugin auto-manages numbering and reference list)
```

---

## 7. Your Daily Typical Workflow

1. **Start**: open Zotero desktop
2. **Want to read?** say "use read" → AI gives search strings / reads notes
3. **Want to write?** say "use write, section X.X" → AI drafts + traces → you review
4. **Not sure what to plot?** say "draw figures" → p gives summary table → you confirm
5. **Changed parameters/data?** append a log entry in `04_experiments/logs/`
6. **All sections done**: paste .md into Word, insert citations via Zotero plugin, insert figures, format

---

## 8. Current Gaps (not yet built)

| Missing | Status |
|---|---|
| Abstract writing | No abstract.skill; you write it yourself |
| Core argument extraction | Mostly manual; AI can help find gaps from read notes but won't decide for you |
| Final manuscript QA | No "consistency check across full draft" skill (terminology / figure numbering / missing citations) |
| English polishing | write produces Chinese draft only; you translate for SCI submission |
| Response to reviewers | Defer until after submission |
