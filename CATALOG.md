# 总目录：技能、连接器与 API

> 这是整套系统的「设备清单」。另一个 Muse 接手时，照着本目录逐条对：**这是什么、怎么连、主人要去哪儿取 Key、接通后拿什么验证**。
> 范围：只收**自定义**的连接器、API 与技能；平台自带的是 Muse 标配，不列。
> 通用铁律：一切 Key/Token 只走安全凭据通道（保险库/安全录入卡），目录里只写「去哪取」，永远不写值。接入流程见 [01 号模块](modules/01-connector-onboarding.md)。

图例：🔑 需主人取 Key · 📱 需主人扫码/在 App 内操作 · 🖱️ 一键授权 · 🆓 免 Key · 💻 需在主人电脑上装客户端

---

## A. 自定义连接器（逐条接入说明）

### A1. 生活服务（官方 MCP / 官方开放平台）

**米家（custom.mijia）💻📱**
- 用途：查智能家居设备与在线状态、经确认控制设备。
- 怎么连：小米无个人官方 API，走社区 `mijiaAPI` 客户端＋小米账号扫码登录；凭据存客户端本地配置（权限 600）。
- 主人要做：在装好客户端的机器上扫一次码（约 30 天有效，失效重扫）。
- 验证：列出家庭与设备清单，数量与主人家实际对得上。
- 注意：默认只读；控制共用设备先经确认；个别品类（如喂食器余量）设备本身不提供数据，不许编造。

**瑞幸咖啡（custom.luckin）🔑**
- 用途：查门店/商品、预览订单、经确认下单、查单。
- 怎么连：官方远程 MCP（Streamable HTTP，Bearer Token）。
- 主人取 Token：打开 `open.lkcoffee.com/mcp`，右上角手机号＋短信验证码登录，复制页面上的 Token，经安全录入交给 Muse（约一个月有效）。
- 验证：用主人家坐标查附近门店，返回真实门店名与距离。
- 注意：预览只是报价；下单/取消先列明细等确认；付款由主人扫返回的二维码，Muse 不碰支付。

**麦当劳（custom.mcd）📱🔑**
- 用途：查门店餐品、领券查券、积分账户与商城、查单、经确认下单。
- 怎么连：官方 MCP（`mcp.mcd.cn`，Bearer Token，限流约 600 次/分）。
- 主人取 Token：打开 `open.mcd.cn` 扫码登录 → 控制台 → 激活 → 同意协议 → 复制 MCP Token，经安全录入交给 Muse。
- 验证：查「我的账户」，返回真实积分。
- 注意：计价只是报价；下单、取消、积分兑换/抽奖（花积分）、新增地址等写操作一律先确认。

**飞猪 Fliggy（custom.fliggy）🔑**
- 用途：搜机票/酒店/火车票/景点/万豪套餐（仅搜索，下单跳转飞猪完成）。
- 怎么连：官方 CLI（`@fly-ai/flyai-cli`）。无 Key 模式能搜但价格打码；正式 Key 模式出真实价格。
- 主人取 Key：打开 `flyai.open.fliggy.com` 控制台领取 API Key。
- ⚠️ 已知坑：飞猪要求每个请求用 Key 本身作 HMAC-SHA256 密钥签名（`x-ff-ctx` 头），凭据替身机制算不出签名——须经主人明确批准后，把 Key 直写 CLI 自己的配置文件（目录 700/文件 600），撤销办法是主人在控制台重置 Key。验活只看「价格是否打码」，永远不读出 Key 本身。
- 验证：同一查询，无 Key 时价格是区间打码、配 Key 后是真实数字。

**途牛（custom.legacy-tuniu）🔑**
- 用途：酒店/机票/火车票/门票查询（只查不订），与飞猪互为替补：飞猪主查、途牛核价与故障顶上；火车票权威数据另走 12306 技能。
- 怎么连：官方 MCP，凭据经保险库替身注入，本连接器强制只读白名单。
- 验证：任意城市酒店搜索返回真实列表与「¥起」价。

**滴滴出行（custom.didi-mcp）🔑**
- 用途：打车估价/下单/订单跟踪、司机位置、POI 搜索、路线规划。
- 怎么连：滴滴官方 MCP Server，按其开放平台指引获取凭据存保险库。
- 验证：一次估价查询。注意：下单先列明细等确认。

