---
name: engine
description: "【手动唤起型，仅在用户输入 ENGINE START 时触发】在当前项目根目录一键生成论文项目的标准文件夹骨架（英文目录名，避免中文路径编码问题）：data/literature/papers/experiments/outline/temp/draft/figures 八大目录及各自子目录，并在根目录落一份 README.md 说明各目录用途与写入规则。不自动触发，不在用户没说 ENGINE START 时创建或改动任何文件夹。"
---

# engine — 项目骨架一键生成

用户在某个项目根目录下输入 `ENGINE START`，本 skill 自动在该目录生成标准文件夹结构。**所有目录和模板文件均使用英文命名**，避免中文路径在脚本/工具中出现编码问题。

## 触发条件

- **唯一触发**：用户输入 `ENGINE START`（大小写不限、可带前后空格）。
- 其他情况（包括"帮我建个项目""初始化文件夹"）**不自动执行**，先问用户是不是要 ENGINE START。

## 执行流程

1. **确认项目根目录**：问用户"在哪个目录下建骨架"。如果用户当前对话里已经给出路径，直接用；没给就问。
2. **检查是否已存在**：如果目标目录里已经有 `01_data` 等文件夹，先告诉用户"骨架已存在，要不要重建/跳过"，不要无脑覆盖。
3. **生成文件夹**（结构见下）。
4. **在根目录写 `README.md`**：把各目录用途和写入规则落盘，后续任何 AI 进项目先读这份。
5. **在 `05_outline/` 写三份模板**：
   - `outline.md`（空的章节树，用户自己填）
   - `terminology.md`（空表，write skill 写作过程中逐步积累）
   - `figure-list.md`（空表，按图号登记图/表，附"分析模块→图类型"对照表）
6. **在 `04_experiments/logs/` 放一份日志模板**，用户跑分析时追加记录。
7. **回报**：告诉用户建好了哪些目录、模板在哪。

## 标准骨架

```
<project-root>/
├── 01_data/
│   └── raw/                    # 年鉴、遥感等原始数据（只读，AI 禁止覆盖）
├── 02_literature/               # 检索关键词、检索式、导出的 RIS/BibTeX
├── 03_papers/                  # read skill 工作区
│   ├── Article/               #   每篇论文笔记：<title>.md
│   ├── ArticleMatrices.md     #   目录卡片
│   └── _reading-progress.md    #   已读/待读/放弃
├── 04_experiments/
│   ├── code/                  # 主线测试脚本（python/r 等）
│   ├── results/               # 脚本跑出的中间结果
│   └── logs/                  # 实验日志
├── 05_outline/                 # 论文中心、实验方案、章节大纲
├── 06_temp/                   # AI 过程文件：试跑脚本、中间数据、废稿
├── 07_draft/                   # write skill 最终稿工作区
│   └── (按章节建子文件夹，如 1-intro/、2.2-methods/)
└── 08_figures/                 # p skill 工作区：所有图件工程、数据、最终图
    ├── origin/                 #   Origin工程（.opj）+ 数据 + 最终图
    ├── arcmap/                 #   ArcMap工程（.mxd）+ shp + 最终图
    ├── geoda/                  #   GeoDa项目文件
    └── python/                 #   Python画图脚本 + 输出图
```

## 写入规则（写进 README.md）

- **01_data/raw/ 只读**：任何 AI 不许修改、删除、覆盖原始文件。
- **02_literature/**：放检索策略，不放论文全文。
- **03_papers/**：read skill 专用，其他 skill 不往里写。
- **04_experiments/results/**：脚本输出统一落这，不要散在 code/ 旁边。
- **05_outline/**：论文中心论点、技术路线图、章节大纲的草稿。
- **06_temp/**：AI 随便写，但**项目定稿前要清理**；不确定放哪的东西先扔这。
- **07_draft/**：write skill 专用，每个章节一个子文件夹，里面 `<section>.md` 是成稿、`<section>data.md` 是溯源过程表。
- **08_figures/**：p skill 专用，按平台（origin/arcmap/geoda/python）分子文件夹，每个图一个子文件夹（`FigX_name/`），里面放工程文件+绘图数据+最终图。

## 与其他 skill 的衔接

- `read` skill 的输出根目录 = `03_papers/`。
- `write` skill 的输出根目录 = `07_draft/`。
- `p` skill 的工作目录 = `08_figures/`。
- 画图数据从 `01_data/raw/` 或 `04_experiments/results/` **复制**到对应图件文件夹，原始数据不动。

## 交付前自检

- [ ] 问清了项目根目录，没在错的地方建。
- [ ] 已存在骨架时先提示用户，没直接覆盖。
- [ ] 8 个一级目录（01_data ~ 08_figures）和各自子目录都建好了。
- [ ] 根目录 `README.md` 已写，内容包含各目录用途和写入规则。
- [ ] `05_outline/outline.md` 模板已生成。
- [ ] `05_outline/terminology.md` 模板已生成。
- [ ] `05_outline/figure-list.md` 模板已生成。
- [ ] `04_experiments/logs/` 目录已建，放了日志模板。
- [ ] `08_figures/` 下建了 origin/ arcmap/ geoda/ python/ 四个平台子目录。
- [ ] 没有在 `01_data/raw/` 里放任何示例文件（保持干净）。
