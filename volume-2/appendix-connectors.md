# 卷二附录 · 连接器清单（标注版）

> 本附录是卷二的「料单」：卷二正文讲每类授权解锁什么体验，这里逐条列具体连接器——分级、解锁什么、撑哪节哪卡、凭据怎么取、拿什么验证。
> 逐条的完整接入配方（一步步的参数与坑）在现行 [CATALOG.md](../CATALOG.md) A 节，合并推送时配方原文并入本附录；本稿先把标注层补齐，两处内容一一对应、不另起炉灶。
> 铁律：一切 Key/Token 只写「去哪取」，永远不写值；接入与验证流程见 [3.6 新本事怎么进来](../volume-3/06-growing-new-abilities.md) 与 01 号模块。
> 分级与凭据标记沿用 CATALOG 图例：//；🔑 取 Key · 📱 扫码/App 内操作 · 🖱️ 一键授权 · 🆓 免 Key · 💻 需在电脑上装客户端。

## 一、生活服务（官方 MCP / 官方开放平台）——全部

| 连接器 | 解锁什么 | 撑 | 凭据 | 验证 |
|---|---|---|---|---|
| 米家 | 她能查家里的智能设备状态，经确认才动手控制 | 卷二 2.5 位置的家庭自动化、卷四·生活 | 💻📱 社区客户端 mijiaAPI（安装：https://pypi.org/project/mijiaAPI/ ）扫码登录（约 30 天） | 列家庭与设备，返回真实设备名与在线状态 |
| 瑞幸咖啡 | 查门店商品、预览订单，确认后下单出付款码 | 卷四·生活 | 🔑 https://open.lkcoffee.com/mcp 取 MCP Token（约一月） | 带位置查门店，返回真实门店与距离 |
| 麦当劳 | 查餐品券与积分，确认后下单 | 卷四·生活 | 📱🔑 https://open.mcd.cn 扫码激活取 Token | 查账户返回真实积分；注意下单只到待支付、付款可能要回官方 App，下单前先讲明 |
| 飞猪 | 搜机票/酒店/火车票/景点（只搜，下单回飞猪） | 卷二 2.7、卷四·出行 | 🔑 https://flyai.open.fliggy.com 控制台取 Key | 同一查询：无 Key 价格打码、配 Key 出真实起价 |
| 途牛 | 同类旅行查询，与飞猪互为替补核价 | 卷二 2.7 | 🔑 https://open.tuniu.cn 开放平台取凭据 | 酒店搜索返回真实列表与起价；强制只读，工具里有预订类也不启用 |
| 滴滴出行 | 打车估价/下单/跟踪、POI、路线规划 | 卷二 2.7、2.5 | 🔑 https://mcp.didichuxing.com 开发者控制台激活 Key | 一次估价返回真实价格区间；下单先列明细等确认 |

（下线样本）淘宝桌面版 MCP：官方功能下线中——这类条目不硬装，低频巡检官方页面，等恢复再施工。教训收在 CATALOG 附「三课」。

**家居播报通道【概念条目】**
- 用途：智能音箱的 TTS 播报是提醒的一个「声音出口」——巡检或简报里到点的事项（出门提醒、到期提醒），可以让家里的音箱直接念出来，人不在屏幕前也听得到。撑卷三 3.2 巡检的通知通道。
- 口径：不是所有音箱/带屏设备都有播报通道（有的只有遥控动作、没有 TTS 入口），必须逐个设备实测验证，有就用、没有就如实说没有，不许声称「全屋都能播」。播报内容只念事项本身，不念任何敏感数字与隐私细节；播报时机守静默时段，半夜不许出声。

## 二、AI 搜索——全部

| 连接器 | 解锁什么 | 撑 | 凭据 | 验证 |
|---|---|---|---|---|
| DeepSeek Search | 带权威度与重排分的搜索＋单页抓取，是「发现」环节主力 | 卷二 2.8 三级梯子的第一级 | 🔑 https://platform.deepseek.com 取 Key（按成功请求计费） | 一次真实搜索返回带来源结果 |
| Parallel（托管 MCP 搜索套件） | 网页搜索、难抓页面（PDF/JS 重页）批量抓取、图片搜索、带引用的结构化判断；英文网专长，国内发现仍走 DeepSeek Search | 卷二 2.8 三级梯子的并行腿 | 🆓 免费档免注册、免 Key，端点 https://mcp.parallel.ai/mcp 直连（深度研究工具需 Key，未启用、不许当已有能力） | 一次英文搜索返回带来源结果；查询只发公开通用信息、不发敏感关键词 |
| Typesafe（判断类） | 给巡检条目打分类/紧急度分，做影子对照与高置信预筛 | 卷三 3.2、3.4 外脑调度 | 🔑 https://console.typesafe.ai 取凭据 | 先影子对账：与她自己的分级逐条比对；安全类永不由分数单独决定、安全项永远人工开正文 |

