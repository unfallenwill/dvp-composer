# Dvp Composer

`Dvp-Composer` 是一个 Claude Code Plugin，面向 CRO（合同研究组织）公司的 **Data Manager（数据经理）**，帮助其编写 **DVP（Data Verification Plan，数据验证计划）**。

## 背景

DVP 是临床数据管理中的核心文档，用于规划如何验证临床研究数据的完整性、准确性和一致性。Data Manager 在每个研究项目中都需要编写 DVP，这是一项高频但格式相对固定的工作。本 plugin 旨在辅助 DM 高效、规范地完成 DVP 的编写。

## 插件开发工作流

使用 `plugin-dev` 插件提供的技能完成插件开发任务：

| 阶段 | 技能 | 用途 |
|------|------|------|
| 结构搭建 | `plugin-dev:plugin-structure` | 初始化插件目录结构、清单配置 |
| 技能开发 | `plugin-dev:skill-development` | 创建和迭代 SKILL.md 及辅助文件 |
| 命令开发 | `plugin-dev:command-development` | 创建斜杠命令 |
| 代理开发 | `plugin-dev:agent-development` | 创建专用子代理 |
| 钩子开发 | `plugin-dev:hook-development` | 创建事件钩子 |
| MCP 集成 | `plugin-dev:mcp-integration` | 集成 MCP 服务 |
| 配置管理 | `plugin-dev:plugin-settings` | 管理插件设置 |
| 文档维护 | `claude-md-management:claude-md-improver` | 审计和改进 CLAUDE.md |

### 可用 Agent

| Agent | 模型 | 用途 |
|-------|------|------|
| `plugin-dev:skill-reviewer` | inherit | 审查技能质量：描述、内容组织、渐进式加载 |
| `plugin-dev:plugin-validator` | inherit | 验证插件整体结构是否符合规范 |
| `plugin-dev:agent-creator` | sonnet | 辅助创建新的子代理 |
| `claude-code-guide` | haiku | 查询 Claude Code 最新文档：hooks、settings、MCP、SDK 等 |

### 工作流程

1. 用 `plugin-dev:plugin-structure` 确认目录结构符合规范
2. 用 `plugin-dev:skill-development` 创建/迭代具体技能
3. 用 `plugin-dev:skill-reviewer` agent 审查技能质量
4. 用 `plugin-dev:plugin-validator` agent 验证插件整体
5. 完成后用 `/claude-md-management:claude-md-improver` 更新 CLAUDE.md

## Commit Convention

Every git commit must include this trailer at the end of the message:

```
Co-Authored-By: GLM 5.1 <noreply@z.ai>
```
