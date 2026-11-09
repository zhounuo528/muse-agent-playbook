# Agent Reach 使用笔记【配方：工具从上游安装，本文件只是适配笔记】

## 这是什么
Agent-Reach 是一个开源的多平台搜索/读取 CLI（上游：Panniantong/Agent-Reach），把多个内容平台的搜索与读取聚合成一个命令（`agent-reach <渠道> <动作>`），适合给已有搜索栈补缺口。本仓**不分发该工具本体**，请按上游 README 安装；本文件只记录「在一个个人助理环境里怎么用它」的口径。

## 安装后先做的事
1. 按上游说明安装（uv tool 形态时注意：工具目录可能不在默认 PATH 里，调用前先把它加进 PATH）。
2. `agent-reach version` 与 `agent-reach channels` 各跑一次，确认版本与渠道清单——**渠道清单以你本机实测为准，不要照任何二手文档声称能力**。

## 常用命令形态（以你本机 `channels` 输出为准）
```bash
agent-reach github search <query> [--limit N]
agent-reach github read <owner/repo | issue/PR 链接>
agent-reach v2ex hot [--limit N]
agent-reach v2ex read <帖子链接>
agent-reach web search <query> [--limit N]
agent-reach web read <url>
agent-reach read <url>          # 按链接自动选渠道
```
每条命令输出一个 JSON 对象，看 `ok` / `items` / `error.hint`。失败的后端通常有约一两分钟冷却，别循环重试。

## 使用口径（重点）
1. **定位是补缺口，不是替代主力**：网页搜索、代码托管、视频字幕等已有专长工具的场景，优先用专长工具；Reach 优先用在现有栈没有的渠道（如 V2EX 热榜、语义网页搜索）。
2. **只开免登录的默认渠道**。任何需要登录态的渠道（社交平台类）未经用户明确批准不要启用，更不许碰用户的浏览器登录态——加渠道属于改配置，先报用户批准。
3. **只读**：不发帖、不评论、不点赞。
4. **渠道可用性因环境而异**：同一渠道在不同网络出口下可能一个可用一个不可用（TLS、反爬、区域限制都会影响），实测不可用就如实记录原因，不要为了「修好它」去建议用户登录或换凭据。
5. 升级用上游提供的 update 方式；卸载前先列清影响范围给用户确认。
