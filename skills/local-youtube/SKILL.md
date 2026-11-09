# YouTube 字幕抓取（视频摘要供料）【随仓附带】

## 用途
抓取 YouTube 视频的字幕文本，交给 agent 做摘要——用户不用看完视频就知道讲了什么。

## 工具
```bash
bin/yt "https://www.youtube.com/watch?v=VIDEO_ID"
bin/yt VIDEO_ID
bin/yt VIDEO_ID --lang zh      # 优先中文字幕
bin/yt "https://youtu.be/VIDEO_ID"
```
输入支持：完整 watch 链接、youtu.be 短链、/shorts/ 链接、裸视频 ID。
输出 JSON：`video_id` / `title` / `language` / `language_code` / `transcript`；失败时返回 `error` + `error_detail`（`no_captions`＝视频无字幕，`fetch_failed`＝抓取失败）。

语言策略：默认优先英文（覆盖最广），`--lang zh` 优先中文；都没有时自动退回第一个可用轨道。

## 实现原理
在很多服务器/云环境里直连 YouTube 会被封 IP（youtube-transcript-api、yt-dlp 经常直接被 Ban），因此本脚本经 **Piped 第三方 API** 中转：`streams` 接口拿字幕轨道列表，再拉其代理的字幕文件解析纯文本。内置 8 个公共 Piped 实例按序 fallback。依赖只有 Python 3 标准库。

## 使用规则
1. 拿到 transcript 后由 agent 写摘要，不要把几千字字幕全文甩给用户。
2. 失败时如实说明（无字幕 / 实例不可用），不要编造内容。
3. 重试前等 30 秒以上；不要高频刷同一个视频。

## 已知限制
- 只读字幕：没有字幕的视频抓不到；不能下载视频/音频。
- Piped 公共实例会被 YouTube 间歇封禁，成功率有波动：有时一次就成，有时全部不可用。失败多半是暂时的，过段时间通常恢复。
- 自动生成字幕（ASR）的质量取决于 YouTube 的语音识别，专有名词可能不准。
- 音乐类视频字幕多为歌词，信息量低，摘要时注意。
