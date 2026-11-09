# VERIFICATION · 验收回执（脱敏）

> 按 01 号模块的最小回执格式留存：日期／命令／返回要点／退出码。只记公开查询与边界用例，不含任何账号数据与凭据。环境：作者本机（Linux VM，经出口代理），2026-11-09。

## 2.1 批次

| 命令 | 返回要点 | 退出码 |
|---|---|---|
| `bin/steam --players 730` | CS2 实时在线 963,042 | 0 |
| `bin/steam --app 570` | Dota 2，价格表述为「免费」（is_free 分支） | 0 |
| `bin/hn-ph --hn --count 3` | 返回真实 HN 头条（含标题/分数/评论数） | 0 |
| `bin/arxiv --category cs.CL --max 2 --days 7` | 返回真实论文（CodeQuant2 等） | 0 |
| `bin/gh-trending` | 返回真实 trending 仓库（anthropics/skills 等） | 0 |
| `bin/reddit --hot python --limit 2` | 返回真实帖子（按时间倒序） | 0 |
| `bin/wxread "https://example.com/x"` | BAD_URL：只支持 mp.weixin.qq.com | 2 |
| `bin/wxread "file:///etc/passwd"` | BAD_URL：只支持 http/https | 2 |
| `bin/rss --add "file:///etc/passwd"` | 拒绝添加：只许公网 http/https | 0（拒绝文案即结果） |
| `bin/rss --add "http://192.168.1.1/feed"` | 拒绝添加：只许公网 http/https | 0（同上） |
| `bin/yt "not-a-video"` | 11 位假 ID 走满全部 Piped 实例后 fetch_failed | 1 |
| `bin/query-12306 evil-tool '{}'` | Refused: tool not in allowlist | 2 |

## 2.1.1 批次

| 命令 | 返回要点 | 退出码 |
|---|---|---|
| `bin/sportsdb --team Liverpool` | 返回 Liverpool／英超／Anfield，JSON 完整无截断 | 0 |
| rss `hop_ok("https://news.ycombinator.com/rss")` | True（经 fake-IP DNS 的公网域名不误杀） | — |
| rss `hop_ok("http://192.168.1.1/feed")` / `hop_ok("http://localhost/feed")` | False / False | — |
| rss 受控抓取 HN 源（fetch_all） | 3 条真实条目、errors 为空 | 0 |
| yt `safe_remote_url("https://www.google.com/")` | True | — |
| yt `safe_remote_url("http://192.168.0.9/x")` / `("http://localhost:8080/x")` | False / False | — |
| `bin/wxread "https://mp.weixin.qq.com/"` | 手动重定向链路走通，页面无 js_content 如实报 FETCH_FAILED | 1 |
| `bin/wxread "https://example.com/x"` | BAD_URL（同 2.1） | 2 |

## 已知边界（如实标注）

- URL 校验挡「字面内网＋解析直指内网＋重定向跳内网」；解析与连接之间换答案的 DNS 重绑定不在 CLI 这一层防，需出站代理/防火墙兜底。
- rss 配置的读-改-写未做跨进程串行化：两个进程同时 `--add` 仍可能后写覆盖先写（临时文件已唯一化，不会写坏文件本身）。
- 本文件是作者环境的回执；采用方请按 ADOPT 的三档口径在自己环境重跑冒烟，不要把这里的「运行已验」搬到自己头上。
