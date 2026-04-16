---
name: dvp-composer
description: >
  DVP（数据验证计划）生成器。This skill should be used when the user asks to
  "编写DVP", "创建数据验证计划", "生成DVP", "compose DVP", "create DVP", "help with DVP",
  "write a DVP", "edit checks", "编辑检查", "数据验证", "数据质量", "data quality",
  or mentions clinical data verification plans, edit check definitions, or data quality
  planning for a clinical study. 读取 Protocol/CRF/DMP 文档，经逻辑审查后输出格式化 Excel。
argument-hint: "[Protocol/CRF/DMP 文件路径，或研究名称]"
context: fork
allowed-tools:
  - "Bash(python3 *)"
  - "Bash(pip3 *)"
  - "Read"
  - "Write"
  - "Grep"
  - "Glob"
user-invocable: true
---

# DVP Composer

生成 DVP（Data Verification Plan，数据验证计划），输出格式化 Excel 文件。

## 适用范围

- 为临床研究项目创建 DVP 文档
- 支持读取 Protocol、CRF、DMP 等参考文档
- DVP 章节结构由用户决定，不强制固定模板
- 输出为 .xlsx 格式

## 不做什么

- 不连接 EDC/CDMS 系统
- 不验证内容是否符合监管要求
- 不生成 PDF/Word 输出（仅 .xlsx）

## 信息收集

### 参考文档

用户可能提供 Protocol、CRF、DMP 等参考文档（可提供任意组合或跳过）。

1. 用 Read 读取文档
2. 提取关键信息：
   - Protocol → 研究名称、编号、设计、终点、访视计划
   - CRF → 表单名称、字段列表、字段类型
   - DMP → 数据管理流程、质量标准
3. 将提取结果存入 `/tmp/dvp_extracted.json`

提取后向用户展示摘要（标注来源和缺失项），让用户确认或补充。

### 章节结构

确定 DVP 的章节结构。来源可以是：
- 用户提供现有 DVP 模板 → 读取并提取结构
- 从常见章节选择 → 参考 [section-catalog](references/section-catalog.md)
- 用户直接描述

展示章节列表供用户确认。用户可调整顺序、增删、改名。

### 章节内容

为每个章节填充内容：
- 优先使用文档提取的信息预填充
- 缺失或不明确的部分，向用户询问澄清
- 用户可以跳过任何章节（留占位符）

各类型章节的填充方式见 [flow-guide](references/flow-guide.md)。

## 逻辑审查

按 [review-rules](references/review-rules.md) 执行六项检查：

- R1 字段引用完整性
- R2 数据一致性
- R3 命名规范
- R4 业务逻辑合理性
- R5 规则冲突检测
- R6 交叉引用一致性

输出审查报告（✅/⚠️/❌）：
- ❌ 错误项必须修正
- ⚠️ 警告项由用户决定是否处理

## 生成输出

1. 组装 JSON 数据（格式见 [excel-spec](references/excel-spec.md)）
2. 确认输出路径（默认 `./DVP_<研究名>_<日期>.xlsx`）
3. 执行生成脚本：
   ```
   python3 ${CLAUDE_SKILL_DIR}/scripts/generate_xlsx.py <json_path> --output <output_path>
   ```
4. 如 openpyxl 未安装，先执行 `pip3 install openpyxl`

生成完成后展示预览摘要。

## 澄清原则

- 信息不完整或有歧义时才询问用户，不制造不必要的对话轮次
- 每次只澄清一个主题
- 用户可随时说"跳过"、"稍后填写"、"返回修改"
- 展示当前进度（如"章节 4/8"），让用户知道进展位置

## 辅助文件

| 文件 | 用途 |
|------|------|
| [references/flow-guide.md](references/flow-guide.md) | 详细流程指南 |
| [references/excel-spec.md](references/excel-spec.md) | JSON schema 与 Excel 格式规则 |
| [references/section-catalog.md](references/section-catalog.md) | 常见 DVP 章节目录（建议性） |
| [references/review-rules.md](references/review-rules.md) | 逻辑审查规则定义（R1-R6） |
| [references/example-output.md](references/example-output.md) | 完整 JSON + Excel 输出示例 |
| [scripts/generate_xlsx.py](scripts/generate_xlsx.py) | Excel 生成脚本 |
