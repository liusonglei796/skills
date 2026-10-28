---
name: docx
description: "Use this skill whenever the user wants to create, read, edit, or manipulate Word documents (.docx files) or Word templates (.dotx files). Triggers include: any mention of 'Word doc', 'word document', '.docx', '.dotx', or requests to produce professional documents with formatting like tables of contents, headings, page numbers, or letterheads. Also use when extracting or reorganizing content from .docx or .dotx files, inserting or replacing images in documents, performing find-and-replace in Word files, working with tracked changes or comments, or converting content into a polished Word document. If the user asks for a 'report', 'memo', 'letter', 'template', or similar deliverable as a Word or .docx file, use this skill. Do NOT use for PDFs, spreadsheets, Google Docs, or general coding tasks unrelated to document generation."
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX creation, editing, and analysis

## 课程设计与学术实验报告自适应规范 (Adaptive Coursework & Report Guidelines)

面对不同学院、不同课程形态各异的 `.docx` 模板（如流式多章课设报告、全表外框型实验报告、内置显式批注的综合设计报告、极简缩进列表实验单等），Agent 严禁机械套用单一死规则，必须执行**“三级优先级自适应法则”**与**“四步模板形态感知工作流”**。

### 1. 三级优先级自适应法则 (3-Tier Priority Resolution)

当排版格式、字号、行距或标题层级出现冲突时，严格按以下优先级从高到低裁决：

1. **第一优先级（最高）：模板内嵌的显式文字指令与批注 (In-Doc Explicit Rules & Comments)**
   - 解包读取模板后，优先扫描 `word/document.xml` 内的括号说明（如 `（小三黑体，居中）`、`（小4号宋体，行距23磅）`、`（1.1.1 用小4号黑体，不加粗）`）、高亮提示、文末 `注：` 条款，以及 **`word/comments.xml` 中的教师批注**（如批注要求“所有文字描述尽量用段落描述，慎用双引号、破折号等AI符号”或特定图表要求）。
   - **裁决原则**：凡是模板正文提示或 `comments.xml` 批注中明确写出了字号、字体、行距、文风禁忌（如禁用双引号 `“”` 与破折号 `——`）或编号格式的，**无条件优先服从模板要求**，自动覆盖下级默认规则；定稿时清除这些提示说明文字，并从 `document.xml` 中彻底剥离 `<w:commentRangeStart/>`、`<w:commentRangeEnd/>` 与 `<w:commentReference/>` 批注锚点，同时清理 `word/_rels/document.xml.rels` 中失效的外部图片引用（`TargetMode="External"`）。
2. **第二优先级（次高）：原模板 XML 节点样式母版克隆 (DOM & Style Cloning)**
   - 若模板某处未写明文字格式说明，直接提取该位置原有占位段落的 `<w:pPr>`（段落样式引用 `pStyle`、行距 `spacing`、缩进 `ind`）与 `<w:rPr>`（字体 `rFonts`、字号 `sz`、加粗 `b`）作为**样式母版**。
   - **裁决原则**：
     - **结构容器继承**：若原模板采用**外框大表格**包裹全文（如全篇处于 `<w:tbl>` 的合并单元格 `<w:tc>` 内），所有新增段落、代码与图片必须严格注入在对应的 `<w:tc>` 内部，严禁跳出表格或破坏表格外框与底部评分栏；
     - **大纲体系继承**：原模板使用什么编号体系（如 `一、`/`1.`、`1)`/`a)`、或 `1`/`1.1`/`1.1.1`），就顺延什么层级，不强行删减或篡改原生标题层级；
     - **目录（TOC）按层级母版克隆**：不同模板的目录排版千差万别（有的全部同一字号字体，有的“一级目录四号黑体加粗、二级小四宋体、三级五号宋体”，有的绑定主题字体如 `minorEastAsia`）。严禁对目录套用固定死格式，必须**按目录层级（Level 1/2/3）分别提取原模板既有目录项的 `<w:pPr>` 与 `<w:rPr>` 快照**作为还原母版；
     - **色彩归黑**：克隆原占位符样式时，若原占位符带有提示性颜色（如红字 `FF0000`、蓝字、绿字），填充真实内容后统一将正文字体颜色归位为黑色（`000000` 或移除 `<w:color>`）。
