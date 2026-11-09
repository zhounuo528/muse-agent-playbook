# TMDB 影视数据库【随仓附带】

## 用途
搜电影/剧集、查详情与评分、看本周热门。免注册不可用——需要一个免费 API 凭据（见下）。

## 安装与凭据（手把手）
1. 把本目录拷进你的技能目录。
2. 取凭据：打开 themoviedb.org 注册免费账号 → 头像菜单「设置」→「API」→ 申请 API Key。页面会同时给两样东西，**二选一即可**：
   - v4「Read Access Token」（一长串，走 Bearer 头）→ 设环境变量 `TMDB_READ_TOKEN`；
   - v3「API Key」（短串，走 query 参数）→ 设环境变量 `TMDB_API_KEY`。
3. 凭据只经你的安全凭据通道/环境变量注入，**永不写进技能文件或聊天**。
4. 验证：跑一次 `bin/tmdb --trending movie`，返回真实影片列表即接通。

> 经典坑：v3 短 Key 和 v4 Token 是两套鉴权，混用（拿短 Key 走 Bearer 头）会 401。本包脚本已按变量名自动分流，你只需设对其中一个。

## 工具
```bash
bin/tmdb --movie "复仇者联盟"     # 搜电影
bin/tmdb --tv "绝命毒师"          # 搜剧集
bin/tmdb --trending movie         # 本周热门电影
bin/tmdb --trending tv            # 本周热门剧集
bin/tmdb --detail movie 299534    # 影片详情（id 来自搜索结果）
```

## 使用规则
1. 用户问电影剧集、找片、查评分时用这个技能。
2. 结果默认中文简介（脚本已带 language=zh-CN）。
3. 不打印、不记日志、不落盘任何凭据值；汇报里只提「凭据已配置」。
4. 热门榜一天查一两次足够，不要高频刷。
