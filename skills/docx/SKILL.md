---
name: docx
description: "Use this skill whenever the user wants to create, read, edit, or manipulate Word documents (.docx files) or Word templates (.dotx files). Triggers include: any mention of 'Word doc', 'word document', '.docx', '.dotx', or requests to produce professional documents with formatting like tables of contents, headings, page numbers, or letterheads. Also use when extracting or reorganizing content from .docx or .dotx files, inserting or replacing images in documents, performing find-and-replace in Word files, working with tracked changes or comments, or converting content into a polished Word document. If the user asks for a 'report', 'memo', 'letter', 'template', or similar deliverable as a Word or .docx file, use this skill. Do NOT use for PDFs, spreadsheets, Google Docs, or general coding tasks unrelated to document generation."
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX creation, editing, and analysis

## 课程设计与学术实验报告规范 (Coursework & Report Guidelines)

### 1. 整体与正文字体规范 (General & Body Font Standards)
- **正文主体**：中文字体统一采用**宋体**，字号为**小四号**（12 pt），**1.5 倍行距**，段落**首行缩进 2 个字符**（对应 Word 样式 `正文` / `设计-正文`）。
- **西文字体**：除各级标题外，所有的英文、英文字符与阿拉伯数字一律采用 **Times New Roman** 字体。
- **代码片段规范**：代码字体统一采用 **Times New Roman**，字号为**五号**（10.5 pt），**单倍行距**，缩进 4 个字符（对应 Word 样式 `代码` / `设计-代码`）。禁止整篇无节制粘贴源码，需突出关键类与核心算法，并附规范中文注释。
- **标点符号规范**：并列属性、名词或枚举项必须使用中文顿号（`、`），例如：“学生有姓名、学号、年龄属性”；全文严格排查错别字与标点符号规范性。

### 2. 各级标题与章节层级规范 (Heading Hierarchy & Font Standards)
- **一级标题**：**黑体**，**小三号**（15 pt），对应 Word 样式 `标题 1`（如 `1 需求分析`、`2 概要设计` 等）。
- **二级标题**：**黑体**，**四号**（14 pt），对应 Word 样式 `标题 2` / `设计-标题2`（如 `5.1 预定房间`）。
- **三级条目说明**：课设报告原则上**不出现三级标题**，二级标题下的细分条目统一采用 `(1)`、`(2)`、`(3)` 或 `1.`、`2.`、`3.` 进行分项。
- **目录生成与格式**：大标题“目 录”采用**三号黑体，居中**；目录正文条目采用小四号（12 pt），宋体/Times New Roman，制表位右对齐带前导点（对应 Word 样式 `目录`）。标题修改后必须及时右键更新域。
- **独立章节分页**：“参考文献”与“心得体会”标题均为**小三号黑体**，且**必须插入分页符另起一页**撰写。

### 3. 模板优先与直接修改规范 (Template-First Modification Guideline)
- **强制基于模板编辑**：凡是任务中提供了模板文件（包括但不限于 .docx 实验报告/文档模版、.pptx 演示文稿、.xlsx 表格、.tex 论文模版、代码骨架或脚手架等），必须直接在原模板文件（或复制出的模板文件）上进行编辑修改，严禁新建空白文件从零生成。
- **严格保留模板固有版式与元数据**：必须完整保留并继承原模板中的页面边距、版心尺寸、页眉页脚、样式定义、段落行距及封面排版。文档正文内容多时自然向下分页顺延，不得破坏模板的原生节属性。
- **封面与任务书表格规范**：
  - 封面大标题保持**仿宋**、**初号**（48 pt）、**加粗居中**；学生信息填写项保持小三号（15 pt）。
  - 任务书表格表头保持**宋体**、**五号**（10.5 pt）、**加粗**；填写内容采用**五号**（10.5 pt）宋体/Times New Roman 常规体。
- **占位符原位填充与色彩约定**：
  - 黑色文字为固定框架，保持原样严禁破坏；
  - 绿色文字为撰写内容指引，撰写时根据提示补充并删除绿色提示；
  - 红色文字为格式要求说明，定稿前全部清除；
  - 蓝色文字为示例示范，必须原位替换为自己项目的真实设计与代码。

### 4. 图表排版与截图优化规范 (Figures, Tables & Layout Standards)
- **图题规范（图在下）**：每张插图必须有图号与图名（如 `图5.1 预定房间功能`），放置在**图片正下方且水平居中**；正文叙述中**必须有明确引用**（如“如图5.1所示”）。
- **表题规范（表在上）**：每个表格必须有表号与表名，放置在**表格正上方且水平居中**。
- **图表题注字体**：小四号（12 pt），中文宋体，英文/数字 Times New Roman，加粗（对应 Word 样式 `设计-表格图形标题`）。
- **版面空白控制**：严禁因插入图片导致页面产生超过 3 行的大面积空白；必须根据版心合理缩放、裁剪或调整文字混排。
- **截图优化指南**：
  - 图片宽度除系统结构图、流程图外，最多占正文版心宽度的 **90%**；
  - 截图前裁剪掉所有无关空白或冗余操作区；
  - 连续操作界面避免重复截取相同菜单/背景，紧邻步骤应引用前图并仅截取关键差异部分。

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
