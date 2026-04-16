# 示例输出

以下是一个完整的 DVP 生成过程中各阶段的中间产物和最终输出示例。

## 阶段 1 产物：原料数据

文件：`/tmp/dvp_raw_materials.json`

```json
{
  "sources": {
    "protocol": {
      "extracted": {
        "study_name": "ABC-123",
        "protocol_number": "PROTO-2024-001",
        "phase": "III期",
        "design": "随机、双盲、安慰剂对照、多中心",
        "indication": "2型糖尿病",
        "target_enrollment": "300",
        "visit_schedule": [
          {"name": "Screening", "window": "D-28 ~ D-1", "day": "-28 ~ -1", "domains": "人口学、知情同意、入排标准"},
          {"name": "Baseline (Day 1)", "window": "D1", "day": "0", "domains": "生命体征、实验室检查、合并用药"},
          {"name": "Visit 2 (Week 4)", "window": "±3天", "day": "28", "domains": "生命体征、AE、疗效评估"},
          {"name": "Visit 3 (Week 8)", "window": "±3天", "day": "56", "domains": "生命体征、实验室检查、AE"},
          {"name": "Visit 4 (Week 12)", "window": "±5天", "day": "84", "domains": "生命体征、实验室检查、AE、疗效评估"},
          {"name": "End of Study", "window": "±5天", "day": "84", "domains": "全部数据域"},
          {"name": "Follow-up", "window": "D+14", "day": "98", "domains": "AE、合并用药"}
        ]
      },
      "missing": ["研究中心数量", "申办方名称"]
    },
    "crf": {
      "extracted": {
        "forms": [
          {"name": "人口学", "fields": ["SUBJID", "DOB", "SEX", "ETHNIC"]},
          {"name": "知情同意", "fields": ["ICFDATE", "RFICDATE"]},
          {"name": "生命体征", "fields": ["VSDAT", "SYSBP", "DIABP", "HR", "TEMP", "WEIGHT", "HEIGHT"]},
          {"name": "实验室", "fields": ["LBTEST", "LBORRES", "LBUNIT", "LBNRLO", "LBNRHI"]},
          {"name": "AE", "fields": ["AETERM", "AESTDAT", "AEENDAT", "AESER", "AESDTH", "AESHOSP", "AESDISAB", "AESLIFE", "AESCONG", "AESMIE"]},
          {"name": "合并用药", "fields": ["CMTRT", "CMDOSE", "CMINDC", "CMSTDAT", "CMENDAT"]}
        ],
        "total_fields": 33
      },
      "missing": []
    },
    "dmp": {
      "extracted": {
        "edc_system": "Medidata Rave",
        "query_response_sla": "7个工作日"
      },
      "missing": ["数据库版本", "数据管理流程细节"]
    }
  },
  "user_supplied": {
    "cro_name": "XYZ CRO",
    "dm_name": "张三"
  },
  "clarifications": [
    {"question": "研究设计是什么类型？", "answer": "随机、双盲、安慰剂对照、多中心"}
  ]
}
```

## 阶段 2 产物：探索结果

追加到 `/tmp/dvp_raw_materials.json`：

```json
{
  "exploration": {
    "study_overview": "ABC-123，III期，随机双盲安慰剂对照多中心研究，目标入组300人",
    "complexity": "高（多中心、7个访视、6个数据域）",
    "key_domains": ["人口学", "知情同意", "生命体征", "实验室", "AE", "合并用药"],
    "conflicts": [],
    "ambiguities": [],
    "domain_readiness": [
      {"domain": "人口学", "status": "可生成"},
      {"domain": "知情同意", "status": "可生成"},
      {"domain": "生命体征", "status": "可生成"},
      {"domain": "实验室", "status": "部分可生成", "missing": ["正常范围定义"]},
      {"domain": "AE", "status": "可生成"},
      {"domain": "合并用药", "status": "可生成"}
    ]
  }
}
```

## 阶段 3 产物：模板定义

文件：`/tmp/dvp_template.json`

