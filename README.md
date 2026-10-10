# Contract Desensitizer 合同脱敏工具

A local-first desensitization tool for Chinese Word contracts. It replaces sensitive entities — companies, persons, ID numbers, phone numbers, credit codes, dates, amounts, addresses — with placeholders like `[[COMPANY_001]]`, saves a local mapping file, and restores the original text after AI review. **Runs 100% locally: no network calls, your documents never leave your machine.**

> ⚠️ Security note: the mapping file (`--映射.json`) contains the original sensitive data. Keep it local — never push it to a public repo. This repo's `.gitignore` already excludes contracts and mapping files under the input/output directories.

> ✅ Used in real client engagements.

## Workflow

1. **Desensitize** — drop the original contract `.docx` into `输入文件/`, run the script → get `原文件--脱敏.docx` (desensitized) + `原文件--映射.json` (mapping) in `输出文件/`
2. **AI review** — send the desensitized version for AI review → `原文件--AI审查.docx`
3. **Restore** — put the reviewed file into `输入文件/` (keep the mapping file), run restore → `原文件--恢复.docx` with originals restored

## Supported entity types

| Category | Description | Placeholder example |
|---|---|---|
| COMPANY / PERSON | Counterparties (甲方/乙方…), legal representatives, contacts, signatories | `[[COMPANY_001]]` |
| CREDIT_CODE | Unified Social Credit Code (18-digit) | `[[CREDIT_CODE_001]]` |
| ID_CARD | National ID number | `[[ID_CARD_001]]` |
| PHONE | Mobile numbers (incl. +86) | `[[PHONE_001]]` |
| EMAIL | Email addresses | `[[EMAIL_001]]` |
| DATE | Dates | `[[DATE_001]]` |
| AMOUNT | Monetary amounts | `[[AMOUNT_001]]` |
| PERCENT | Percentages | `[[PERCENT_001]]` |
| ADDRESS | Mailing addresses | `[[ADDRESS_001]]` |

## Usage

```bash
pip install -r requirements.txt

# Interactive menu
python 合同脱敏.py

# Or run directly
python 合同脱敏.py 脱敏     # desensitize
python 合同脱敏.py 恢复     # restore
```

## Highlights

- Covers body text, tables (including nested tables), headers and footers
- Preserves original character formatting when replacing text
- Text normalization: tolerant of CN/EN bracket, spacing and line-break inconsistencies
- One consistent placeholder per entity across the whole document
- Refuses to overwrite existing output files

## Limitations

- Rule- and regex-based: **always manually verify the output** before sharing — false positives/negatives are possible
- `.docx` text only; scanned images and text inside embedded images are not processed
- Optimized for Chinese-language contracts

---

## 中文说明

本地运行的中文合同脱敏工具：把 Word 合同里的敏感信息替换成占位符（如 `[[COMPANY_001]]`），并生成映射表；AI 审查完成后，再用映射表一键恢复原文。**全程本地运行**——脚本没有任何网络请求，合同内容不会经过第三方服务。

> ⚠️ 安全提醒：映射表（`--映射.json`）包含原始敏感信息，请只保存在本地，切勿上传到公开仓库。本仓库的 `.gitignore` 已默认排除输入 / 输出目录下的合同与映射文件。

> ✅ 已在实际客户项目中使用。

### 工作流

1. **脱敏**：把原始合同 `.docx` 放进 `输入文件/`，运行脚本 → 在 `输出文件/` 生成 `原文件--脱敏.docx` 和 `原文件--映射.json`
2. **AI 审查**：把脱敏版交给 AI 审查，得到 `原文件--AI审查.docx`
3. **恢复**：把 AI 审查版放进 `输入文件/`（保留好映射表），运行恢复 → 生成 `原文件--恢复.docx`

### 能识别的敏感信息

| 类别 | 说明 | 占位符示例 |
|---|---|---|
| COMPANY / PERSON | 甲方乙方、法定代表人、联系人、签字 / 盖章主体 | `[[COMPANY_001]]` |
| CREDIT_CODE | 统一社会信用代码（18 位） | `[[CREDIT_CODE_001]]` |
| ID_CARD | 身份证号 | `[[ID_CARD_001]]` |
| PHONE | 手机号（含 +86 前缀） | `[[PHONE_001]]` |
| EMAIL | 邮箱 | `[[EMAIL_001]]` |
| DATE | 日期 | `[[DATE_001]]` |
| AMOUNT | 金额 | `[[AMOUNT_001]]` |
| PERCENT | 百分比 | `[[PERCENT_001]]` |
| ADDRESS | 邮寄地址 | `[[ADDRESS_001]]` |

### 使用方法

```bash
pip install -r requirements.txt

# 交互式菜单
python 合同脱敏.py

# 或直接指定操作
python 合同脱敏.py 脱敏
python 合同脱敏.py 恢复
```

### 特性

- 覆盖正文、表格（含嵌套表格）、页眉页脚
- 替换时保留原有文字格式（字体、加粗等）
- 文本标准化匹配：自动处理中英文括号、空格、换行等格式差异
- 同一主体全文统一占位符
- 输出文件已存在时自动停止，绝不覆盖已有结果

### 局限

- 基于正则与规则匹配，**脱敏后请务必人工复核**
- 仅支持 `.docx` 的文字内容；扫描件图片、嵌入图片里的文字无法处理
- 针对中文合同优化
