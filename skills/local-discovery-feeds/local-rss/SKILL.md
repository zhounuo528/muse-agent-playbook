# RSS 阅读器（每日精选供料）

## 用途
聚合用户订阅的 RSS 源，取最近条目、去重、按时间排序、按兴趣关键词过滤，给每日精选供料。

## 工具
```bash
bin/rss                    # 抓全部源（默认 3 天内）
bin/rss --days 7           # 取 7 天内
bin/rss --filter ai gaming # 按兴趣过滤
bin/rss --list             # 列出订阅源
bin/rss --add "URL" --name "名字"  # 添加源
bin/rss --remove "URL"     # 删除源
```
filter 可选值：ai / gaming / devtools / tech，可多选。
输出 JSON：entries[] 含 feed/title/url/published/summary，errors[] 列出抓取失败的源。

## 订阅清单
配置文件是本子技能目录下的 `feeds.json`（结构见随附的 `feeds.example.json`）。订阅清单属于用户自己的数据，本仓不附任何人的真实订阅列表；从示例复制一份改成你自己的即可。用户说订阅/取消某源时用 --add/--remove 维护。

## 使用规则
1. 输出给用户前，对照你自己的已推送日志去重。
2. 「为什么值得看」一句话由 agent 写，脚本只给结构化数据。
3. errors 非空时不要声称全量覆盖，如实说明哪个源没抓到。
4. 不要高频刷源；一天查几次足够，--days 按需放大。
5. 依赖 feedparser：换环境时 `pip install feedparser`。

## 已知限制
- 只读公开 RSS；需登录/付费墙的内容抓不到。
- 部分源的 summary 是全文、有的是摘要，展示时注意长度。