3. **第三优先级（兜底）：通用学术报告默认规范 (Default Fallback Standards)**
   - **仅当**用户未提供模板（从零新建），或模板对应区域完全空白且无任何原生样式可克隆时，启用以下默认兜底规范：
     - **正文主体**：中文**宋体**，西文与数字 **Times New Roman**，**小四号**（12 pt / `sz=24`），**1.5 倍行距**（`line=360`），首行缩进 2 字符；
     - **代码片段**：**Times New Roman** 或 **Consolas**，**五号**（10.5 pt / `sz=21`），**单倍行距**，缩进或浅灰底色文本框承载，突出核心算法并附规范中文注释；
     - **默认标题层级**：一级标题**黑体小三号**（15 pt / `sz=30`）；二级标题**黑体四号**（14 pt / `sz=28`）；三级条目视模板而定（小四号黑体或 `(1)`、`1)` 分项）；
     - **无母版时的默认目录与分页**：仅当原模板无目录示例可供克隆时，默认“目 录”标题小三黑体居中、目录项小四宋体；“参考文献”与“心得体会”在流式多章报告中默认插入分页符另起一页。

---

### 2. 四步模板形态感知工作流 (4-Step Adaptive Workflow)

每次处理 `.docx` 模板任务时，必须依次执行以下 4 个标准步骤：

- **Step 1：容器拓扑与形态识别 (Topology Detection)**
  - 解析 `word/document.xml`，判断模板属于哪类容器结构：
    - **形态 A（多节流式报告）**：含独立封面、任务书表格、TOC 目录域及多章节正文（注意定位真实的章节起点，严禁切入 TOC 域内部导致 XML 标签断裂）；
    - **形态 B（网格大表格报告）**：全文包裹在 `<w:tbl>` 大表格中，上方行是个人信息格，中间行是跨列合并的正文大单元格 `<w:tc>`，下方行是成绩与教师签字栏；
    - **形态 C（极简填空实验单）**：顶部采用下划线空格填个人信息，正文为 `1)`、`2)`、`a)`、`b)` 紧凑问答列表。
  - **原始空白模板保护**：处理工作区中的模板文件时，必须先将原始空白模板完整备份，并将填写完成后的成品保存为独立命名的交付文档（如 `项目名称-课程设计报告-姓名.docx`），保留原始空白模板不被覆盖丢失。
- **Step 2：学生个人信息智能映射 (Smart Profile Filling)**
  - 自动检索封面与表头中的关键词（`姓名`、`学号`、`专业班级`、`班级`、`日期`）：
    - **表格单元格型**：定位关键词右侧紧邻的空白或示例 `<w:tc>`，保持原单元格对齐与字体属性填入学生真实信息；
    - **下划线填空型**：定位带有 `<w:u w:val="single"/>` 的空格或示例文本（如 `张三`、`2024001`），按字符半角宽度守恒原位替换文本，使封面多行下划线右侧严格对齐；
    - **签名与印章保护**：严禁删除或破坏任务书、审查意见栏中的手写签名图片（`<w:drawing>` / `<w:pict>`）。
