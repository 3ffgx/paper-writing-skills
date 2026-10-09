---
name: write
description: "Section-by-section academic drafting with traceability. For Chinese geosciences/resource-environment papers. Triggers when the user asks to draft, rewrite, or polish any section (introduction, study area & data, methods, results, discussion, conclusion) and requires: standalone paragraphs without numbering/bullets, Language 1 (author transitions) vs Language 2 (cited literature) layering, every formula/parameter/common claim traceable to a Zotero source, GB/T 7714-2015 reference list, and a per-sentence traceability process file (<section>data.md). Core goal: lower plagiarism rate and AI detection while remaining strictly source-grounded. Depends on zotero-cli."
---

# write — Academic Section Drafting with Traceability

Standardizes "draft a publishable section from literature material + your own data." Each section produces **two files**: the draft `<section>.md` and the process file `<section>data.md`.

## Iron Rules (non-negotiable)

1. **No fabrication.** Formulas, parameters, constants, statistics, literature claims must have a real source: user's calculation workbook (e.g. main2.xlsx) or text actually read from Zotero. If unsure, mark "TBD" — **never fill a plausible-looking number from memory**.
2. **Every Language 2 sentence is traceable.** Each cited/paraphrased/parameter-giving sentence in the draft must correspond in the process file to: original sentence (EN/ZH) + translation + Zotero Key.
3. **Data caliber follows user's workbook.** For formula parameters (equivalence factors, yield factors, sample sizes, time ranges, unit conversions), check the user's workbook first; do not apply textbook defaults.
4. **Discard flawed literature claims.** If a source has internal contradictions (mixed symbols, self-contradictory criteria), use only the verifiable parts, explicitly discard the contradictory parts, and record why in the process file. Do not copy just because it is an English top journal.
5. **Ask before writing.** Section topic, output filename, formulas/results to cover, required citations, bolding convention — if unclear, ask.
6. **[CRITICAL] Analytical conclusions / experimental feedback must NOT enter the draft.** The .md draft allows only six content types: study object & scope, data source, formulas, parameter values, unit conversions, processing methods ("what data, how calculated"). Any interpretation of "what the results mean," methodological caveats, or experimental insights — e.g. "therefore one cannot only compare single-city annual changes," "southern Henan low values do not imply higher water-use efficiency" — belongs in **conversation feedback only, never in the .md**. This is especially true for methods sections: write what was done, not why the results are interpreted that way. Any sentence containing "therefore/cannot/easily/should not/this indicates/this suggests/implies" must be self-checked: is this draft content or analysis for the user? If the latter, delete it from the .md.
7. **[Results/Discussion only] Data description must connect to the central thesis and practical significance, but the "data→meaning" bridge must follow literature.** Methods sections present data only. Results/Discussion sections must not merely list trends and ranges ("rise then fall," "high north low south," "fluctuating increase") — that answers nothing about "why was this plotted?" Data phenomena must be guided toward the paper's central thesis and practical application. But this extension has two boundaries: (a) do not exceed academic common sense — do not jump from "northern pressure is high" to "must immediately divert water/restrict extraction" without literature support; (b) the "data→cause→policy" bridge must mimic how similar papers in Zotero write it, borrowing their depth and phrasing. When unsure how far to extend, cite literature's conclusion framing first, then embed your data.
8. **[One section at a time; never generate the full text.]** This is a per-section writing tool, not a full-text generator:
   - **Write only one H2 or H3 section per invocation** (e.g. only "3.1 Water ecological footprint model," not 3.1 + 3.2 together).
   - **Stop after each section**, deliver draft + process file for user review, and wait for "OK, next section" or "revise here" before proceeding.
   - **Never automatically chain multiple sections**, never auto-start the next section, never write intro+methods+results in one go.
   - The "section-by-section architecture" below is a pattern reference for when you arrive at that section — not an instruction to write all sections now.

## Two Languages (both must mimic literature expression habits)

**Core requirement: Language 1 and Language 2 are not "let AI improvise" — they mimic the expression habits of similar papers in the user's Zotero library.** Before writing, read 2–3 Chinese papers' corresponding sections (to learn Language 1 tone) and 2–3 English papers' corresponding sections (to learn Language 2 syntax). Follow their rhythm; do not invent a generic "academic tone."

