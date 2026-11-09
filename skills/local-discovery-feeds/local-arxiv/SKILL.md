# ArXiv AI 论文日报

## 用途
抓取 ArXiv 上 AI 相关分类的最新论文，给每日精选、AI 进展追踪供料。

## 工具
```bash
bin/arxiv                    # 默认：最近 1 天 cs.AI+cs.LG+cs.CL，最多 20 篇/类
bin/arxiv --days 3           # 最近 3 天
bin/arxiv --category cs.CL   # 只看计算语言学
bin/arxiv --max 10           # 每类最多 10 篇
```
输出 JSON：papers[] 含 title/authors/summary/url/published/categories，errors[] 列出抓取失败的分类。

## 使用规则
1. 输出给用户前，对照你自己的已推送日志去重（见聚合技能 local-discovery-feeds 的口径）。
2. 挑论文时优先：与用户关注方向相关的、有 demo 或代码的、标题有意思的。
3. summary 是英文摘要，展示时用中文一句话概括，别直接贴英文。
4. ArXiv 官方 API 免费免 Key；别高频刷，一天查 1–2 次足够。

## 已知限制
- 只按提交日期排序取最新，不做引用量/热度排序。
- 摘要截断到 600 字符，太长的需要点链接看全文。
- 分类过滤靠 ArXiv 自带的 cat 标签，偶有串类。