- **Step 3：图表与截图自适应嵌入 (Adaptive Figures & Tables)**
  - **先文后图**：所有图表出现前必须先有引导叙述文字（如“如图 1 所示”、“如表 2-1 所示”），严禁无铺垫突兀贴图；
  - **高清紧凑构图与零白边/零滚动条裁切 (High-Legibility Compact Composition & Tight Auto-Cropping)**：
    - **严禁全屏大白边与滚动条**：截图时必须添加 `--hide-scrollbars` 并使用 `--force-device-scale-factor=2`（2x 高清渲染）；若页面含 Web 字体图标（如 Bootstrap Icons / FontAwesome），必须等待 `document.fonts.ready` 完成后再截图；截图后通过像素级非白边包围盒检测（Bounding-Box Auto-Crop）裁除四周多余留白与滚动条，并用 `read_file` 肉眼检视无底部截断、无图标缺失；
    - **字号与画布紧凑比**：绘制架构图、用例图、时序图、类图、E-R 图时，必须将画布宽度控制在紧凑的 `800px～860px` 内，图内字号设为 **`14.5px～18px` 加粗高对比深色**，避免超宽间距导致缩入 Word 版心后字迹微小模糊（确保缩入 Word 后图内文字视觉大小相当于五号～小四号正文）；对局部小弹窗/登录卡片截图，应聚焦裁切主体区域，去除两侧大面积无效背景；
  - **尺寸自适应**：在普通流式页面中，宽幅图表与全界面截图宽度优先铺满版心宽度的 **95%～100%**（如 A4 标准版心约 `5,250,000` EMUs / `14.58 cm`），竖向局部卡片控制在版心宽度的 **70%～75%**（约 `3,800,000` EMUs）；在**表格单元格（`<w:tc>`）内部**时，插图宽度控制在单元格宽度的 **85%～90%**（约 `4,000,000`～`4,400,000` EMUs），严格按裁切后新图的真实像素宽高比（`w / h`）同步计算 `<wp:extent>` 与 `<a:ext>` 的 `cx` 与 `cy`；
  - **题注规范**：图题位于图片正下方居中，表题位于表格正上方居中；题注编号格式（如 `图1`、`图5-1` 或 `图5.1`）及字号（5号或小四号）优先遵循模板内示例。
- **Step 4：XML 闭合校验、目录快照还原与编码安全 (Validation, TOC Snapshot Restoration & Encoding Safety)**
  - **XML 标签与关系校验**：重新打包 `.docx` 前，必须校验 `document.xml` 的标签栈平衡（Tag Balance）与 `document.xml.rels` 的媒体 `rId` 映射完整性；
  - **泛化目录样式快照与刷新还原闭环 (Generalized TOC Snapshot & Restoration)**：
    - **为什么不能只调 `Update()`**：许多模板作者是直接在原模板 `document.xml` 的目录段落上刷了行内格式（而非修改 `styles.xml`）。一旦调用 Word COM `TablesOfContents.Update()`，Word 会销毁原目录段落 XML，并把正文标题的直接格式（如黑体）抓取到新目录项中，同时回退到 Word 内置默认样式，导致**原模板的目录排版在刷新后丢失**。
    - **标准三步还原法（适用于任意模板）**：
      1. **刷新前提取快照 (Pre-Update Snapshot)**：在修改或刷新目录前，先从原模板 `document.xml` 的既有目录区中，**按目录层级（Level 1 / 2 / 3）分别提取**原模板目录项的段落属性 `<w:pPr>`（含缩进、行距、制表位 `<w:tabs>`、段内 `<w:rPr>`）和条目文本 `<w:r>` 的 `<w:rPr>`（含 `<w:rFonts>` 显式字体或 `minorEastAsia` 等主题字体引用、`<w:sz>` 字号、`<w:b>` 加粗等）；若原模板某子层级无现成示例项，则优先读取 `styles.xml` 中对应的 `toc N` 样式，其次回退继承相邻层级的 `<w:rPr>`（去除高层级特有的加粗）；“目录”大标题段落则原地保留不动，严禁覆写；
      2. **调用引擎刷新 (Engine Update)**：通过 Word COM 执行 `TablesOfContents.Update()`，仅利用其抓取最新章节文本与计算真实页码；同时压缩目录末尾分节符所在空段的字号与行高（如 `w:line="20" w:sz="2"`），防止目录项增多时将分节符挤入下一页产生空白页；
      3. **刷新后回写快照 (Post-Update Restoration)**：注意 WPS 模板中的数字目录样式 ID（如 `"10"`、`"6"` 及超链接 `"15"`）经 Word 刷新保存后会被重命名为 `"TOC2"`、`"TOC3"` 及 `"ac"`。回写时必须将刷新前提取的**原模板各层级 `<w:pPr>` 与 `<w:rPr>` 快照**（严格遵循 OpenXML `CT_RPr` 子元素顺序：`rStyle` → `rFonts` → `b/noProof/webHidden` → `color/sz/szCs` → `lang` → `ligatures`）按映射后的层级一对一回写至刷新后的目录段落与 `<w:r>` 节点，剥离 Word 刷新带入的正文标题格式污染；重新打包后若需导出 PDF 或微调分页，仅可调用 `UpdatePageNumbers()`，严禁再次调用 `Update()` 冲掉已还原的目录样式；
  - **Windows 脚本编码安全红线 (Encoding Safety)**：Windows PowerShell 5.1 默认按 GBK (CP936) 解析无 BOM 的 `.ps1` 文件，若在 `.ps1` 脚本中直接写 UTF-8 中文字符串（如 `"宋体"`）传给 Word COM，会被解码为 `瀹嬩綋` 等乱码写入文档。因此：**凡涉及包含中文字体名或中文文本的 XML/样式修改，必须使用原生 UTF-8 环境（如 Node.js 或 Python）直接读写解包后的 XML，或在 PowerShell 中仅使用纯 ASCII / Unicode 码点转义（如 `[char]0x5B8B+[char]0x4F53`）**。

