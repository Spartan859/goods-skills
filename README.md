# goods-skills

适用于 Codex / ChatGPT 的可复用技能集合。

## 技能

### QQ 评论排单转表格

`qq-comment-to-sheet` 将带时间戳的 QQ 群、微信群评论、文本或截图转换为固定九槽位的商品排单 Markdown 表格。

支持：

- 商品全称、简称和用户自定义别名
- 先到先得与“提”优先单
- “凹”包揽剩余库存
- 超售截断与未分配记录
- 固定商品顺序和空槽位输出

该技能只处理排单数据转换，不处理闲聊、图片生成或一般数据分析。

## 安装

将 `qq-comment-to-sheet` 目录复制到 Codex 的用户技能目录：

```bash
mkdir -p "$HOME/.agents/skills"
cp -R qq-comment-to-sheet "$HOME/.agents/skills/"
```

也可将目录放入项目的 `.agents/skills/`，作为仓库级技能使用。

## 目录结构

```text
goods-skills/
└── qq-comment-to-sheet/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        └── worked-examples.md
```

## 示例

输入可以是文本：

```text
09:01 | AB | 千2
09:02 | CD | 提善1
09:03 | EF | 千3
```

输出为 9 行商品、9 个库存槽位的 Markdown 表格。完整示例见
[`qq-comment-to-sheet/references/worked-examples.md`](qq-comment-to-sheet/references/worked-examples.md)。
