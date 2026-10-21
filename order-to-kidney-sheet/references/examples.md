# 输入与输出示例

## 一、排表和价格信息

输入：

```text
商品 | 第1配 | 第2配 | 调价 | 汇率 | 单价
梨   | 阿甲   | 阿甲   | 6    | 0.0455 | 400
善   | 阿甲   | 阿庚   | 12   | 0.0455 | 400
```

规范化结果：

```json
{
  "source_mode": "arrangement",
  "title": "测试团",
  "products": [
    {
      "name": "梨",
      "allocations": ["阿甲", "阿甲", null, null, null, null, null, null, null, null],
      "unit_price": 400,
      "exchange_rate": 0.0455,
      "adjustment": 6
    },
    {
      "name": "善",
      "allocations": ["阿甲", "阿庚", null, null, null, null, null, null, null, null],
      "unit_price": 400,
      "exchange_rate": 0.0455,
      "adjustment": 12
    }
  ],
  "kidney_rows": []
}
```

生成结果：

- 梨价格：24.20
- 善价格：30.20
- 阿甲共占 2 点，应付 54.40
- 阿庚共占 1 点，应付 30.20

肾表：

| CN | 应付 | 点数 |
| --- | ---: | ---: |
| 阿甲 | 54.40 | 2 |
| 阿庚 | 30.20 | 1 |

## 二、直接提供肾表图片

图片内容：

| cn | 国际金额 |
| --- | ---: |
| 甲 | 41.57 |
| 乙 | 10.71 |

规范化结果：

```json
{
  "source_mode": "kidney",
  "title": "示例团",
  "products": [],
  "kidney_rows": [
    {"cn": "甲", "amount": 41.57, "points": null},
    {"cn": "乙", "amount": 10.71, "points": null}
  ]
}
```

工作簿结果：

- 上半部排表数据行留空。
- 下半部使用三列：`CN、应付、点数`。
- 甲应付 41.57，点数空白。
- 乙应付 10.71，点数空白。

## 三、重复 CN

输入 `甲 41.57`、`乙 10.71`、`甲 2.43、点数 2` 时，输出两行：

| CN | 应付 | 点数 |
| --- | ---: | ---: |
| 甲 | 44.00 | 2 |
| 乙 | 10.71 |  |
