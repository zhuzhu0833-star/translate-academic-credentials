# translate-academic-credentials

Cursor Agent Skill：将中文成绩单、学位证、毕业证等学历文件译为英文，并严格按原件版式导出 Word 与 PDF。

## 安装

```bash
mkdir -p ~/.cursor/skills
git clone <仓库地址> ~/.cursor/skills/translate-academic-credentials
```

或手动将本仓库解压/复制到 `~/.cursor/skills/translate-academic-credentials/`。

## 依赖

```bash
python3 -m pip install python-docx fpdf2
```

可选：安装 [LibreOffice](https://www.libreoffice.org/)，PDF 将从 Word 转换，排版更接近原件。

## 使用

在 Cursor 新对话中说，例如：

- 「用 translate-academic-credentials skill 翻译这份成绩单」
- 「严格按原件格式，输出 Word 和 PDF，不要中文原文」

Agent 会读取 `SKILL.md`，生成 `translation.json`，并运行：

```bash
python3 ~/.cursor/skills/translate-academic-credentials/scripts/build_translation_docs.py translation.json
```

## 文件结构

```
translate-academic-credentials/
├── SKILL.md              # 主流程
├── reference.md          # 术语表
├── templates.md          # JSON 模板
├── layout-reference.md   # 版式还原指南
└── scripts/
    └── build_translation_docs.py
```

## 同步到其他电脑

```bash
cd ~/.cursor/skills/translate-academic-credentials
git pull
```

## 更新后推送

```bash
git add .
git commit -m "描述你的修改"
git push
```