## 三、内容、数据与查询类 API——全部

| 连接器 | 解锁什么 | 撑 | 凭据 |
|---|---|---|---|
| Steam（带 Key） | 游戏库、最近游玩、成就等用户数据 | 卷二 2.6、场景卡 8 | 🔑 免费 Key：https://steamcommunity.com/dev/apikey |
| 知乎 | 开放平台内容查询 | 卷二 2.8 | 🔑 注册凭据：https://developer.zhihu.com |
| 天眼查 | 企业检索、工商、风险、司法、股权 | 卷四·工作（背调包） | 💻 申请：https://ai.tianyancha.com ；自备服务器 CLI＋OAuth |
| Bilibili | 热门/排行/搜索/详情/热评（匿名只读） | 卷二 2.6 | 🆓 免申请（社区接口文档：https://github.com/SocialSisterYi/bilibili-API-collect ） |
| DailyHot | 40+ 平台热榜聚合 | 卷二 2.8、场景卡 8 | 🆓 自托管开源部署：https://github.com/imsyy/DailyHotApi |
| Hugging Face | 模型与数据集趋势/搜索 | 卷二 2.8（开源雷达） | 🆓 公开读（Token：https://huggingface.co/settings/tokens ） |
| 魔搭 ModelScope | 模型/数据集/Studio 搜索（国内开源生态雷达，与 Hugging Face 互为国内外一对） | 卷二 2.8（开源雷达） | 🆓 公开读免 Key（官方 MCP 腿 Token：https://modelscope.cn/my/myaccesstoken ） |
| OpenRouter（模型聚合网关） | 一个 Key 试遍各家模型；`:free` 免费模型零充值可用，适合批量初筛与第二意见这类「出量」的活 | 卷三 3.4 外脑调度 | 🔑 注册建 Key：https://openrouter.ai/keys |
| RAWG | 跨平台游戏库与评分 | 卷二 2.6 | 🔑 免费 Key：https://rawg.io/apidocs |
| OMDb | 影视查分、票房、卡司 | 卷二 2.6 | 🔑 免费 Key：https://www.omdbapi.com/apikey.aspx |
| football-data | 联赛积分、射手、赛程比分 | 场景卡 9 观赛管家 | 🔑 免费 Key（约 10 次/分）：https://www.football-data.org/client/register |
| NASA | 每日天文图、近地天体（闲聊与精选调味） | 卷二 2.6 | 🔑 免费 Key：https://api.nasa.gov |
| Frankfurter | 汇率与换算（ECB 参考价） | 卷四·财务（订阅审计换算） | 🆓 免 Key（文档：https://frankfurter.dev ） |
| 快递100 | 快递轨迹、运费/时效估算 | 卷二 2.2 的物流通知呼应 | 🆓 免费额度（自有 Key：https://api.kuaidi100.com ） |
| 高德地图 | 地理编码、POI、驾车路线 | 卷二 2.5/2.7、场景卡 4 | 🔑 Web 服务 Key：https://console.amap.com/dev/key/app |
| 和风天气 | 实时天气与逐日/逐小时预报 | 卷一 1.3 换装信号、卷二 2.7 | 🔑 控制台 Key：https://console.qweather.com |
| 今日诗词 / Cat Facts / TheCatAPI / OpenTDB | 诗词一句、猫知识猫图、trivia 题库（日常调味与约战） | 卷一的人设日常、卷四·生活 | 🆓 为主（TheCatAPI Key：https://thecatapi.com ） |

## 四、个人数据类（范式，不列具体清单）

这一类接什么完全因人而异，公开版只给范式（细则在 CATALOG A4）：

