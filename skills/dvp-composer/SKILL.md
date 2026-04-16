---
name: dvp-composer
description: >
  DVP（数据验证计划）生成器。This skill should be used when the user asks to
  "编写DVP", "创建数据验证计划", "生成DVP", "compose DVP", "create DVP", "help with DVP",
  "write a DVP", "edit checks", "编辑检查", "数据验证", "数据质量", "data quality",
  or mentions clinical data verification plans, edit check definitions, or data quality
  planning for a clinical study. 通过七阶段工作流（资料收集→探索→模板确认→规则生成→全量生成→质量检查→生成Excel）输出格式化 DVP。
argument-hint: "[Protocol/CRF/DMP 文件路径，或研究名称]"
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

通过七阶段工作流生成 DVP（Data Verification Plan，数据验证计划），输出格式化 Excel 文件。

## 启动

1. 创建全部 7 个阶段任务：

```
TaskCreate: "阶段 1：资料收集"
TaskCreate: "阶段 2：探索"          (blockedBy: 阶段 1)
TaskCreate: "阶段 3：模板确认"      (blockedBy: 阶段 2)
TaskCreate: "阶段 4：规则生成"      (blockedBy: 阶段 3)
TaskCreate: "阶段 5：全量生成"      (blockedBy: 阶段 4)
TaskCreate: "阶段 6：质量检查"      (blockedBy: 阶段 5)
TaskCreate: "阶段 7：生成 Excel"    (blockedBy: 阶段 6)
```

2. 读取阶段 1 定义文件，开始执行。

## 执行

按阶段顺序执行。每个阶段：
- TaskUpdate 为 `in_progress`
- 读取对应的阶段定义文件
- 完成后 TaskUpdate 为 `completed`

## 阶段定义

| # | 阶段 | 定义文件 |
|---|------|---------|
| 1 | 资料收集 | [stage-1-raw-materials](references/stages/stage-1-raw-materials.md) |
| 2 | 探索 | [stage-2-exploration](references/stages/stage-2-exploration.md) |
| 3 | 模板确认 | [stage-3-template-confirmation](references/stages/stage-3-template-confirmation.md) |
| 4 | 规则生成 | [stage-4-rule-generation](references/stages/stage-4-rule-generation.md) |
| 5 | 全量生成 | [stage-5-full-generation](references/stages/stage-5-full-generation.md) |
| 6 | 质量检查 | [stage-6-quality-check](references/stages/stage-6-quality-check.md) |
| 7 | 生成 Excel | [stage-7-generate-excel](references/stages/stage-7-generate-excel.md) |
