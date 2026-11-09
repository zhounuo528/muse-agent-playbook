# 微信公众号文章读取（移动端 UA 直抓）【随仓附带】

## 用途
用户发来公众号文章链接（mp.weixin.qq.com/s/…）问讲了什么、要摘要、要存档时，用这个技能秒读全文。不走浏览器自动化（慢且重）。

## 原理
公众号文章页是服务端渲染的，「环境异常」验证墙主要拦不像微信客户端的请求。伪装 iPhone 微信客户端 UA 用 curl 请求文章页，可直接拿到完整 HTML：正文在 `id="js_content"` 区块，标题在 og:title。

## 安装
1. 把本目录拷进你的技能目录。
2. 纯文本模式只需要 `curl` ＋ Python 3 标准库，开箱可用。
3. 保格式 Markdown 模式（`--md`/`--save`）需要 markdownify：建议在本目录建虚拟环境并把脚本首行 shebang 指向它：
   ```bash
   python3 -m venv .venv && .venv/bin/pip install markdownify beautifulsoup4
   ```
   没装 markdownify 时脚本会自动退回纯文本并在 stderr 说明，不会报错中断。

## 工具
```bash
bin/wxread "<文章链接>"                 # 纯文本正文（标题/公众号/作者/发布时间/正文）
bin/wxread "<文章链接>" --json          # 结构化字段 title/mp_name/author/publish_time/content/chars
bin/wxread "<文章链接>" --md            # 保格式 Markdown（加粗/标题/链接/图片保留）
bin/wxread "<文章链接>" --save <路径>.md  # 存成一篇完整 Markdown 文档（适合归档）
```
正文图片在纯文本模式下保留为 `![图](原图URL)`。
退出码 1 且 stderr 报 `FETCH_BLOCKED`＝被验证墙拦下；报 `FETCH_FAILED`＝抓到页面但没有正文区块（可能不是标准图文页，如视频号/小程序页，本工具不支持）。

## 使用规则
1. 用户贴公众号链接，先用本技能读，不要先开浏览器试错。
2. 问「讲了什么」给要点摘要，不要整篇原文倒给用户；用户要全文/存档时用 `--save` 落文件并附上。
3. 文中数字、日期、人名按原文转述；抓不到的字段（个别文章没有作者/时间）如实缺省，不编造。
4. 只读公开文章页，不登录、不带 Cookie、不做任何互动（点赞/评论/关注都不碰）。
5. 所在环境如需走代理，curl 会自动读环境变量里的代理设置。
6. `FETCH_BLOCKED` 连续出现时先报告实情，再问用户要不要换路线，不要自作主张开慢速浏览器长任务磨。
