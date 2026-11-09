# GitHub 每日趋势

## 用途
抓取 github.com/trending 热榜，给每日精选供料，适合找好玩的新项目。

## 工具
```bash
bin/gh-trending                  # 默认：全语言 daily 榜
bin/gh-trending --lang python    # 只看 Python
bin/gh-trending --since weekly   # 周榜（daily/weekly/monthly）
```
输出 JSON：repos[] 含 name/description/language/stars/forks/stars_today/url，另有 count/lang/since 字段。

## 使用规则
1. 输出给用户前，对照你自己的已推送日志去重。
2. 挑项目时优先：与用户关注方向相关、开发提效类、有在线 demo 的、star 增速快的。
3. 给用户时一句话说清这是干嘛的＋为什么值得看＋链接。
4. 一天查一次足够；weekly 榜适合周末看。

## 已知限制
- 抓的是公开 trending 页 HTML；GitHub 改版面可能导致解析失效，失效时如实说。
- language 字段有时为空（页面没标），不影响主要信息。
- 无需 API Key；别高频刷。想深入跟进某个仓库的 issue/PR，走 GitHub 官方连接器（另一条线）。
