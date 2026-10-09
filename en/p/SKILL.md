---
name: p
description: "Manual-invoke only. Figure management skill. Load when user says 'plot a figure,' 'what should I plot,' 'can my data support X plot,' 'how to present this figure,' 'help me figure out what to draw.' Functions: (1) Read ArticleMatrices.md to tell user how similar papers present figures; (2) Assess whether available data supports a given figure; (3) Create figure folders under 08_图/ by platform (origin/arcmap/geoda/python), managing project files, data, final images; (4) Cross-reference write skill's figure-guide.md for general recommendations. Does NOT draw figures — only planning and data assessment."
---

# p — Figure Planning & Management

Solves "what figures should this paper have, do I have enough data, where do figure files go."

## Iron Rules

1. **Do not draw.** p only plans (what to plot, data sufficiency, file location); actual plotting is done by the user in Origin/ArcMap/GeoDa.
2. **Assess data before recommending a figure.** Do not open with "you should plot a Moran scatterplot" — first check whether the user has a spatial weights matrix and computed Moran's I; state what's missing first.
3. **Recommendations must be traceable.** Each figure recommendation answers: which file is the data source, which 08_图/ subfolder it goes to, which paper section it maps to.
4. **Ask before doing.** If user hasn't specified what to plot, which tool, or where data is, ask.

## Four Entry Points

### A. Full-Figure Plan (default action when p starts)

When the user runs p to prepare plotting, **do a full-figure plan first** — don't wait for one-by-one questions:

1. **Read the thesis outline:** `05_主线设计/论文大纲.md` — how many Results subsections, what each covers.
2. **Read reference papers:** `03_论文仓库/ArticleMatrices.md` — how many figures Results sections of similar papers use, what types.
3. **Inventory available data:** what's in `01_数据处理/原始数据/` and `04_实验测试/结果输出/`.
4. **Recommend per subsection:** what to plot for each Results subsection, which reference paper's approach to borrow, whether data is sufficient.
5. **Output a summary table:**

| Section | Suggested figure | Reference paper | Data needed | Have it? | What's missing |
|---|---|---|---|---|---|
| 4.1 Temporal change | Per-capita WEF yearly line | He2024 Fig2 | Yearly per-capita EF/EC | ✅ Yes | None |
| 4.2 Spatial pattern | Multi-year average choropleth | He2024 Fig5 | Annual mean + boundary shp | ✅ Yes | None |
| 4.3 Spatial autocorrelation | Moran scatter + LISA | Rey2006 | Spatial weights matrix | ❌ No | Need queen weights in GeoDa |
| 4.4 Drivers | LMDI stacked bar | Su2020 | Decomposition effect values | ❌ No | Need to run LMDI first |
| 4.5 Prediction | Historical vs forecast line | Liu2018 | GM(1,1) forecast values | ❌ No | Need to run forecast model |

6. **Wait for per-subsection confirmation:** user says "4.1 is OK" or "skip 4.4" before creating folders.

### B. Single-Figure Data Sufficiency (when user asks about one figure)

1. Inventory available data (01_数据处理/, 04_实验测试/结果输出/).
2. Cross-reference write skill's `references/figure-guide.md` for what data this figure needs.
3. Assess: sufficient → tell user they can plot; insufficient → state what's missing.
4. Ask whether to proceed.

### C. Create Figure Folder (08_图/ management)

After user confirms a figure:

1. Create `08_图/<platform>/FigX_name/`.
2. Place three types of files: project files (.opj/.mxd/scripts), plotting data (copied over), final image.
3. Register a row in `05_主线设计/图表清单.md`.

## 08_图/ Directory Structure

```
08_图/
├── origin/
│   ├── Fig2_perCapitaWEF/
│   │   ├── Fig2.opj              # Origin project
│   │   ├── Fig2_data.csv        # Plotting data
│   │   └── Fig2.tif              # Final figure
│   └── Fig6_LMDI/
├── arcmap/
│   ├── Fig3_SpatialPattern/
│   │   ├── Fig3.mxd              # ArcMap project
│   │   ├── Fig3_data.shp         # Spatial data
│   │   └── Fig3.tif
│   └── Fig5_LISA/
├── geoda/
│   └── Fig4_MoranScatter/
└── python/
    └── Fig7_Forecast/
```

**Rules:**
- One subfolder per figure, named `FigX_name`.
- Project file, data, final image live in the same folder; do not scatter.
- Data files are **copied** from 01_数据处理/ or 04_实验测试/结果输出/ (not moved); raw data untouched.

## Standard Workflow

1. **p starts** → default Entry A (full-figure plan).
2. **Read thesis outline:** how many Results subsections.
3. **Read ArticleMatrices.md:** how reference papers present figures.
4. **Inventory data:** 01_数据处理/, 04_实验测试/结果输出/.
5. **Output summary table:** what to plot per section, which reference, data sufficiency, gaps.
6. **User confirms per section:** which to adopt, which to skip.
7. After confirmation → create platform folder under 08_图/ → register in figure list.

## Pre-Delivery Self-Check

- [ ] Read thesis outline first; not recommending figures from thin air.
- [ ] Borrowed reference papers' figure organization.
- [ ] Every section recommendation assessed against available data; gaps stated explicitly.
- [ ] Output summary table for per-section confirmation; did not auto-start plotting.
- [ ] Created 08_图/ folders only after user confirmation.
- [ ] Registered every figure in the figure list.
