# 12306 火车票查询【随仓附带】

> 路径约定：本文件里的 `bin/…` 指本技能目录下的脚本。把本目录拷进你自己的技能目录后，用你那份副本的路径调用，下同。

## 用途
查 12306 车次时刻、余票与票价，不用开浏览器。只查不订：不能下单、不能买票。

## 前置依赖（先装这一步，1.0 踩坑点）
查询本体是一个开源 MCP 服务 **Joooook/12306-mcp（MIT）**，不在本技能包内，需要先单独装：

1. 从其 GitHub 仓库克隆到你工作区的 `mcp-servers/12306-mcp/`；
2. 按上游 README 安装依赖并构建（产物应为 `build/index.js`）；
3. 若你放在别的位置，调用前设环境变量 `MCP_12306_PATH=<你的 build/index.js 绝对路径>`。

本技能的 `bin/query-12306` 只是一个命令行包装：它以 stdio 方式拉起该 MCP 服务并转发一次调用。

## 工具
```bash
# 当前日期（解析"明天"等相对日期时先调这个）
bin/query-12306 get-current-date '{}'

# 站名 -> 编码（可用 | 查多个）
bin/query-12306 get-station-code-by-names '{"stationNames":"北京南|上海虹桥"}'
# 城市 -> 该城市所有车站
bin/query-12306 get-stations-code-in-city '{"city":"上海"}'

# 查余票/车次/票价（日期 yyyy-MM-dd，不能早于今天）
bin/query-12306 get-tickets '{"date":"2026-11-20","fromStation":"北京南","toStation":"上海虹桥","trainFilterFlags":"G","limitedNum":5}'

# 某趟车次的经停站
bin/query-12306 get-train-route-stations '{"trainCode":"G101","departDate":"2026-11-20"}'

# 中转方案
bin/query-12306 get-interline-tickets '{"date":"2026-11-20","fromStation":"北京","toStation":"西安北"}'

# 列出全部可用工具
bin/query-12306 --list
```
`fromStation`/`toStation` 接受中文站名或三字码。全部参数见 `references/tools.md`。

## 鉴权
无。服务走 12306 公开查询端点＋匿名会话，不需要账号、不需要 Key。

## 使用规则
1. 只查不订——下单买票必须由用户本人在 12306 官方渠道完成。
2. 日期必须是 `yyyy-MM-dd` 且不早于今天；用户给相对日期时先调 `get-current-date`。
3. 城市名有歧义时先列车站（如同城多站），别替用户猜。
4. 控制查询量：12306 对高频请求有风控。一个问题一次定向查询，不要循环扫多日多车次。
5. 失败（cookie/网络类错误）隔几秒重试一次；仍失败就如实报告（可能是风控），不要连环重试。
6. 预售期约 15 天，超期日期查不到任何车次。

## 常见坑
- 没先装 MCP 服务就调包装脚本 → 直接报文件不存在。先走前置依赖三步。
- 返回空 ≠ 没有车：先核对日期是否超预售期、站名是否写错。

## 接入验收口径

验收必须打到核心链路：跑通一次真实的 `get-tickets` 余票查询（真实日期＋真实车站对）才算接通；只跑 `get-current-date` 只证明服务活着，不证明余票链路可用。验收回执按 01 号模块的最小格式留一行（命令／返回要点／退出码）。包装脚本只放行 `references/tools.md` 登记过的工具名，未登记的一律拒绝——服务端升级新增工具时，先审再加进白名单。