**淘宝（未接通，官方功能下线中）💻**
- 用途（设计中）：桌面客户端内置 MCP，可搜商品、加购、下单到付款前（付款主人本人）。
- 现状：官方已于近期以「技术维护」下线 MCP 配置功能、无恢复日期；已设每周巡检盯官方指南页，通知消失再重启施工。新用户现阶段开不了，不要硬装。
- 施工图（恢复后用）：主人电脑装淘宝桌面版 v2.5+ 登录 → 设置→AI 设置→开启 MCP（旺旺代理授权单独控制，默认不开）→ 服务只监听本机 `localhost:3654/mcp` → 须靠 Tailscale 或本机助理搭桥，Muse 才能远程调用。

### A2. AI 外援与搜索

**OpenAI（custom.openai）🔑**
- 用途：批量初筛、翻译、转录、第二意见、大块头重活的外援模型池。
- 主人取 Key：OpenAI 平台创建 API Key，经安全录入存保险库。
- 用法纪律：批量活默认便宜档、旗舰档只用在要紧判断；花费异常主动报；外援出量、Muse 终审。

**腾讯云 TokenHub（custom.tokenhub）🔑**
- 用途：国产模型池（OpenAI 兼容端点），作 OpenAI 的备用外援。
- 主人取 Key：腾讯云控制台开通 TokenHub 后创建 API Key 存保险库。

**DeepSeek Search（custom.deepseek-search）🔑**
- 用途：AI 原生网页搜索（带权威度与重排分）＋单页抓取转 Markdown，是「发现」环节的主力。
- 主人取 Key：DeepSeek 开放平台注册。按成功请求次数计费，失败不计费。

**Typesafe / Jev（custom.typesafe）🔑**
- 用途：决策打分（分类/紧急度/是否需主人拍板），用于巡检的影子分级对照。
- 纪律：只做影子或「高置信忽略归档」一条窄通道；安全类永远人工核实，是否打扰主人永不由打分单独决定。

### A3. 内容、数据与查询类 API

| 连接器 | 用途 | 怎么连 / 主人取 Key | 验证 |
|---|---|---|---|
| Steam（custom.steam） | 游戏搜索、新闻、在线人数、国区价格、愿望单监控 | 🔑 `steamcommunity.com/dev/apikey` 注册免费 Key | 搜一款游戏返回真实条目 |
| 知乎（custom.zhihu） | 知乎开放平台查询 | 🔑 按开放平台指引注册凭据 | 一次内容查询 |
| 天眼查（custom.tianyancha） | 企业检索、工商、风险、司法、股权 | 💻 经主人自备云服务器上的官方 CLI＋OAuth 登录 | 搜一家公司返回主体信息 |
| Bilibili（custom.bilibili） | 热门/排行/搜索/视频详情/热评 | 🆓 公开 Web API 匿名只读，不用账号 Cookie | 拉一次热门榜 |
| DailyHot（custom.dailyhot） | 40+ 平台热榜聚合（微博/知乎/B站/抖音等） | 🆓 自托管开源服务（imsyy/DailyHotApi），本地部署无 Key | 拉一次微博热搜 |
| Hugging Face（custom.huggingface） | 热门/搜索模型与数据集 | 🆓 公开读免 Key | 拉一次趋势模型 |
| RAWG（custom.rawg） | 跨平台游戏库搜索、详情、评分 | 🔑 rawg.io 注册免费 Key | 搜一款游戏 |
| OMDb（custom.omdb） | 影视查分（IMDb/烂番茄）、票房、卡司 | 🔑 omdbapi.com 注册免费 Key | 查一部电影 |
| football-data（custom.football-data） | 联赛积分、射手、赛程比分 | 🔑 football-data.org 注册免费 Key（免费层约 10 次/分） | 拉一次积分榜 |
| NASA（custom.nasa） | 每日天文图、近地天体 | 🔑 api.nasa.gov 免费注册 Key | 拉一次当日天文图 |
| Frankfurter（custom.frankfurter） | 汇率（ECB 参考价）与换算 | 🆓 免 Key | 查一次 USD→CNY |
| 快递100（custom.kuaidi100） | 快递轨迹（自动识别承运商）、运费/时效估算 | 🆓 官方免费额度免 Key（可选个人 Key 提额） | 查一个真实单号 |
| 高德地图（custom.legacy-amap） | 地理编码、POI 搜索、驾车路线 | 🔑 高德开放平台注册 Web 服务 Key | 一次 POI 搜索 |
| 和风天气（custom.legacy-qweather） | 实时天气、逐日/逐小时预报 | 🔑 和风天气控制台注册 Key | 查一次实时天气 |
| 今日诗词（custom.jinrishici） | 随机古典诗词一句 | 🆓 免 Key | 拉一句 |
| Cat Facts / TheCatAPI / OpenTDB | 猫知识、猫图、 trivia 题库（闲聊调味） | 🆓 免 Key 为主（TheCatAPI 品种端点需免费 Key） | 各拉一次 |

