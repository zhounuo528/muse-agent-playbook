# 发现源聚合【随仓附带】

把四个单源抓取子技能（ArXiv / GitHub 趋势 / HN＋Product Hunt / RSS）打包成一个供料入口，给「每日精选」类任务供新鲜内容。

## 按需选源抓取
先判断用户要什么：AI 论文走 ArXiv，开发者项目走 GitHub trending，科技讨论和新产品走 HN 与 Product Hunt，用户自己订阅的源走 RSS。一次任务只抓相关的源，不全抓。

| 子技能 | 命令（相对本聚合目录） | 说明 |
|---|---|---|
| local-arxiv | `local-arxiv/bin/arxiv` | 默认最近 1 天 cs.AI＋cs.LG＋cs.CL，每类最多 20 篇；`--days`/`--category`/`--max` 调范围。输出 papers 含标题、作者、摘要、链接、发布时间 |
| local-github-trending | `local-github-trending/bin/gh-trending` | 默认全语言日榜；`--lang` 按语言过滤，`--since daily|weekly|monthly` 切范围。输出 repos 含名称、描述、语言、star 数与今日新增 |
| local-hn-ph | `local-hn-ph/bin/hn-ph` | `--hn` 取 HN 热门，`--ph` 取 PH 推荐，`--all` 两边都要；`--filter` 可选 ai/gaming/devtools/tech 可多选。注意 PH 抓的是首页推荐位快照，不是得票榜，不要声称排名 |
| local-rss | `local-rss/bin/rss` | 默认抓全部订阅源最近 3 天条目；`--days`/`--filter` 调范围，`--list` 列源，`--add`/`--remove` 维护订阅。订阅清单在子技能目录的 `feeds.json`（本仓只附 `feeds.example.json` 示例，你的订阅清单是你自己的数据，自己建） |

每个子技能目录里有自己的 SKILL.md，参数细节以它为准。四个子技能全部免 Key。

## 去重与挑选
- 输出给用户前，一律对照一份你自己维护的「已推送日志」（纯文本，一行一条链接或标题即可）去重，已经推过的不再推。
- 按用户画像挑：与用户关注方向相关、有 demo 或代码、标题有意思的优先。ArXiv 摘要是英文，展示时用中文一句话概括，不直接贴英文。
- 每条配一句为什么值得看，由你根据标题和用户画像来写；脚本只给结构化数据。
- 口味挑选由你按画像直接做，不要外包给打分模型——打分模型不懂具体使用场景，容易被关键词抬分。
- 不要高频刷源，每个源一天查 1–2 次足够；新闻类结果可缓存几小时再查。

## 如实说明局限
- 抓取失败时看输出里的 errors 字段，如实说哪个源没抓到，不声称全量覆盖。
- ArXiv 只按提交日期取最新，不做热度排序。GitHub trending 抓的是公开榜单页，页面改版可能导致解析失效，失效时如实说。PH 没有票数数据。
- RSS 只读公开内容，登录墙和付费墙抓不到。部分源的 summary 长短不一，展示时注意长度。

## Alternatives for better results
- 时间紧时只抓 HN 和 RSS 两个最稳的源；ArXiv 和 trending 适合周末深挖。
- 用户问起冷门方向时，用 `--filter` 或 `--category` 先缩小范围，比全量抓回来再筛更省。
