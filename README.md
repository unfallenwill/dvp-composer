# DVP Composer

Claude Code Plugin — 交互式 DVP（Data Verification Plan，数据验证计划）生成器，面向 CRO 公司的 Data Manager。

## 功能

- 读取 Protocol / CRF / DMP 参考文档，自动提取关键信息
- 五阶段引导式问答，每阶段有质量门禁
- 逻辑审查（R1-R6），自动检测字段引用、数据一致性、命名规范等问题
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

## 五阶段流程

| 阶段 | 内容 | 门禁 |
|------|------|------|
| 1. 文档收集 | 读取 Protocol/CRF/DMP，提取信息 | G1: 完整性确认 |
| 2. 结构发现 | 确定 DVP 章节结构 | G2: 结构确认 |
| 3. 内容填充 | 逐章节收集数据 | G3: 填充汇总 |
| 4. 逻辑审查 | R1-R6 六项检查 | G4: 审查通过 |
| 5. 生成输出 | 输出 Excel 文件 | G5: 输出验证 |

## License

MIT
