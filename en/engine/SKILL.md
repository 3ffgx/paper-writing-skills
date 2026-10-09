---
name: engine
description: "Manual-invoke only; triggers ONLY when user types ENGINE START. Generates a standard paper-project folder skeleton in the current directory: data processing / literature search / paper repository / experiment testing / main-line design / temp files / writing output / figures — eight top-level directories with subdirectories, plus a root _README.md explaining directory purposes and write rules. Does not auto-create or modify folders otherwise."
---

# engine — One-Command Project Skeleton

User types `ENGINE START` in a project root directory; this skill auto-generates the standard folder structure.

## Trigger Condition

- **Sole trigger:** user types `ENGINE START` (case-insensitive, leading/trailing spaces allowed).
- Other situations ("help me set up a project," "initialize folders") do **not** auto-execute — ask first if the user means ENGINE START.

## Execution Flow

1. **Confirm project root:** ask "which directory?" If already given in conversation (e.g. "set up in D:\papers\Henan"), use it directly; otherwise ask.
2. **Check existence:** if `01_数据处理` etc. already exist, tell the user "skeleton exists; rebuild/skip?" — do not blindly overwrite.
3. **Generate folders** (structure below).
4. **Write root `_README.md`:** directory purposes and write rules on disk; any AI entering the project reads this first.
5. **Write three templates in `05_主线设计/`:**
   - `论文大纲.md` (empty section tree, user fills)
   - `术语表.md` (empty table, write skill accumulates)
   - `图表清单.md` (empty table, figure/table registry with module→type reference)
6. **Create `04_实验测试/日志/`** with a log template for the user's experiment entries.
7. **Report:** which directories were created, where templates are.

## Standard Skeleton

```
<project root>/
├── 01_数据处理/
│   └── 原始数据/            # Yearbooks, remote-sensing raw data (read-only; AI must not overwrite)
├── 02_文献检索/             # Search keywords, Boolean strings, exported RIS/BibTeX
├── 03_论文仓库/             # read skill workspace
│   ├── Article/            #   Per-paper notes: <title>.md
│   ├── ArticleMatrices.md  #   Directory cards
│   └── _阅读进度.md
├── 04_实验测试/
│   ├── 代码/               # Main-line test scripts (python/r)
│   ├── 结果输出/           # Script intermediate outputs
│   └── 日志/               # Experiment logs
├── 05_主线设计/             # Paper thesis, experimental plan, section outline
├── 06_临时文件/             # AI process files: test scripts, intermediate data, drafts
├── 07_写入参考/             # write skill final draft workspace
│   └── (section subfolders, e.g. 1引言/, 2.2核算方法/)
└── 08_图/                  # p skill workspace: all figure projects, data, final images
    ├── origin/             #   Origin projects (.opj) + data + final figures
    ├── arcmap/             #   ArcMap projects (.mxd) + shp + final figures
    ├── geoda/              #   GeoDa project files
    └── python/             #   Python plotting scripts + output figures
```

## Write Rules (in _README.md)

- **01_数据处理/原始数据/ is read-only:** no AI may modify/delete/overwrite raw files.
- **02_文献检索/:** search strategies only, no full papers.
- **03_论文仓库/:** read skill only; other skills do not write here.
- **04_实验测试/结果输出/:** script outputs go here, not scattered beside code.
- **05_主线设计/:** draft of central thesis, technical roadmap, section outline.
- **06_临时文件/:** AI free to write, but **clean before final submission**; unsure where something goes → put here.
- **07_写入参考/:** write skill only; one subfolder per section; `<section>.md` is draft, `<section>data.md` is traceability process file.
- **08_图/:** p skill only; per-platform subfolders (origin/arcmap/geoda/python); one subfolder per figure (`FigX_name/`) containing project files + plotting data + final image.

## Integration with Other Skills

- `read` output root = `03_论文仓库/`.
- `write` output root = `07_写入参考/`.
- `p` working directory = `08_图/`.
- Plotting data is **copied** (not moved) from `01_数据处理/原始数据/` or `04_实验测试/结果输出/` to the figure folder; raw data untouched.

## Pre-Delivery Self-Check

- [ ] Project root confirmed; not created in the wrong place.
- [ ] Existing skeleton prompted user, not overwritten.
- [ ] 8 top-level directories and subdirectories created.
- [ ] Root `_README.md` written with purposes and rules.
- [ ] `05_主线设计/论文大纲.md` template generated.
- [ ] `05_主线设计/术语表.md` template generated.
- [ ] `05_主线设计/图表清单.md` template generated.
- [ ] `04_实验测试/日志/` created with log template.
- [ ] `08_图/` has origin/ arcmap/ geoda/ python/ subdirectories.
- [ ] No sample files placed in `01_数据处理/原始数据/` (kept clean).