```json
{
  "meta_schema": {
    "study_name": "required",
    "protocol_number": "required",
    "version": "optional",
    "date": "auto",
    "author": "optional"
  },
  "sections": [
    {
      "title": "研究信息",
      "type": "key-value",
      "fields": ["研究名称", "方案编号", "研究阶段", "研究设计", "适应症", "目标入组人数", "申办方", "CRO", "数据经理"],
      "source": "P+M"
    },
    {
      "title": "访视计划",
      "type": "table",
      "columns": ["访视名称", "访视窗口", "相对天数", "数据域"],
      "source": "P"
    },
    {
      "title": "编辑检查",
      "type": "table",
      "columns": ["检查ID", "数据域", "变量", "条件", "消息", "严重程度"],
      "source": "C+M"
    },
    {
      "title": "数据验证范围",
      "type": "narrative",
      "source": "P+D"
    }
  ],
  "formatting": {
    "header_color": "4472C4",
    "font": "Calibri 11"
  }
}
```

## 阶段 4 产物：生成规则

文件：`/tmp/dvp_rules.json`

```json
{
  "sections": [
    {
      "title": "研究信息",
      "type": "key-value",
      "rules": {
        "field_mapping": [
          {"field": "研究名称", "source": "protocol.study_name"},
          {"field": "方案编号", "source": "protocol.protocol_number"},
          {"field": "研究阶段", "source": "protocol.phase"},
          {"field": "研究设计", "source": "protocol.design"},
          {"field": "适应症", "source": "protocol.indication"},
          {"field": "目标入组人数", "source": "protocol.target_enrollment"},
          {"field": "CRO", "source": "user_supplied.cro_name"},
          {"field": "数据经理", "source": "user_supplied.dm_name"},
          {"field": "申办方", "source": "missing"}
        ]
      }
    },
    {
      "title": "访视计划",
      "type": "table",
      "rules": {
        "source": "protocol.visit_schedule",
        "columns": ["访视名称", "访视窗口", "相对天数", "数据域"]
      }
    },
    {
      "title": "编辑检查",
      "type": "table",
      "rules": {
        "id_prefix": "EC",
        "id_format": "EC{NNN}",
        "start_number": 1,
        "severity_distribution": {"错误": 0.7, "警告": 0.3},
        "variables_to_cover": ["DOB", "SEX", "ICFDATE", "RFICDATE", "SYSBP", "DIABP", "HR", "TEMP", "WEIGHT", "HEIGHT", "LBTEST", "LBORRES", "AESTDAT", "AEENDAT", "AESER", "CMTRT", "CMDOSE"],
        "check_types": ["required", "range", "logical", "consistency"],
        "generation_order": ["required → range → logical → consistency"]
      }
    },
    {
      "title": "数据验证范围",
      "type": "narrative",
      "rules": {
        "topics": ["验证范围", "验证方法", "EDC 系统"],
        "references": ["protocol.study_name", "dmp.edc_system"]
      }
    }
  ]
}
```

## 阶段 5 产物：完整 DVP 数据

文件：`/tmp/dvp_data.json`