### A4. 主人的个人数据类（经 App 内专属凭据）

**腾讯会议（custom.tencent-meeting）🔑**
- 用途：预约/改/取消会议、查参会人、拉录制与逐字稿、AI 纪要。
- 主人取 Token：在腾讯会议开放平台以个人账号获取 Token 存保险库。注意：云录制与转写需主人主持并开启录制才有素材。

**腾讯文档（custom.legacy-tencent-docs）** — 官方 MCP，默认只读（搜文档、看目录、读内容），不开创建/删除/改权限。

**微信读书（custom.legacy-weread）** — 官方 Agent 网关只读：书架、阅读统计、划线笔记、推荐。

**训记 Xunji（custom.xunji）🔑**
- 用途：训练记录、官方计划、身体与饮食数据的读写（健身数据的唯一事实源）。
- 主人取凭据：在训记 App 内按其 LLM Open API 指引获取。纪律：写入前经主人点头，写完回读验证，只写一次。

**Trakt（custom.trakt）🔑**
- 用途：观影历史与评分的审计同步。
- 怎么连：PKCE 应用模式（只需 client_id，设备码式手动授权），主人 在 Trakt 注册 API 应用并完成一次授权。

**Hemory（custom.hemory）🔑📱**
- 用途：搜索全天监听沉淀的对话记忆与说话人库。
- 主人取密钥：App 内「Agent」设置里为 Muse 槽位生成专属密钥，经安全录入存保险库。注意其官方有多个域名入口，要认准 App 内给出的那个 MCP 地址。

**AideMemo（custom.aidememo）🔑📱**
- 用途：搜索本地优先「第二记忆」App 的笔记、语音转写、日历、提醒。
- 主人取密钥：App 内为 Muse 槽位生成专属密钥；权限给只读。每次查询会唤醒主人手机并弹通知，属正常现象。

**飞书（custom.feishu）** — 注册残留，未接通：OAuth 流程已建、凭据未到位即被主人取消，勿误认为可用。

---

### A5. 官方 MCP/Skill 增补（据公开清单回官方页核实，未在本机启用）

> 来源：2026-11-03 据一份公开整理清单（X 用户 @longhaiqwe123 的汇总帖）逐条回官方文档核实。本节条目均为「官方存在、接入路径属实」，但**未在本机实际接通**，启用前仍须按 01 号模块走一遍真实查询验证。同清单里的滴滴、淘宝、麦当劳、瑞幸、飞猪已在 A1 在用，不重复列。

**百度地图 MCP** 🆓🔑
- 用途：逆地理编码、地点检索、路线规划等 10 项核心能力，另有「马克地图」可把攻略导入百度地图 App。
- 怎么连：官方 MCP Server，远程 Streamable HTTP / SSE，或本地 stdio（uvx/npx）。
- 主人取 Key：百度地图开放平台注册开发者、创建应用获取 AK。
- 验证状态：官方文档核实，未在本机启用。

**腾讯地图 MCP** 🔑
- 用途：基于腾讯位置服务的地点、路线与行程规划类能力，返回做过语义化处理。
- 怎么连：官方云端 MCP，SSE / Streamable HTTP，无需本地部署。
- 主人取 Key：腾讯位置服务（lbs.qq.com）注册、创建 Key，并开通 WebServiceAPI 的接口权限与配额（MCP 依赖它构建）。
- 验证状态：官方文档核实，未在本机启用。

**高德地图 MCP（A3 现用 Web API 的官方 Agent 版）** 🔑
- 用途：位置服务、地点搜索、路径规划、天气等 12 项能力；2.0 还能把行程攻略一键生成专属地图导入高德 App。
- 怎么连：官方 MCP Server；与 A3 的高德 Web API 同属高德开放平台 Key 体系，是给 Agent 的原生路线。
- 验证状态：官方文档核实；本机现用 Web API 版，MCP 版未启用。

### A6. 官方开放生态增补（同一清单 2–7 部分，2026-11-03 逐条核实）


**办公和协作**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 飞书 | 官方 CLI（larksuite/cli，含 MCP 与 Skill）：在飞书开放平台建自建应用取凭据 | 🟢 |
| 金山文档 | 官方 Skill（kdocs-app/kdocs-skill）：WPS 个人账号授权 | 🟢 |
| 语雀 | 官方 MCP（yuque/yuque-mcp-server）：需语雀 Token，接口能力与会员等级相关 | 🟡 |
| QQ 邮箱 | 官方 Skill（Tencent/AgentlyMail）：QQ 邮箱账号授权 | 🟢 |
| 腾讯文档 / 腾讯会议 | 见 A4（已接通在用） | — |

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
| 微信读书 | 见 A4（另有官方 Skill 版，与网关版二选一） | — |

