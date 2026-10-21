# 输入与输出示例

## 一、排表和价格信息

用户资料确认：

- 配列数：10
- 商品行：2
- 商品顺序：梨、善

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
  "allocation_count": 10,
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

用户确认：

- 配列数无法从图片推断，采用默认 10，并已确认。
- 排表数据留空。

规范化结果：

```json
{
  "source_mode": "kidney",
  "title": "示例团",
  "allocation_count": 10,
  "products": [],
  "kidney_rows": [
    {"cn": "甲", "amount": 41.57, "points": null},
    {"cn": "乙", "amount": 10.71, "points": null}
  ]
}
```

工作簿结果：

- 上半部配位表头为第1配至第allocation_count配（本示例默认 10），数据行留空。
- 下半部使用三列：`CN、应付、点数`。
- 甲应付 41.57，点数空白。
- 乙应付 10.71，点数空白。

## 三、可变配列数

如果资料只有“第1配”至“第4配”，则候选 `allocation_count=4`；向用户确认后，上半部生成 4 个配位列，而不是默认 10 列。

## 四、重复 CN

输入 `甲 41.57`、`乙 10.71`、`甲 2.43、点数 2` 时，输出两行：

| CN | 应付 | 点数 |
| --- | ---: | ---: |
| 甲 | 44.00 | 2 |
| 乙 | 10.71 |  |


## 五、二维码

- 本地记忆提供有效默认二维码时，写入该路径作为 `qr_image`，无需再次询问。
- 本地记忆没有有效默认值时，询问用户是否需要二维码。
- 用户需要时，请用户提供图片并写入 `qr_image`。
- 用户不需要时将 `qr_image` 设为 `null` 或省略。

示例：

```json
{
  "qr_image": "/path/to/local-qr.png"
}
```