```json
{
  "meta": {
    "study_name": "ABC-123",
    "protocol_number": "PROTO-2024-001",
    "version": "1.0",
    "date": "2026-04-16",
    "author": "张三",
    "output_path": "./DVP_ABC-123_2026-04-16.xlsx"
  },
  "sections": [
    {
      "title": "研究信息",
      "type": "key-value",
      "rows": [
        {"field": "研究名称", "value": "ABC-123"},
        {"field": "方案编号", "value": "PROTO-2024-001"},
        {"field": "研究阶段", "value": "III期"},
        {"field": "研究设计", "value": "随机、双盲、安慰剂对照、多中心"},
        {"field": "适应症", "value": "2型糖尿病"},
        {"field": "目标入组人数", "value": "300"},
        {"field": "申办方", "value": "(待补充)"},
        {"field": "CRO", "value": "XYZ CRO"},
        {"field": "数据经理", "value": "张三"}
      ]
    },
    {
      "title": "访视计划",
      "type": "table",
      "columns": ["访视名称", "访视窗口", "相对天数", "数据域"],
      "rows": [
        ["Screening", "D-28 ~ D-1", "-28 ~ -1", "人口学、知情同意、入排标准"],
        ["Baseline (Day 1)", "D1", "0", "生命体征、实验室检查、合并用药"],
        ["Visit 2 (Week 4)", "±3天", "28", "生命体征、AE、疗效评估"],
        ["Visit 3 (Week 8)", "±3天", "56", "生命体征、实验室检查、AE"],
        ["Visit 4 (Week 12)", "±5天", "84", "生命体征、实验室检查、AE、疗效评估"],
        ["End of Study", "±5天", "84", "全部数据域"],
        ["Follow-up", "D+14", "98", "AE、合并用药"]
      ]
    },
    {
      "title": "编辑检查",
      "type": "table",
      "columns": ["检查ID", "数据域", "变量", "条件", "消息", "严重程度"],
      "rows": [
        ["EC001", "人口学", "DOB", "> 研究开始日期", "出生日期不能晚于研究开始日期", "错误"],
        ["EC002", "人口学", "SEX", "notin [M,F]", "性别必须在 M(男) 或 F(女) 范围内", "错误"],
        ["EC003", "知情同意", "ICFDATE", "> RFICDATE", "知情同意签署日期不能晚于任何研究操作日期", "错误"],
        ["EC004", "生命体征", "SYSBP", "< 60 or > 260", "收缩压超出合理范围 (60-260 mmHg)", "警告"],
        ["EC005", "生命体征", "DIABP", "< 30 or > 150", "舒张压超出合理范围 (30-150 mmHg)", "警告"],
        ["EC006", "生命体征", "HR", "< 30 or > 200", "心率超出合理范围 (30-200 bpm)", "警告"],
        ["EC007", "实验室", "LBTEST", "== '' and LBORRES != ''", "有检验结果但未选择检验项目", "错误"],
        ["EC008", "AE", "AESTDAT", "> AEENDAT", "不良事件开始日期不能晚于结束日期", "错误"],
        ["EC009", "AE", "AESER", "== 'Y' and AESDTH != 'Y' and AESHOSP != 'Y' and AESDISAB != 'Y' and AESLIFE != 'Y' and AESCONG != 'Y' and AESMIE != 'Y'", "严重 AE 标记为是，但未选择任何严重程度标准", "错误"],
        ["EC010", "合并用药", "CMTRT", "== '' and CMDOSE != ''", "有药物剂量但未填写药物名称", "错误"]
      ]
    },
    {
      "title": "数据验证范围",
      "type": "narrative",
      "content": "本 DVP 适用于 ABC-123 研究的所有临床数据验证工作。验证范围涵盖 EDC 系统（Medidata Rave）中收集的所有数据域，包括但不限于：人口学数据、知情同意、生命体征、实验室检查、不良事件、合并用药和疗效评估。\n\n数据验证方法包括：\n1. 编辑检查（Edit Checks）— 自动化逻辑检查\n2. 数据一致性审查 — 跨表单数据交叉验证\n3. 来源数据核查（SDV）— 与原始文件对比验证\n4. 医学审查 — 由医学监查员进行医学合理性审查"
    }
  ],
  "formatting": {
    "header_color": "4472C4",
    "font": "Calibri 11"
  }
}
```

## 阶段 7 产物：Excel 文件

生成的 `DVP_ABC-123_2026-04-16.xlsx` 文件包含一个名为 "DVP" 的 Sheet，布局如下：

### 元信息区域（Row 1-7）

| 行 | 内容 | 样式 |
|----|------|------|
| 1 | "DVP - 数据验证计划" | 16pt 粗体深蓝，合并4列 |
| 2 | 研究名称: ABC-123 | 标签粗体 |
| 3 | 方案编号: PROTO-2024-001 | |
| 4 | DVP 版本: 1.0 | |
| 5 | 日期: 2026-04-16 | |
| 6 | 作者: 张三 | |
| 7 | (空行) | |

### 研究信息（Row 8-17）

- Row 8: 章节标题 "研究信息"（浅蓝背景，合并2列）
- Row 9: 表头 "字段" | "值"（蓝色背景，白色粗体）
- Row 10-18: 9 行 key-value 数据（偶数行浅蓝背景）

### 访视计划（Row 20-28）

- Row 20: 章节标题 "访视计划"
- Row 21: 表头 "访视名称" | "访视窗口" | "相对天数" | "数据域"
- Row 22-28: 7 行表格数据

### 编辑检查（Row 30-41）

- Row 30: 章节标题 "编辑检查"
- Row 31: 表头 "检查ID" | "数据域" | "变量" | "条件" | "消息" | "严重程度"
- Row 32-41: 10 行表格数据

### 数据验证范围（Row 43-44）

- Row 43: 章节标题 "数据验证范围"
- Row 44: 叙述文本（自动换行）

### 格式特征

- 列宽自动调整
- 所有单元格有薄边框
- 表头蓝色背景白色文字
- 数据行交替浅蓝背景
- 横向打印设置
