# Excel 输出规范

## JSON Schema

生成脚本 `scripts/generate_xlsx.py` 接受以下 JSON 结构：

```json
{
  "meta": {
    "study_name": "string — 研究名称",
    "protocol_number": "string — 方案编号",
    "version": "string — DVP 版本号",
    "date": "string — 日期 (YYYY-MM-DD)",
    "author": "string — 作者",
    "output_path": "string — 输出文件路径（可选，脚本参数优先）"
  },
  "sections": [
    {
      "title": "string — 章节标题",
      "type": "key-value | table | narrative",
      "rows": [],
      "columns": [],
      "content": ""
    }
  ],
  "formatting": {
    "header_color": "string — 十六进制颜色（默认 4472C4）",
    "font": "string — 字体（默认 Calibri 11）"
  }
}
```

### Section 类型

#### key-value — 键值对

用于研究信息、版本历史等结构化描述数据。

```json
{
  "title": "研究信息",
  "type": "key-value",
  "rows": [
    {"field": "研究名称", "value": "ABC-123"},
    {"field": "方案编号", "value": "PROTO-2024-001"}
  ]
}
```

#### table — 表格

用于编辑检查、数据列表等行列数据。

```json
{
  "title": "编辑检查",
  "type": "table",
  "columns": ["检查ID", "变量", "条件", "消息", "严重程度"],
  "rows": [
    ["EC001", "DOB", "> 研究日期", "出生日期晚于研究日期", "错误"],
    ["EC002", "SEX", "notin [M,F]", "性别值不在有效范围内", "错误"]
  ]
}
```

- `columns` 数组定义列头，长度决定列数
- `rows` 中每个子数组长度应与 `columns` 一致

#### narrative — 叙述文本

用于背景说明、目的、范围等段落内容。

```json
{
  "title": "背景",
  "type": "narrative",
  "content": "本 DVP 适用于 ABC-123 研究..."
}
```

## Excel 格式规则

### 元信息区域

| 元素 | 样式 |
|------|------|
| 标题 "DVP - 数据验证计划" | Calibri 16pt 粗体，颜色 #1F4E79，合并 4 列 |
| 字段标签 | Calibri 11pt 粗体 |
| 字段值 | Calibri 11pt 常规 |

### 章节标题行

| 元素 | 样式 |
|------|------|
| 背景色 | #D6E4F0（浅蓝） |
| 字体 | Calibri 12pt 粗体，颜色 #1F4E79 |
| 边框 | 薄边框，颜色 #B4C6E7 |
| 对齐 | 左对齐，垂直居中，合并整行 |

### 表头行

| 元素 | 样式 |
|------|------|
| 背景色 | #4472C4（蓝色） |
| 字体 | Calibri 11pt 粗体，白色 |
| 边框 | 薄边框，颜色 #B4C6E7 |
| 对齐 | 水平居中，垂直居中，自动换行 |

### 数据行

| 元素 | 样式 |
|------|------|
| 字体 | Calibri 11pt 常规 |
| 边框 | 薄边框，颜色 #B4C6E7 |
| 对齐 | 自动换行，顶部对齐 |
| 交替行色 | 偶数行 #F2F7FB（极浅蓝） |

### 列宽

- 自动根据内容调整，公式：`min(内容最大长度 + 4, 50)`
- 最小宽度 12 字符
- 最大宽度 50 字符

### 打印设置

- 方向：横向
- 适应宽度：1 页
- 适应高度：自动

## 验证规则

脚本在生成前校验 JSON：

1. 根对象必须是 `dict`
2. 必须包含 `sections` 数组
3. 每个 section 必须有 `title`
4. section `type` 必须是 `key-value`、`table`、`narrative` 之一
5. table 类型 section 的每行数据长度应与 `columns` 一致
