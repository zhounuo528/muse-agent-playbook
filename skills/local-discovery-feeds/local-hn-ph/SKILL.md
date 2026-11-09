# HN / Product Hunt 聚合（每日精选供料）

## 用途
抓 Hacker News 热门和 Product Hunt 推荐产品，按兴趣过滤，给每日精选供料。

## 数据来源
- HN：官方 Firebase API（https://hacker-news.firebaseio.com/v0/），免 Key，稳定。返回 top 500，默认取前 30，可调。
- PH：首页 HTML 抓取（best-effort）。官方 API 需要 OAuth 申请，本技能未接。注意：抓到的是首页推荐位产品，不是严格意义上的今日得票榜单；无票数数据。

## 工具
```bash
bin/hn-ph --hn                    # HN top 30
bin/hn-ph --hn --count 15         # HN 前 15
bin/hn-ph --hn --filter ai gaming # 按兴趣过滤
bin/hn-ph --ph                    # PH 推荐产品
bin/hn-ph --all                   # 两边都要
bin/hn-ph --all --filter ai devtools
```
filter 可选值：ai / gaming / devtools / tech，可多选。
输出 JSON：hackernews[] 含 title/url/hn_url/score/comments/by/time；producthunt[] 含 title/tagline/url。

## 使用规则
1. HN 时间字段脚本已换算展示口径，引用时注明。
2. 「为什么值得看」一句话由 agent 根据标题和用户画像来写，脚本只给结构化数据。
3. PH 数据是推荐位快照，不要声称是今日得票榜。
4. 输出给用户前，对照你自己的已推送日志去重。
5. HN API 限流宽松，但不要高频刷；一天查几次足够。

## 已知限制
- PH 官方 API 需 OAuth，未接；如以后拿到凭据可升级。
- PH 无票数/排名数据，排序按页面出现顺序。
- HN 评论数是快照值，会变化。
