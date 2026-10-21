# 规范化输入契约

生成脚本只接受 JSON 对象。未知字段会导致生成失败，避免输入被静默忽略。

## 公共字段

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `source_mode` | string | 是 | `arrangement` 或 `kidney` |
| `title` | string | 否 | 标题前缀；为空时输出“排表”“肾表” |
| `products` | array | 是 | 排表或空数组 |
| `kidney_rows` | array | 是 | 直接肾表数据或空数组 |

## 排表模式

```json
{
  "source_mode": "arrangement",
  "title": "示例团",
  "products": [
    {
      "name": "梨",
      "allocations": [
        "阿甲", "阿甲", null, null, null,
        null, null, null, null, null
      ],
      "unit_price": 400,
      "exchange_rate": 0.0455,
      "adjustment": 6
    }
  ],
  "kidney_rows": []
}
```

商品字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `name` | string | 是 | 商品名称 |
| `allocations` | array | 是 | 最多 10 项；空值使用 `null` |
| `price` | number | 条件 | 固定价格；不可与计算参数同时使用 |
| `unit_price` | number | 条件 | 单价 |
| `exchange_rate` | number | 条件 | 汇率 |
| `adjustment` | number | 否 | 调价，默认 0 |

有非空配位时，必须提供 `price`，或同时提供 `unit_price` 与 `exchange_rate`。至少一个配位必须非空。

## 直接肾表模式

```json
{
  "source_mode": "kidney",
  "title": "示例团",
  "products": [],
  "kidney_rows": [
    {"cn": "甲", "amount": 41.57, "points": null}
  ]
}
```

肾表行字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `cn` | string | 是 | 非空用户名或 CN |
| `amount` | number | 是 | 国际金额，映射为应付 |
| `points` | integer/null | 否 | 点数；缺失时输出空单元格 |

`products` 必须为空，`kidney_rows` 至少包含一行。

## 错误条件

以下情况必须停止生成，而不是修正或猜测：

- 未知字段、错误类型、空 CN、空商品名。
- 超过 10 个配位。
- 排表有占用但缺少价格信息。
- 固定价格与价格计算参数同时出现。
- 非有限数字、负数点数或非整数点数。
- 模式与数据结构不匹配。
