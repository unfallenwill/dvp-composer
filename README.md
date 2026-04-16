# DVP Composer

Claude Code Plugin — 交互式 DVP（Data Verification Plan，数据验证计划）生成器，面向 CRO 公司的 Data Manager。

## 功能

- 读取 Protocol / CRF / DMP 参考文档，自动提取关键信息
- 六阶段工作流，每阶段独立、自治、可回退
- 质量检查（R1-R9），包含内部逻辑检查和外部正确性验证
- 输出格式化 Excel (.xlsx) 文件

## 安装

将本仓库克隆到 Claude Code 插件目录：

```bash
git clone <repo-url> ~/.claude/plugins/dvp-composer
```

## 依赖

- Python 3
- [openpyxl](https://openpyxl.readthedocs.io/)（首次运行时自动安装）

## 使用

在 Claude Code 中输入：

```
/dvp-composer path/to/protocol.pdf
```

或直接描述需求：

```
帮我编写 ABC-123 研究的 DVP
```

## 六阶段工作流

| 阶段 | 职责 | 产出 |
|------|------|------|
| 1. 原料收集 | 收集文档和信息，澄清歧义 | 结构化原料数据 |
| 2. 模板确认 | 确认章节结构、列定义和格式 | DVP 模板 |
| 3. 规则生成 | 为各章节定义生成规则/逻辑 | 生成规则集 |
| 4. 全量生成 | 批量生成所有章节的完整数据 | 完整 DVP 数据 |
| 5. 质量检查 | 验证数据的正确性和完整性（R1-R9） | 检查报告 |
| 6. 生成 Excel | 输出格式化 Excel 文件 | .xlsx 文件 |

## License

MIT