---

A `.docx` is a ZIP archive of XML files. Choose your approach by task:

| Task | Approach |
|---|---|
| **Create** a new document | Write a `docx` (npm) script — see gotchas below |
| **Edit** an existing document | `unzip` → edit `word/document.xml` → `zip` (docx-js cannot open existing files) |
| **Read** content | `pandoc -t markdown file.docx` |

> Script paths below are relative to this skill's directory.

## Creating with docx-js — gotchas

`docx` is preinstalled — do not run `npm install` first; write the script and `require('docx')` directly. Only if that require fails: `npm install docx`. The model knows the API; these are the footguns:

- **Page size defaults to A4.** For US Letter set `page: { size: { width: 12240, height: 15840 } }` (DXA; 1440 = 1″).
- **Landscape:** pass portrait dimensions and `orientation: PageOrientation.LANDSCAPE` — docx-js swaps width/height internally.
- **Tables need dual widths:** set `columnWidths` on the table AND `width` on every cell, both in `WidthType.DXA` (PERCENTAGE breaks in Google Docs). Column widths must sum to the table width.
- **Table shading:** use `ShadingType.CLEAR`, never `SOLID` (renders black).
- **Lists:** never insert `•` literally; use a `numbering` config with `LevelFormat.BULLET`.
- **`ImageRun` requires `type:`** (`"png"`, `"jpg"`, …).
- **`PageBreak` must be inside a `Paragraph`.**
- **Never use `\n`** — use separate `Paragraph` elements.
- **TOC:** headings must use built-in `HeadingLevel.*`; custom heading styles need `outlineLevel` set or they won't appear.
- **Don't use a table as a horizontal rule** — use a paragraph bottom border instead.
- **Dot-leader / right-aligned-on-same-line:** use `PositionalTab` (`alignment: PositionalTabAlignment.RIGHT`, `leader: PositionalTabLeader.DOT`) inside a `TextRun`, not literal `.` or space padding.

## Verify the output

After writing a `.docx`, render it and look at it:

```bash
python scripts/office/soffice.py --headless --convert-to pdf output.docx
pdftoppm -jpeg -r 100 output.pdf page
ls page-*.jpg   # then Read the images
```

