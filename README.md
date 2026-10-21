# goods-skills

适用于 Codex / ChatGPT 的可复用技能集合。

## 技能

### QQ 评论排单转表格

`qq-comment-to-sheet` 将带时间戳的 QQ 群、微信群评论、文本或截图转换为商品排单 Markdown 表格。

支持：

- 可配置商品集合、商品行数和每商品槽位数
- 商品全称、简称和用户自定义别名
- 先到先得与“提”优先单
- “凹”包揽剩余库存
- 超售截断与未分配记录
- 商品顺序和空槽位输出

默认商品行数和槽位数都是 9，但都只是默认值。生成前必须从资料推断或使用默认值，并向用户确认。

### 排表/肾表 XLSM 生成

`order-to-kidney-sheet` 从已结构化的排表或“CN + 国际金额”肾表数据生成 `.xlsm`：

- 可配置上半部配列数，默认 10。
- 排表与肾表标题均合并单元格并居中。
- 有排表和价格信息时生成上半部排表与下半部肾表。
- 已提供肾表时排表留空，并统一输出 `CN、应付、点数`。
- 支持表格、文本或图片抽取后的规范化数据。
- 保留价格公式和原汇总宏；二维码由本地记忆默认值或用户当次提供，公开仓库不保存个人二维码。

生成前必须从资料推断或使用默认配列数，并询问用户确认，不得静默采用数量。

原始 QQ 抢单评论继续使用 `qq-comment-to-sheet`。

## 安装

将需要的技能目录复制到 Codex 的用户技能目录：

```bash
mkdir -p "$HOME/.agents/skills"
cp -R qq-comment-to-sheet order-to-kidney-sheet "$HOME/.agents/skills/"
```

也可将目录放入项目的 `.agents/skills/`，作为仓库级技能使用。`order-to-kidney-sheet` 的生成脚本需要 Python 3.10+、`openpyxl` 与 `Pillow`。

## 目录结构

```text
goods-skills/
├── qq-comment-to-sheet/
│   ├── SKILL.md
│   └── references/
│       └── worked-examples.md
└── order-to-kidney-sheet/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    ├── assets/
    │   └── order-kidney-template.xlsm
    ├── references/
    │   ├── input-schema.md
    │   └── examples.md
    └── scripts/
        └── build_order_kidney_workbook.py
```

## 示例

输入可以是文本：

```text
09:01 | AB | 千2
09:02 | CD | 提善1
09:03 | EF | 千3
```

生成前先确认商品行和槽位数。默认配置为 9 个商品、每商品 9 个槽位；用户也可确认其他数量。完整示例见
[`qq-comment-to-sheet/references/worked-examples.md`](qq-comment-to-sheet/references/worked-examples.md)。

## 宏安全提示

`order-to-kidney-sheet` 内置的 `.xlsm` 模板保留一个只用于汇总工作簿数据的 VBA 宏。打开文件时 Excel 可能显示宏安全提示；请只运行仓库内置、已审阅的汇总宏。