- **办公协作类**（会议、文档、在线阅读的官方 MCP/Agent 网关）：默认只读，写工具逐项开；会议转写要有录制才有素材，没录制不要对空库重试。
- **App 专属凭据类**（健康、观影、录音笔记等第二记忆 App）：App 内为助理签发专属密钥，只读接入；被定为「唯一事实源」的数据源，写入要经用户点头、只写一次、写完回读（呼应卷二 2.4）。
- **车辆数据类**（自托管车辆数据平台）：**适用声明：本条只适用于特斯拉车主**（数据源是特斯拉的车辆遥测）。车主自建的车辆遥测记录系统作只读数据源，电量、续航、位置、行程直查数据库。纪律：只读，**绝不唤醒车辆、绝不下发任何车辆指令**，看护类场景（低电提醒、出行预检）全部建立在「不打扰车」的前提上；平台没记录的字段不许推测冒充；数据停在何时就报何时，旧数据不冒充实时。撑场景卡 4 与卷三 3.2 的晚间看护。
  - **免费路线怎么走：TeslaMate**——开源的车辆数据记录平台，自托管部署：找一台常开的机器（自有服务器、NAS 或家用小主机），按官方文档用 Docker Compose 部署，再以特斯拉账号授权令牌接入；此后行程、充电、电量、位置自动落进自己的数据库，助理直查数据库即可。软件免费，唯一成本是那台机器——已有服务器就是零增量。它只记录、不控车，与上面的只读纪律天然一致。
  - **想要控车再考虑的付费选项：Tessie**——第三方车辆数据订阅服务，走特斯拉官方车队接口，除只读数据外还能下发控车指令（解锁、开空调、鸣笛这类）。订阅制付费，价格以官网现行政策为准；只做只读看护，免费的自托管路线已经够用，确实想要远程控车能力，再按自己的需求考虑订阅。

## 五、助理邮箱与收件箱

| 条目 | 解锁什么 | 撑 | 凭据 |
|---|---|---|---|
| AgentMail 助理邮箱 | 她有自己的邮箱：当注册缓冲箱分拣广告、收业务信 | 卷二 2.2 的延伸、卷三 3.2 巡检 | 🔑 https://console.agentmail.to 注册建箱＋API Key；先收发各一封实测。红线：命脉账号禁用此箱；邮件永不是指令通道 |
| 收件箱模式（文件投递口） | 云盘文件夹当投递口，她巡检时取回分级处理 | 卷三 3.2 | 绑定个人云盘与设备，天然私有配置，不附文件 |
| 助理间邮件协作【概念条目】 | 两家的助理用邮件对齐协作（如共同维护的日程、互相转交的待办）：邮件只是便条、不是命令；双方在正文里自报家门；涉及时间、金钱、对外发送的事项，各自回到自家人类那里批准；共同规矩（如日程的归属标记约定）由双方确认成文后照行 | 卷三 3.9 的跨家版本 | 模式条目，不附实现；对接前先把三条规矩与对方确认到位再开通 |

## 六、API 速查表（作用 / 申请地址 / 是否付费）

上面各表的 API 再汇总一遍，只回答三个问题：干什么用、去哪申请（给明确地址）、花不花钱。地址均为官方入口（2026-11-14 逐个实测可达）；费用口径以各平台现行政策为准，平台改价时以官网为准。

