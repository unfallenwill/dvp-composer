---
name: dvp-composer
description: >
  DVP（数据验证计划）生成器。This skill should be used when the user asks to
  "编写DVP", "创建数据验证计划", "生成DVP", "compose DVP", "create DVP", "help with DVP",
  "write a DVP", "edit checks", "编辑检查", "数据验证", "数据质量", "data quality",
  or mentions clinical data verification plans, edit check definitions, or data quality
  planning for a clinical study. 通过六阶段工作流（原料收集→模板确认→规则生成→全量生成→质量检查→生成Excel）输出格式化 DVP。
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

通过六阶段工作流生成 DVP（Data Verification Plan，数据验证计划），输出格式化 Excel 文件。

## 启动方式

- 用户提供了文件路径参数 → 直接进入阶段 1，读取并提取文档信息
- 用户仅描述需求 → 创建任务后进入阶段 1，通过问答收集信息

## 适用范围

- 为临床研究项目创建 DVP 文档
- 支持读取 Protocol、CRF、DMP 等参考文档
- DVP 章节结构由用户决定，不强制固定模板
- 输出为 .xlsx 格式

## 不做什么

- 不连接 EDC/CDMS 系统
- 不验证内容是否符合监管要求
- 不生成 PDF/Word 输出（仅 .xlsx）

## 工作流

```
[开始]
  → 1. 原料收集
  ⇄ 2. 模板确认          ← 用户补充新信息时回到阶段 1
  → 3. 规则生成
  → 4. 全量生成
  → 5. 质量检查
  → 6. 生成 Excel
→ [结束]
```

### 设计原则

- **阶段自治**：每个阶段是黑盒，只关心自己的输入和输出
- **最小传递**：只传递下游需要的数据，能少则少
- **按需索取**：阶段不关心数据怎么来的，缺了就报告出来
- **乐观推进**：能走就走，不够就记
- **发现式回退**：用户看到具体产物才发现缺什么，此时回到阶段 1 补充

### 回退规则

| 情形 | 根因 | 处理 |
|------|------|------|
| 用户补充了信息 | 输入变了 | 回到阶段 1 |
| 用户纠正了思路 | 处理错了 | 当前阶段修正 |

### 任务追踪

工作流启动时，立即创建全部 6 个阶段任务：

```
TaskCreate: "阶段 1：原料收集"
TaskCreate: "阶段 2：模板确认"    (blockedBy: 阶段 1)
TaskCreate: "阶段 3：规则生成"    (blockedBy: 阶段 2)
TaskCreate: "阶段 4：全量生成"    (blockedBy: 阶段 3)
TaskCreate: "阶段 5：质量检查"    (blockedBy: 阶段 4)
TaskCreate: "阶段 6：生成 Excel"  (blockedBy: 阶段 5)
```

每个阶段开始时 TaskUpdate 为 `in_progress`，完成时更新为 `completed` 并在 description 中记录产出摘要。

回退处理：当用户补充信息触发回退到阶段 1 时，将阶段 2 及之后所有已完成的任务状态重置为 `pending`。

## 阶段定义

| 阶段 | 职责 | 详细定义 |
|------|------|---------|
| 1. 原料收集 | 收集文档和信息，澄清歧义 | [stage-1-raw-materials](references/stages/stage-1-raw-materials.md) |
| 2. 模板确认 | 确认章节结构、列定义和格式 | [stage-2-template-confirmation](references/stages/stage-2-template-confirmation.md) |
| 3. 规则生成 | 为各章节定义生成规则/逻辑 | [stage-3-rule-generation](references/stages/stage-3-rule-generation.md) |
| 4. 全量生成 | 批量生成所有章节的完整数据 | [stage-4-full-generation](references/stages/stage-4-full-generation.md) |
| 5. 质量检查 | 验证数据的正确性和完整性 | [stage-5-quality-check](references/stages/stage-5-quality-check.md) |
| 6. 生成 Excel | 输出格式化 Excel 文件 | [stage-6-generate-excel](references/stages/stage-6-generate-excel.md) |

执行某个阶段前，先读取对应的阶段定义文件。

## 澄清原则

- 信息不完整或有歧义时才询问用户，不制造不必要的对话轮次
- 每次只澄清一个主题
- 用户可随时说"跳过"、"稍后填写"、"返回修改"
- 当前阶段编号始终可见，让用户知道进展位置

## 辅助文件

| 文件 | 用途 |
|------|------|
| [references/stages/](references/stages/) | 六个阶段的独立定义文件 |
| [references/excel-spec.md](references/excel-spec.md) | JSON schema 与 Excel 格式规则 |
| [references/section-catalog.md](references/section-catalog.md) | 常见 DVP 章节目录（建议性） |
| [references/review-rules.md](references/review-rules.md) | 质量检查规则定义（R1-R6 + 外部检查） |
| [references/example-output.md](references/example-output.md) | 各阶段中间产物 + 最终输出示例 |
| [scripts/generate_xlsx.py](scripts/generate_xlsx.py) | Excel 生成脚本 |
