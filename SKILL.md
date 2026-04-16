---
name: dvp-composer
description: >
  交互式 DVP（数据验证计划）生成器。This skill should be used when the user asks to
  "编写DVP"、"创建数据验证计划"、"生成DVP"、"compose DVP"、"create DVP"、"help with DVP"，
  或提及临床数据验证计划相关任务。读取 Protocol/CRF/DMP 文档，通过逐步问答收集信息，
  经逻辑审查后生成格式化的 Excel DVP 文档。
argument-hint: "[Protocol/CRF/DMP 文件路径，或研究名称]"
context: fork
agent: general-purpose
allowed-tools: "Bash(python3 *) Bash(pip3 *) Read Write Grep Glob"
user-invocable: true
---

# DVP Composer

交互式生成 DVP（Data Verification Plan，数据验证计划），输出格式化 Excel 文件。

## 适用范围

- 为临床研究项目创建 DVP 文档
- 支持读取 Protocol、CRF、DMP 等参考文档
- DVP 章节结构由用户决定，不强制固定模板
- 输出为 .xlsx 格式

## 不做什么

- 不连接 EDC/CDMS 系统
- 不验证内容是否符合监管要求
- 不生成 PDF/Word 输出（仅 .xlsx）

## 五阶段流程

执行以下五个阶段，每个阶段有质量门禁。不通过门禁则不能进入下一阶段。

### 阶段一：文档收集与信息提取

1. 询问用户提供参考文档（Protocol / CRF / DMP），可提供任意组合或跳过
2. 读取文档并提取关键信息：
   - Protocol → 研究名称、编号、设计、终点、访视计划
   - CRF → 表单名称、字段列表、字段类型
   - DMP → 数据管理流程、质量标准
3. 将提取结果存入临时文件 `/tmp/dvp_extracted.json`

**门禁 G1**：汇总展示提取信息，标注来源和缺失字段，用户确认完整性后才继续。

详细流程见 [flow-guide](references/flow-guide.md#阶段一文档收集与信息提取)。

### 阶段二：结构发现

1. 询问 DVP 章节结构来源：
   - A) 用户提供现有 DVP 模板 → 读取并提取结构
   - B) 从常见章节选择 → 参考 [section-catalog](references/section-catalog.md)
   - C) 用户口述 → 解析并确认
2. 展示最终章节列表（编号、名称、类型）

**门禁 G2**：用户显式确认章节列表（回复"确认"/"OK"）后才继续。

### 阶段三：逐章节收集数据

1. 逐章节收集内容，一次一个主题
2. 优先使用文档提取的信息**预填充**，用户确认或修改
3. 未覆盖部分通过问答补充
4. 用户可跳过任何章节（留占位符）
5. 表格类章节支持批量粘贴

**门禁 G3**：全部章节完成后展示汇总视图（完成/部分/跳过），用户确认后继续。

### 阶段四：逻辑审查

1. 按 [review-rules](references/review-rules.md) 执行六项检查：
   - R1 字段引用完整性
   - R2 数据一致性
   - R3 命名规范
   - R4 业务逻辑合理性
   - R5 规则冲突检测
   - R6 交叉引用一致性
2. 输出审查报告（✅/⚠️/❌）

**门禁 G4**：所有 ❌ 必须修正，⚠️ 用户决定是否处理。

### 阶段五：生成输出

1. 组装 JSON 数据（格式见 [excel-spec](references/excel-spec.md)）
2. 确认输出路径（默认 `./DVP_<研究名>_<日期>.xlsx`）
3. 执行生成脚本：
   ```
   python3 ${CLAUDE_SKILL_DIR}/scripts/generate_xlsx.py <json_path> --output <output_path>
   ```
4. 如 openpyxl 未安装，先执行 `pip3 install openpyxl`

**门禁 G5**：验证文件生成成功，展示预览摘要，用户确认满意。

## 交互原则

- 每次只问一个主题，不展示长表单
- 预填充文档信息减少重复输入
- 用户可随时说"跳过"、"稍后填写"、"返回修改"
- 审查报告中的错误项必须修正才能继续

## 辅助文件

| 文件 | 用途 |
|------|------|
| [references/flow-guide.md](references/flow-guide.md) | 五阶段详细交互流程与门禁规范 |
| [references/excel-spec.md](references/excel-spec.md) | JSON schema 与 Excel 格式规则 |
| [references/section-catalog.md](references/section-catalog.md) | 常见 DVP 章节目录（建议性） |
| [references/review-rules.md](references/review-rules.md) | 逻辑审查规则定义（R1-R6） |
| [references/example-output.md](references/example-output.md) | 完整 JSON + Excel 输出示例 |
| [scripts/generate_xlsx.py](scripts/generate_xlsx.py) | Excel 生成脚本 |