- **Language 1 (study object/data processing/unit conversion/transitions, not bolded):** States "what data, how converted, how sections connect." **Mimic Chinese geography papers in the library** (e.g. Huang 2008, Hao 2021, Wang 2024) for how they open, state data sources, transition to the next formula. Language 1 does not write analytical conclusions or concept definitions.
- **Language 2 (academic definitions/formulas/parameters/criteria, all bolded):** Definitions, formulas, parameters, criteria aligned with the literature. **Mimic English methods papers in the library** (e.g. Borucke 2013, He 2024) for how they define, list variables, cite criteria — extract sentence patterns, paraphrase in Chinese, do not copy verbatim or invent patterns.

### Language 1 Anti-AI Rules (high priority)

Language 1 is where AI traces are most visible. Self-check line by line:

1. **Ban high-frequency AI/translation clichés.** Never write:
   - Openers: "with the development of…," "in the context of…," "in recent years," "it is well known," "undoubtedly," "has important theoretical and practical significance."
   - Transitions: "first/second/third/finally," "on one hand…on the other hand," "not only…but also," "notably," "it should be pointed out," "in summary," "in conclusion."
   - Empty endings: "provides important reference/basis/support for…," "has important guiding significance," "is of great significance for…"
2. **No content-free transition sentences.** Every Language 1 sentence must carry specific content (study object, data source, conversion method, processing decision, actual logical link to adjacent paragraphs). Delete empty sentences like "this section introduces…"
3. **Reject nested long modifiers.** Chinese geography papers use tight SVO; do not write translation-style nested sentences like "the XX model constructed based on the XX method used to measure the XX index." Break into short sentences.
4. **No four-character idiom parallelism.**
5. **Vary sentence length.** AI tends toward uniform medium-length sentences; deliberately intersperse short (8–15 char) and longer sentences.
6. **Numbers/facts in Language 1 must come from user data or recently read literature.** Do not use content-free generalizations like "numerous studies have shown."
7. **Language 1 does not define concepts** — that is Language 2's job; Language 1 only says "how this paper processes it and why."

### Language 2 Citation Discipline (high priority; eliminate hallucinated citations)

The biggest risk in Language 2 is **reverse-inferring "Smith (2020) argued that…" from a title/common sense without having read the paper.** Rules:

