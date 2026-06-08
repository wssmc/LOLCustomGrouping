# LOL 自定义分组工具：CSV 数据源版

这个版本把选手名单和历史评分从 HTML 中拆出来，放到仓库的 CSV 文件中。

## 文件结构

```text
lol-team-randomizer/
├── index.html
└── data/
    ├── players.csv
    ├── scores.csv
    └── match_history.csv
```

## players.csv

记录“有哪些选手可以被选择”。

格式：

```csv
player_id,nickname,enabled
top01,上单哥,1
jug01,打野哥,1
```

字段说明：

| 字段 | 含义 |
|---|---|
| player_id | 选手唯一 ID，不建议频繁修改 |
| nickname | 显示名称，可以修改 |
| enabled | 是否默认选中，1 表示选中，0 表示不选中 |

## scores.csv

记录“历史评分汇总”。

格式：

```csv
player_id,games,total
top01,5,420
jug01,5,390
```

字段说明：

| 字段 | 含义 |
|---|---|
| player_id | 对应 players.csv 中的 player_id |
| games | 已记录局数 |
| total | 历史总分 |

页面中的实力分计算方式：

```text
平均分 = total / games
```

没有历史记录的新玩家使用页面中的“新玩家默认分数”。

## match_history.csv

这是可选文件，用来保存每局明细。

格式：

```csv
match_id,date,player_id,nickname,team,score
```

当前网页会提供“导出本局明细 CSV”，但不会自动追加到仓库中的 match_history.csv。

## 使用流程

### 开局前

1. 打开网页。
2. 网页自动读取：
   - `data/players.csv`
   - `data/scores.csv`
3. 勾选本局参赛选手。
4. 选择：
   - 普通随机
   - 实力随机
5. 点击“生成分组”。

### 游戏结束后

1. 在实力随机模式下录入每个玩家本局分数。
2. 点击“保存本局分数”。
3. 点击“导出 scores.csv”。
4. 把下载的新 `scores.csv` 上传到 GitHub 仓库，覆盖 `data/scores.csv`。

## 重要说明

GitHub Pages 是静态网页，前端不能安全地自动写回仓库文件。

因此本版本采用安全方案：

```text
网页读取仓库 CSV
→ 页面内更新数据
→ 导出新的 CSV
→ 管理员手动上传覆盖仓库 CSV
```

不要把 GitHub Token 写进 HTML 或 JS 文件里。
