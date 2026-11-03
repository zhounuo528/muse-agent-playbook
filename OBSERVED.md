# 未验证观察清单（OBSERVED）

> ⚠️ **本文件里的条目全部未实际接通验证。** 它们的「官方存在、接入路径属实」只经过公开文档核实，不构成任何可用能力。任何 Muse 不得把本清单条目当作已接通的装备引用或据此安排任务；启用任何一条之前，必须按 [01 号模块](modules/01-connector-onboarding.md) 走完「查已连 → 最省事授权 → 凭据入保险库 → 真实查询验证 → 落档」全流程，验证通过后才迁入 [CATALOG.md](CATALOG.md)。
> 采集与核实日期：2026-11-03。来源：一份公开整理清单（X 用户 @longhaiqwe123 的汇总帖）逐条回官方文档核实。状态类信息（如某功能下线、某接口调整）过期很快，引用前以官方页面为准。
> 适接评级说明：🟢 = 个人用户可走通的接入路径明确；🟡 = 存在会员、配额、审核或按量计费等附加条件，先看清再接。

---

## 一、官方 MCP/Skill 增补（地图类）

同批清单里的滴滴、淘宝、麦当劳、瑞幸、飞猪已在 [CATALOG.md](CATALOG.md) A1 以接入模式列出，不在此重复。

**百度地图 MCP** 🆓🔑
- 用途：逆地理编码、地点检索、路线规划等 10 项核心能力，另有「马克地图」可把攻略导入百度地图 App。
- 怎么连：官方 MCP Server，远程 Streamable HTTP / SSE，或本地 stdio（uvx/npx）。
- 用户取 Key：百度地图开放平台注册开发者、创建应用获取 AK。

**腾讯地图 MCP** 🔑
- 用途：基于腾讯位置服务的地点、路线与行程规划类能力，返回做过语义化处理。
- 怎么连：官方云端 MCP，SSE / Streamable HTTP，无需本地部署。
- 用户取 Key：腾讯位置服务（lbs.qq.com）注册、创建 Key，并开通 WebServiceAPI 的接口权限与配额（MCP 依赖它构建）。

**高德地图 MCP（CATALOG A3 现用 Web API 的官方 Agent 版）** 🔑
- 用途：位置服务、地点搜索、路径规划、天气等 12 项能力；2.0 还能把行程攻略一键生成专属地图导入高德 App。
- 怎么连：官方 MCP Server；与 A3 的高德 Web API 同属高德开放平台 Key 体系，是给 Agent 的原生路线。

## 二、官方开放生态增补（同一清单其余部分）

**办公和协作**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 飞书 | 官方 CLI（larksuite/cli，含 MCP 与 Skill）：在飞书开放平台建自建应用取凭据 | 🟢 |
| 金山文档 | 官方 Skill（kdocs-app/kdocs-skill）：WPS 个人账号授权 | 🟢 |
| 语雀 | 官方 MCP（yuque/yuque-mcp-server）：需语雀 Token，接口能力与会员等级相关 | 🟡 |
| QQ 邮箱 | 官方 Skill（Tencent/AgentlyMail）：QQ 邮箱账号授权 | 🟢 |
| 腾讯文档 / 腾讯会议 | 见 CATALOG 的 A4 范式（办公协作类） | — |

**笔记、阅读和网盘**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 腾讯 ima | 官方 Skill（ima 的 agent-interface）：腾讯账号授权 | 🟢 |
| Get 笔记 | 官方 MCP / Skill（得到出品）：账号授权 | 🟢 |
| flomo | 官方 MCP：在 flomo 设置中获取接口令牌 | 🟡 |
| 印象笔记 | 官方 Skill：经技能市场（skillhub.cn）分发，需配置 API Key 并完成账号授权 | 🟢 |
| 有道云笔记 | 官方 MCP / Skill：账号授权 | 🟢 |
| 夸克网盘 | 官方 Skill：夸克账号授权 | 🟢 |
| 腾讯微云 | 官方 Skill：腾讯账号授权 | 🟢 |
| 百度网盘 | 官方 MCP（百度网盘开放平台）：建应用＋OAuth 授权，部分能力有审核与配额 | 🟡 |
| 微信读书 | 见 CATALOG 的 A4 范式（办公协作类；官方另有 Skill 版，与网关版二选一） | — |

**搜索、内容和企业数据**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 博查搜索 | 官方 MCP（BochaAI/bocha-search-mcp）：注册 API Key，按量计费 | 🟡 |
| 企查查 | 官方 MCP（agent 平台）：企业数据，账号开通后按量/套餐 | 🟡 |
| 知乎 | 见 CATALOG A3（官方另有 zhihu_cli 路线） | — |
| 天眼查 | 见 CATALOG A3（官方 CLI 仓库即 A3 所用通道的上游） | — |

**设计和创作**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| MasterGo | 官方 Magic MCP：MasterGo 账号，设计稿数据与图层操作 | 🟢 |
| 墨刀 | 官方 MCP：墨刀账号 | 🟢 |
| Pixso | 官方远程 MCP：Pixso 账号 | 🟢 |
| 可灵 Kling | 官方 Skills（klingai-tech/skills）：视频生成，API 按量付费 | 🟡 |
| MiniMax | 官方 MCP（MiniMax-AI/MiniMax-MCP）：注册 API Key，按量付费 | 🟡 |
| 网易云音乐 | 官方 Skill（ncmcli）：账号授权 | 🟢 |

**支付和金融**（支付类官方 MCP/Skill 经核实均为商户/机构侧，无个人通道，按口径不列；下列为个人可用的金融数据类）

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 同花顺 iFinD | 官方 MCP / Skill：需 iFinD 专业数据终端账号授权 | 🟡 |
| 同花顺问财 | 官方 Skill（问财 SkillHub）：账号授权，部分能力与会员相关 | 🟡 |
| 东方财富妙想 | 官方 Skill 集：东财账号授权 | 🟢 |

**云和开发**（都是管自家云资源的，Key 即权限：只给最小权限、优先用子账号）

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 阿里云 | 官方 MCP 广场/服务：云账号＋AccessKey | 🟢 |
| 腾讯云 | 官方 Skill / MCP：云账号凭据 | 🟢 |
| 华为云 | 官方 MCP（huaweicloud/huaweicloud-mcp）：云账号凭据 | 🟢 |
| 火山引擎 | 官方 MCP 集（volcengine/mcp-server）：云账号凭据 | 🟢 |
| 百度智能云 | 官方 MCP 文档：云账号凭据 | 🟢 |
| 七牛云 | 官方 MCP（qiniu/qiniu-mcp-server）：AccessKey/SecretKey | 🟢 |
| 智谱 | 官方 MCP（搜索等服务）：BigModel API Key | 🟡 |
| Gitee | 官方 MCP Server：账号令牌 | 🟢 |
| 极狐 GitLab | 官方 MCP：账号令牌 | 🟢 |
