---
name: "publish-dialogue-pages"
description: "园中对话出版 talk-with-agents 静态页全流程（JSON 原浆优先，webarchive 备选，治表手术与发布）"
version: 1
created: "2026-09-26"
updated: "2026-09-26"
---
## When to Use
主人令把某段对话（千问 App / 园笔会话）做成 math4mad.github.io/talk-with-agents 静态页，或治站内坏表时

## Procedure
1. 首选源=千问 JSON 导出（markdown 原浆：真表带行界与加粗）入 external/；plist 配对 user/assistant 成 pairs，按主题锚句切篇，每篇 Talkmd/talk-YYYYMMDD-HHMM-N.md 格式「Math4Mad:\n> 问」+裸答+--- 分隔
2. 次选源=Safari .webarchive：python plistlib 取 WebMainResource.WebResourceData(整页HTML)→regex 解 <table><tr><td>（先剥 <[^>]+> span 壳）组 pipe 表；与旧残表配对用「行指纹集」模糊匹配（只留汉字字母数字，任一行前12字交集），表头常与前文粘连勿用
3. 禁走弯路：textutil/docx 与 App 内 PDF 导出皆把表熔成单行（docx 真表=0 已验尸）；千问打印只出一页
4. 成页=手写 qmd（勿依赖 intake.py 的 slug 与发布）：front-matter title/description + 「关于这一篇」callout 一条（单花括号 ::: {\.callout-note appearance="minimal"}，f-string 里写 {{{{ 出 {{ 系灾）+ 正文；表前后必留空行防 pandoc 吞表
5. 页放 talking/learning-theory（学问类）或 gossip（运维元事），date 定殿序自动收录；本地仓在 ~/Programming/code-2026/talk-with-agents
6. commit 用 -c user.name=math4mad；push 失败勿死磕，交 Concept-Space-Sphere/bin/git-push-patient.sh；Actions「Render and publish to gh-pages」自动发布
7. 验活站：curl GLOSSARY/页 URL grep shelfbar-row/table 计数；callout 未套上病根=div 直入 tr（浏览器剥出表外），须独立 tr 行

## Pitfalls
- intake.py 生成的页名从英文 slug，中文对话全撞 untitled.qmd——手写 qmd 后记得删杂物（ai.qmd 案）
- {{.callout 双花括号、尾 "}} 两种病要各自清；判渲染以活站 grep 为准，本地 build 缓存会骗人（quarto 文件级 render cache: 清 site/.quarto + touch *.qmd）
- 千问对话含私人图片/链接轮次要剔除不出版
- 并发会话会 git reset 覆写：发布前后 fetch 对账，禁 force push

## Verification
1. 活站每页 grep -c '<table' 与 -c 'class="callout' 均 >0 且无 '{{' 残留
2. 真表数 = JSON 原浆统计数（隐喻案 23）
3. Actions run completed/success