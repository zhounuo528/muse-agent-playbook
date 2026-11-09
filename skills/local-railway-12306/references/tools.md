# railway-12306 工具参数详解

Server: 开源项目 Joooook/12306-mcp（MIT），上游 https://github.com/Joooook/12306-mcp
本文件只记包装脚本转发的工具参数；服务的安装与版本以其上游 README 为准。

## get-tickets — 余票/车次/票价查询

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| date | string | 是 | `yyyy-MM-dd`，不能早于今天 |
| fromStation | string | 是 | 出发地中文名或 station_code |
| toStation | string | 是 | 到达地中文名或 station_code |
| trainFilterFlags | string | 否 | 车次类型筛选，可组合：`G`(高铁/城际) `D`(动车) `Z`(直达) `T`(特快) `K`(快速) `O`(其他) `F`(复兴号) `S`(智能动车组)。默认 `""` 不筛选 |
| earliestStartTime | number | 否 | 最早出发小时 0–24，默认 0 |
| latestStartTime | number | 否 | 最迟出发小时 0–24，默认 24 |
| sortFlag | string | 否 | `startTime` / `arriveTime` / `duration`，默认不排序 |
| sortReverse | boolean | 否 | 逆序，默认 false |
| limitedNum | number | 否 | 最多返回条数，0 = 不限 |
| format | string | 否 | `text`（默认）/ `csv` / `json` |

返回示例（text）：
```
G1102 深圳北(telecode:IOQ) -> 广州南(telecode:IZQ) 06:06 -> 06:37 历时：00:31
- 商务座: 剩余5张票 199.5元
- 一等座: 剩余8张票 99.5元
- 二等座: 有票 74.5元
```
票量语义：`剩余N张票` / `有票` / `无票` / `无票需候补`。

## get-interline-tickets — 中转查询

同 get-tickets 参数，另加：

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| middleStation | string | 否 | 指定中转站，不指定则自动推荐 |
| showWZ | boolean | 否 | 是否显示无座车，默认 false |
| limitedNum | number | 否 | 默认 10，上限建议 ≤ 10 |

输出含换乘类型（同车/同站/换站换乘）、等待时间、总历时，以及每段车次的余票。

## get-train-route-stations — 车次经停站

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| trainCode | string | 是 | 车次号，如 `G101` |
| departDate | string | 是 | `yyyy-MM-dd` 该车次出发日期 |
| format | string | 否 | `text`（默认）/ `json` |

返回站序、车站、到/发时间、停留历时。注意个别跨线车后半程可能显示不同车次号，属 12306 原始数据特性。

## 车站/城市查询

- `get-station-code-by-names {"stationNames":"北京南|上海虹桥"}` — 站名转编码（`|` 分隔多个）
- `get-stations-code-in-city {"city":"上海"}` — 列出该城市所有车站
- `get-station-code-of-citys {"citys":"北京|上海"}` — 城市代表站编码
- `get-station-by-telecode {"stationTelecode":"IOQ"}` — 编码查车站详情
- `get-current-date {}` — 当前日期

## 已知限制

- 12306 预售期约 15 天，超期日期查不到。
- 频繁/并发请求会被 12306 风控拦截（表现为 cookie 获取失败或空结果）；单次定向查询一般没问题。
- 余票数据是查询瞬间快照，不保证下单时仍有票；购票必须去 12306 官方渠道。