**搜索、内容和企业数据**

| 服务 | 接入方式 | 适接 |
|---|---|---|
| 博查搜索 | 官方 MCP（BochaAI/bocha-search-mcp）：注册 API Key，按量计费 | 🟡 |
| 企查查 | 官方 MCP（agent 平台）：企业数据，账号开通后按量/套餐 | 🟡 |
| 知乎 | 见 A3（官方另有 zhihu_cli 路线） | — |
| 天眼查 | 见 A3（官方 CLI 仓库即 A3 所用通道的上游） | — |

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

## B. 自定义技能（Skills）

技能是「做事的章法」，连接器是「数据的通道」。本节只收主人与 Muse 共同沉淀的自定义技能；Muse 产品自带的技能是标配，不在此列。

存放位置约定：`~/workspace/custom-skills/<name>/SKILL.md`。复刻方法：把对应 SKILL.md 拷进新环境的技能目录即可，涉及凭据的按 A 部分重新取。

| 技能 | 用途 | 来源/说明 |
|---|---|---|
| local_connector_onboarding | 新 API/MCP 接入的端到端验收法（文档→鉴权→工具清单→最小权限→真实冒烟），未验证不许声称有某能力 | 本机沉淀，配合 01 号模块用 |
| local-mijia / local-luckin / local-mcd / local-fliggy | A1 四个生活服务连接器的操作规矩（谁只读、谁先确认、替补分工） | 与连接器文档成对存在 |
| local-xunji | 训记数据读写规矩（唯一事实源、点头后只写一次、回读验证） | 配合 custom.xunji |
| tencent-meeting-mcp | 腾讯会议 MCP 的使用说明 | 配合 custom.tencent-meeting |
| tyc-it（天眼一下） | 天眼查查询的入口与口径 | 配合 custom.tianyancha |
| zhihu | 知乎开放平台调用说明 | 配合 custom.zhihu |
| local-railway-12306 | 查高铁/火车车次、余票、票价、经停与换乘（国内铁路权威口径，旅行搜索的火车票一律以它为准） | 自研，调公开数据 |
| local-steam | Steam 公开 API 的查询与愿望单降价监控口径 | 配合 custom.steam |
| local-thesportsdb | 免费体育数据：球队、赛程、比分 | 自研 |
| local-tmdb | TMDB 影视数据库：搜索、详情、本周热门 | 自研 |
| local-reddit | Reddit 子版块热榜与关键词搜帖（经免费存档 API） | 自研 |
| local-youtube | 抓 YouTube 字幕文本做摘要（经第三方 API，中英文轨道可选） | 自研 |
| local-agent-reach | 多平台搜索/读取 CLI（Agent Reach）的本机使用说明：只开免登录默认渠道（GitHub/YouTube/B站/V2EX/网页），增删渠道须主人批准 | 开源工具＋本机适配 |
| local_discovery_feeds | 从 arXiv、GitHub 趋势、HN、Product Hunt、订阅 RSS 抓新鲜内容去重，按主人兴趣过滤，供每日精选 | 本机沉淀（含 local-arxiv、local-github-trending、local-hn-ph、local-rss、local-game-deals 等供料子技能） |
| local_check_before_propose | 提方案前先核查现状的自检规矩（防重复提已否决方案） | 本机沉淀 |
| site_automation_upkeep | 站点自动化维护类杂务的操作留档 | 本机沉淀 |
| geo-sleuth | 照片地理定位：以 OSM 几何、地形天际线、卫星图、街景、太阳阴影互证，不靠地标猜（5k+ star 开源技能） | 开源，装前已做代码审计；注意：本类技能的脚本若忽略系统代理变量，需显式传代理环境变量；别人的私密照片跑反向搜图前先征得同意或关掉该步 |
| computer-repair-skill | 电脑维修 Playbook 库（Win/Mac/Linux 诊断、清理、性能、网络、安全）：先取证、只读优先、出计划、经确认再改 | 开源，装前已审计（几乎全 Markdown，唯一脚本只调本地命令） |

---

## 附：接不上也留档的三课

1. **有官方 MCP ≠ 个人能用**：先分清消费侧还是商户侧——支付类官方 MCP 多为商户收单方向，与个人消费侧用法相反。
2. **功能会下线**：官方页顶部的维护通知比任何教程都新；下线期最优解是低频巡检等恢复（见 A1 淘宝条）。
3. **鉴权机制有暗坑**：有的服务要求用 Key 本身签名请求，通用凭据替身做不到——先小规模验真再决定是否破例（见 A1 飞猪条）。