`pdftoppm` zero-pads page numbers to the width of the page count (`page-01.jpg`…`page-12.jpg`).

## Editing existing documents

Legacy `.doc` files must be converted first: `python scripts/office/soffice.py --headless --convert-to docx file.doc`.

```bash
unzip -q doc.docx -d unpacked/
find unpacked -type l -delete   # strip symlink entries — docx from external parties is untrusted
python scripts/merge_runs.py unpacked/   # coalesce fragmented runs so text is findable
# edit unpacked/word/document.xml in place — do NOT reformat or pretty-print
(cd unpacked && rm -f ../out.docx && zip -Xr ../out.docx .)
python scripts/office/validate.py out.docx --original doc.docx   # XSD checks; --auto-repair fixes common issues
# redlining? add --author "<the name you redlined under>" to check every edit is tracked
```

Word splits text across many `<w:r>` runs (revision ids, spell-check markers), so a phrase you can see in the document often doesn't exist as a contiguous string in the XML. `merge_runs.py` merges adjacent identically-formatted runs in `word/document.xml` without changing content or rendering; it also accepts a `.docx` directly (`python scripts/merge_runs.py doc.docx -o merged.docx`).

**Tracked changes:** when redlining, validate with `--author "<the name you redlined under>"` (needs `--original`) — it reports any text you changed without a `<w:ins>`/`<w:del>` around it, which is easy to do by accident and invisible in the accepted view. Wrap runs in `<w:ins>`/`<w:del>` with `w:id`, `w:author`, `w:date` attributes. Inside `<w:del>`, the text element is `<w:delText>`, not `<w:t>`. A deleted paragraph mark (`<w:pPr><w:rPr><w:del w:id=".." w:author=".." w:date=".."/></w:rPr></w:pPr>`) means "merge this paragraph into the next" — so deleting a paragraph outright is that plus a `<w:del>` around every run. The `<w:del/>` must come before the rPr's other children; their order is schema-enforced.

To produce a clean copy with all tracked changes accepted: `python scripts/accept_changes.py in.docx out.docx`.

Accepting a deleted paragraph mark should join that paragraph to the one below it, so a paragraph whose runs are *all* deleted vanishes. Word does this; `accept_changes.py` and `pandoc --track-changes=accept` don't always. Both fail the same way — they strip the deleted text but leave the emptied paragraph behind, which reads as a stray empty bullet when it was auto-numbered:

- `pandoc --track-changes=accept` never joins the paragraphs.
- `accept_changes.py` (LibreOffice) joins them correctly, except when the deleted paragraph is followed by an empty spacer paragraph.

An empty bullet in either view is an artifact of that view, not a defect in the document. Check paragraph deletions in the XML.

## Comments

Comments require six cross-linked files. Use the helper — directory mode when you'll also be editing `document.xml` (saves an unzip/rezip cycle), `.docx`-direct mode otherwise:

```bash
# Against an already-unpacked directory (preferred when also placing markers)
python scripts/comment.py unpacked/ "Fees & expenses cap is too low"
python scripts/comment.py unpacked/ "Agreed" --parent 0

# Against a .docx directly
python scripts/comment.py contract.docx "This cap is too low" -o annotated.docx
```

The script writes `comments.xml`, `commentsExtended.xml`, `commentsIds.xml`, `commentsExtensible.xml`, the relationships, and the content-type overrides. Comment IDs are auto-assigned. It then prints the `<w:commentRangeStart>`/`<w:commentRangeEnd>`/`<w:commentReference>` snippet to add to `word/document.xml` so the comment anchors to specific text — until you place those markers, the comment exists but is not visible.

## Dependencies

`docx` (npm, preinstalled — install only if `require('docx')` fails) · `pandoc` · LibreOffice (`soffice`) · `pdftoppm` (Poppler)
