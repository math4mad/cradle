---
name: "garden-outbound-letter"
description: "园子对外学者信件（Strang/Gärdenfors 类）的备稿-路由-落账全流程"
version: 2
created: "2026-09-22"
updated: "2026-09-25"
---
## When to Use
主人要求给外部学者写信、或交叉查询/验尸产生认亲-判读类通信需求时

## Procedure
1. 查先例: cora-atlas/letters/gilbert-strang/sent-*.md (甲案直邮+公开化双轨, 尾注 sent 戳)
2. 核收件地址: 优先 portal/系页实抓; 网络断则候选地址一律标'候核', 铁律=地址未验不发
3. 起草: cora-atlas/letters/<slug>/draft-0XX-vN.md, 编号承 chora letters/INDEX.md 最大号+1, 中文匣注+英文正文+附件清单表
4. 路由方案节必配: 甲案直邮 + 乙案三道 (第二地址转递→30天无回音即公开信化贴 cora-atlas+Zenodo+双轨社媒→辑者/门生隔山求转)
5. 附件备弹药: 论文 PDF 从 papers/*/arxiv/main.tex 用 tectonic 编译; 谱系/引用改动先过 REFS.md 验尸
6. 发送前净身 (0926 主人纠正入典): 邮件不认 markdown——从 draft 抽正文生成 send-0XX-vN.txt, 剥尽 **/*/ >/ #/围栏并自查残留符号 PASS; 匣注/路由节/编号头一律不带, 主人从 txt 复制粘贴
7. 挂账候批: bin/remind.sh add '📨 Letter 0XX 候朱批…' '备注=回复指令'; 勾=准发
8. 发后: 匣记 sent-YYYY-MM-DD-vN.md + LEDGER 园务行入册 + git commit (署名 math4mad noreply)
## Pitfalls
- AppleScript/日期字面量灾与网络障碍重试律照 AGENTS 执行, 信不过夜死磕
- 勿把模型或园笔 agent 列为作者 (arXiv/学界合规), 尾注 AI 披露句
- 谱系措辞须与主人最新改判一致 (如 0922 夜'独立收敛'改判), 落笔前查 memory/LEDGER

## Verification
1. draft 匣内有甲/乙案路由节且地址标候核
2. remind.sh read 可见该候批条
3. LEDGER/REFS 与本信口径一致, git log 有对应 commit