1. **Only cite literature actually read this session.** "Read" means: used zotero-cli `get fulltext`/`read` to read the full text or specified pages, or read its Zotero notes. Never cite based only on title, abstract, or "Rees (1992) must have said that in this field."
2. **Every Language 2 sentence must have a corresponding original sentence in the process file.** If you cannot produce the original (EN or ZH), **do not add a citation marker** — either read the source first, rewrite it as uncited Language 1 (author's own processing decision), or delete it.
3. **Never reverse-infer citations from titles.**
4. **You may synthesize, but synthesis must rest on read original sentences.** When merging multiple sources into one Chinese summary, list all Zotero Keys in the process file, each with its original sentence.
5. **Distinguish three cases:**
   - Single-source paraphrase → one Key, paste that original;
   - Multi-source synthesis → multiple Keys, each with an original;
   - Author's own processing decision (e.g. "this paper calculates yield factors per city per year, not using external fixed values") → this is Language 1, **no citation**, even if some paper did the opposite.
6. **[n] must point to an actually read item.** Numbers can only reference literature opened this round. Unread items, even if "should be cited," are marked "TBD" in the process file and do not appear in the draft.
7. **If the source text does not match what you want to write**, paraphrase closer to the original meaning; if it cannot match, abandon that citation point.

**Bolding convention is paper-wide:** Language 2 (definitions, formulas, parameter values, criteria, paraphrased claims) all bolded; Language 1 (study object, data processing, unit conversion, transitions, author decisions) not bolded. Introduction follows the same rule.

## Section-by-Section Architecture (pattern reference, not "write everything now")

Different sections follow different patterns. Overall argument chain: **field need → existing bottleneck → this paper's approach → key evidence → broader significance → boundaries.**

> **Note:** The intro/methods/results/discussion/conclusion architectures below are references for when you reach that section. Write one section at a time (Iron Rule #8).

### Introduction: Funnel
1. Field importance (1–2 paragraphs, land on a concrete problem)
2. Bottleneck in existing research (fair review, not a literature list)
3. **State the gap explicitly** (map to six gap types below; state which types this paper occupies)
4. This paper directly responds to the gap (one sentence: what was done)

**Six research-gap types:**

| Type | Definition | Example |
|---|---|---|
| Knowledge gap | X known, Y unknown | Model known, but driver decomposition not done in Henan |
| Empirical gap | Theory exists, missing regional/temporal/sample evidence | Other provinces done, Henan 18-city long time series not done |
| Method gap | A method not yet applied to this problem | LMDI/spatial autocorrelation not used on Henan water footprint |
| Object gap | Research concentrated on certain objects | Mostly national/basin scale, missing city scale |
| Context gap | Other countries/regions done, local not done | Footprint research in North China groundwater over-exploitation areas scarce |
| Practical gap | Gap between theory and policy | Accounting done, but how to map to management zones unclear |

### Methods: Three paragraphs per module
1. **Module design:** input → steps → output (formula + variables)
2. **Why this module is needed:** problem-driven (because problem X exists, this method is used)
3. **Why this works:** technical advantage (why this over alternatives)

### Results: Inventory modules first, then write by evidence ladder

**Before writing Results, do an "analysis module inventory"** — do not start writing the moment data arrives:

1. **List which analysis modules you did** (e.g. accounting → temporal change → spatial pattern → spatial autocorrelation → drivers → prediction)
2. **Each module maps to a Results subsection**; define subsection titles and order first
3. **Allocate depth unevenly:**
   - Module most tied to novelty → deep dive (trend + spatial pattern + driver explanation, 2–3 pages/subsection)
   - Minor modules → one paragraph
   - **Align depth with similar SCI papers in Zotero:** see how many subsections Results has, pages per subsection, figures per paper; do not invent length
4. **Cut modules:** if removing a module still leaves novelty intact, compress to one paragraph or delete it.
5. **Determine what to plot per module:**
   - **First see what reference papers plotted:** read `03_论文仓库/ArticleMatrices.md`, scan the "figures/tables used" row — how many figures, what types, how Results is organized; prioritize these.
   - **Then check the general fallback table:** read [references/figure-guide.md](references/figure-guide.md) for modules not covered by references.
   - Finally read `05_主线设计/图表清单.md` (if exists) to confirm figure numbers.

After inventory, write each subsection by evidence ladder:

1. Overall overview (spatial pattern/temporal trend of this indicator)
2. Method credibility validation (if needed, compare with known data)
3. Main results (by indicator/region)
4. Comparison with prior work/baseline (consistency with existing conclusions)
5. Mechanism/explanation (connect data to central meaning, but do not jump to policy)
6. Scale extrapolation (can this finding generalize)

Open each subsection with "To test X, we did Y."

### Discussion: Expand outward from findings
1. Core advance (most important finding)
2. Why the evidence supports this conclusion
3. What cognition changed
4. Relationship to prior studies
5. Limitations
6. Future work

**Do not restate every figure** — select evidence that changes interpretation.

### Conclusion: Extract 3–4 points
Do not repeat results; give conclusions directly.

## Draft Format (`<section>.md`)

1. **Standalone paragraphs, no numbering, no bullets.**
2. **Single-line linear LaTeX** (recognizable by Word equation editor), e.g. `EF_{w,i,t}=\gamma_w W_{i,t}/P_w`; **no `$$...$$` display blocks, no `aligned`/`equation` environments.** Inline symbols use `$...$`. Follow formulas with "where:" explaining each symbol and unit.
3. **Citation markers** at Language 2 sentence end, e.g. `[3]`, `[4,6-7]`; **numbered by order of first appearance.**
4. **Methods sections do not use "this paper"**; introduction may.
5. **Consistent unit symbols** (hm², m³·hm⁻², person⁻¹); conversions (10k people → people, km² → hm², 100M m³ → m³) stated in Language 1.
6. **End `## References`** per GB/T 7714-2015. English author surnames uppercase, initials (HOEKSTRA A Y); Chinese references use Chinese names. Numbers match in-text markers.

## Process File (`<section>data.md`)

Not just a traceability table — follow the full structure of `2_2data.md`. Template: [references/process-file-template.md](references/process-file-template.md), including:

- Writing proposition and section boundaries
- Terminology/symbol table (indicator → LaTeX symbol → workbook field → handling rule)
- Zotero collection reading log (collection Key, which items read, what each supports, which unused)
- English literature narrative logic table (reference section organization → draft adjustment)
- Chinese tone calibration (which Chinese core journals referenced)
- Data/workbook read-only verification (original unit → conversion → draft formula)
- Formula↔evidence mapping (primary source, support level, boundary for each formula/parameter)
- Language 1/Language 2 assignment list
- **Content not included in draft and why** (models/parameters considered but not adopted)
- Output check conclusions

## Two Entry Points

**A. Draft from scratch:** user gives section name; write from blank. Follow standard workflow below.

**B. Polish/reduce-similarity/traceability for existing text:** user pastes existing Chinese. Three steps:
1. Classify each sentence as Language 1 or Language 2;
2. Language 1: rewrite per anti-AI rules (delete clichés, break long sentences, add specifics);
3. Language 2: verify each against a read original — **for sentences without an original, remove the citation marker, convert to Language 1, or mark "TBD literature"**; never keep a `[n]` that cannot match the source.
4. Output in the same draft format; fill process file accordingly.

## Standard Workflow

1. **Clarify:** section topic, output filename, formulas/results to cover, required citations.
2. **Read terminology table:** if `05_主线设计/术语表.md` exists, read it first for consistency. Append new terms after writing.
3. **Define section boundaries:** list "what to write / what not to write" and get user confirmation (especially methods; prevent textbook formula dumping).
4. **Inventory data:** read user's calculation workbook; list formulas actually computed and unit conversion caliber.
5. **Fetch literature (zotero-cli):**
   - **Run `zotero-cli config` first. If it fails/Zotero desktop is off: stop and tell the user "Zotero unavailable, cannot fetch originals"** — do not write Language 2 without the library connected.
   - User's "sample/English" collection Key: `LWF2MDA4`; list items first, then filter by topic; supplement with `zotero-cli --json search "<topic>"`. 
   - `get metadata` + `get fulltext` for 3–5 most relevant items (or `read <key> --start-page --end-page` to save tokens).
   - Log in process file: which items read, which sections, which draft sentences they support, which unused and why.
6. **Deconstruct narrative + learn tone:** from English papers' section structure (definition→formula→parameters→criteria) as structural reference; **also read 2–3 Chinese papers' same section** for transition sentences and data-source phrasing.
7. **Write draft:** only six content types; any interpretive sentences go in conversation feedback, not .md.
8. **Write process file:** fill template line by line; pay special attention to the "Language 2 sentence ↔ original ↔ Key" table.
9. **Self-check** (below).

## Good/Bad Examples

**Language 1 (anti-AI):**
- ✗ AI tone: "With rapid socioeconomic development, water scarcity has become increasingly prominent, posing serious challenges to regional sustainable development and having important practical significance."
- ✓ Revised: "Henan's multi-year average water resources total ~40.3 billion m³; per capita only 383 m³, below the national average."

**Language 2 (anti-hallucination):**
- ✗ Hallucination: "The ecological footprint was first proposed by Rees in 1992 [1]." (Rees1992 not read this session)
- ✓ Correct: `get fulltext` Rees1992 first, paste original "Ecological footprints and appropriated carrying capacity…", then paraphrase, attach original and Key in process table.

**Results/Discussion (data connects to meaning, no jumping):**
- ✗ Trend listing only: "Northern Henan per capita WEF rose from 0.65 hm² in 2000 to 0.82 hm² in 2024; southern Henan remained below 0.30 hm², showing a high-north low-south pattern."
- ✗ Hard policy jump: "Therefore northern Henan must immediately implement inter-basin water transfer and strictly restrict groundwater extraction."
- ✓ Bridge via literature: "Northern Henan's high WEF coincides with concentrated industrial water use and groundwater over-exploitation [x], reflecting stronger water supply constraints on socioeconomic development; this pattern is consistent with findings in North China [y]."

## Pre-Delivery Self-Check

- [ ] **Only the current section written; stopped for user review.**
- [ ] Every Language 2 sentence has a **session-read** original + Zotero Key; no title/common-sense reverse-inferred citations.
- [ ] **No analytical/interpretive sentences in the .md** ("therefore/cannot/easily/should not/this indicates/implies"); these go in conversation feedback.
- [ ] No banned AI clichés.
- [ ] Language 1 mimics Chinese library tone; Language 2 mimics English library syntax.
- [ ] Results/Discussion does not stop at trend listing; data connected to central thesis with literature-supported bridging.
- [ ] Every Language 1 sentence carries specific content; no empty transitions.
- [ ] Formulas/parameters/sample sizes/time ranges/conversions match user workbook.
- [ ] Draft is continuous paragraphs, no numbering/bullets; formulas are single-line LaTeX with "where:".
- [ ] Language 2 bolded, Language 1 not bolded (paper-wide).
- [ ] References per GB/T 7714-2015, by first appearance, numbers match markers.
- [ ] Process file includes "content not included and why" and "output check" sections.
- [ ] Both files written to the user-specified section directory.