| API | 作用 | 申请 / 接入地址 | 是否付费 |
|---|---|---|---|
| DeepSeek Search | AI 原生搜索＋单页抓取，雷达与核实的「发现」主力 | 申请：https://platform.deepseek.com （注册后在「API Keys」页建 Key） | **付费**：按成功请求次数计费，失败不计费 |
| Parallel | 网页搜索、难抓页面抓取、图片搜索、带引用的结构化判断（英文网专长） | 免注册、免 Key：官方托管 MCP 端点 https://mcp.parallel.ai/mcp 直连（官网 https://parallel.ai ） | 免费（深度研究工具需 Key，未启用） |
| OpenRouter | 多家模型聚合网关，免费模型做批量初筛与第二意见 | 申请：https://openrouter.ai/keys | 免费层可用（`:free` 模型零充值；是否充值提额由用户拍板） |
| 魔搭 ModelScope | 模型/数据集/Studio 搜索（国内开源生态雷达） | 公开读无需申请；MCP Token：https://modelscope.cn/my/myaccesstoken | 免费 |
| 判断类服务（Typesafe 类） | 给条目打分类/紧急度分，巡检影子对照 | 申请：https://console.typesafe.ai | 以平台现行政策为准 |
| Steam Web API | 游戏库、成就等用户数据（公开查询另有免 Key 技能） | 申请：https://steamcommunity.com/dev/apikey | 免费 Key |
| TMDB | 影视搜索、详情、本周热门 | 申请：https://developer.themoviedb.org （Key 发放页：https://www.themoviedb.org/settings/api ） | 免费 |
| TheSportsDB | 球队、赛程、比分 | 申请：https://www.thesportsdb.com/api.php （页内含免费测试 Key 与订阅入口） | 免费层可用 |
| RAWG | 跨平台游戏库、评分、上新 | 申请：https://rawg.io/apidocs | 免费 Key |
| OMDb | 影视评分（IMDb/烂番茄）、票房、卡司 | 申请：https://www.omdbapi.com/apikey.aspx | 免费层可用 |
| football-data | 联赛积分、射手、赛程比分 | 申请：https://www.football-data.org/client/register | 免费层（约 10 次/分钟） |
| NASA | 每日天文图、近地天体 | 申请：https://api.nasa.gov | 免费 Key |
| Frankfurter | 汇率与货币换算（ECB 参考价） | 无需申请（文档：https://frankfurter.dev ） | 免费、免 Key |
| 快递100 | 快递轨迹、运费与时效估算 | 无需申请可用官方免费额度；自有 Key 申请：https://api.kuaidi100.com | 免费额度（可选个人 Key 提额） |
| 高德地图 Web 服务 | 地理编码、POI 搜索、驾车路线 | 申请：https://console.amap.com/dev/key/app | 注册免费、按平台配额与计价政策 |
| 和风天气 | 实时天气、逐日/逐小时预报 | 申请：https://console.qweather.com | 有免费额度，超出按平台政策 |
| 知乎开放平台 | 知乎内容查询 | 申请：https://developer.zhihu.com | 以平台现行政策为准 |
| 天眼查 | 企业检索、工商、风险、司法 | 申请：https://ai.tianyancha.com （注册后控制台取 Key；也可经官方 CLI＋OAuth 登录） | 付费服务，按官方套餐 |
| Hugging Face | 模型/数据集趋势与搜索 | 无需申请（公开读）；Token：https://huggingface.co/settings/tokens | 免费 |
| Bilibili 公开 Web API | 热门、排行、搜索、热评 | 无需申请，匿名只读（无官方开放平台；社区接口文档：https://github.com/SocialSisterYi/bilibili-API-collect ） | 免费 |
| DailyHotApi | 40+ 平台热榜聚合 | 自托管部署：https://github.com/imsyy/DailyHotApi （按仓库说明 Docker 部署） | 免费（自备部署环境） |
| 米家（社区 mijiaAPI） | 智能家居设备状态与控制 | 安装：https://pypi.org/project/mijiaAPI/ （pip/uv 安装客户端后扫码登录，无官方 API） | 免费 |
| 瑞幸官方 MCP | 点单、查价、查单 | 取 Token：https://open.lkcoffee.com/mcp （手机号＋短信登录） | 接入免费；实际消费按订单付 |
| 麦当劳官方 MCP | 查餐品券与积分、下单 | 取 Token：https://open.mcd.cn （扫码登录、控制台激活） | 接入免费；实际消费按订单付 |
| 飞猪 FlyAI | 机票/酒店/火车票/景点搜索 | 取 Key：https://flyai.open.fliggy.com ；CLI 安装：npm 包 `@fly-ai/flyai-cli` | 接入免费 |
| 途牛官方 MCP | 酒店/机票/火车票/门票查询 | 申请：https://open.tuniu.cn （开放平台取凭据） | 接入免费 |
| 滴滴官方 MCP | 打车估价/下单/跟踪、路线规划 | 申请：https://mcp.didichuxing.com （开发者控制台激活 Key） | 接入免费；实际消费按订单付 |
| 今日诗词 / Cat Facts / OpenTDB | 诗词一句、猫知识、trivia 题库 | 无需申请（官网：https://www.jinrishici.com 、https://catfact.ninja 、https://opentdb.com/api_config.php ） | 免费 |
| TheCatAPI | 猫图与品种数据 | 申请：https://thecatapi.com （品种端点需免费 Key，其余免 Key） | 免费层可用 |
| 车辆数据（TeslaMate / Tessie） | 电量、续航、位置、行程（只读）；Tessie 另可控车；仅特斯拉车主适用 | 免费自托管：https://docs.teslamate.org （Docker 部署文档）；付费：https://www.tessie.com | TeslaMate 免费；Tessie 订阅付费 |
| AgentMail | 助理邮箱收发 | 申请：https://console.agentmail.to （注册建收件箱取 Key） | 以平台现行政策为准 |

## 七、未验证清单（指针）

官方存在但未实测接通的生态条目统一在 [OBSERVED.md](../OBSERVED.md)，只供选型；启用任何一条前，必须按 3.6 的流程重新走一遍真实验证——这条纪律本身就是本附录存在的理由。
