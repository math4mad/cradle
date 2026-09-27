---
name: "lola-psyche-bootstrap"
description: "园笔会话开机/收尾走 psyche 三层持久化 (T0 复位、事件记账、handoff 快照、模式切换)"
version: 1
created: "2026-09-27"
updated: "2026-09-27"
---
## When to Use
每次园笔 (lola) 会话开场读完提醒账之后要恢复 T0 状态; 会话中出现关键决策/里程碑需要留痕; 会话收尾需要写交接快照; 或需要按任务切换工作模式 (forge/ledger/scribe) 时。

## Procedure
1. 开机: bin/remind.sh read 之后执行 python3 ~/Programming/code-2026/multi-coworker/techne/psyche/psyche.py load lola --events 5, 凭开机束 (身份层+快照+当前模式+尾事件) 回 T0 —— 严禁全量重读历史对话补课
2. 记账: 每个关键决策或里程碑执行 ... event lola <type> "一句话" (type 如 ruling/milestone/birth/repair/demo)
3. 换模式: ... mode lola set forge|ledger|scribe (未注册模式会被拒绝; 切换只换表现层, IDENTITY 恒在)
4. 收尾: 会话结束前把局势写入 JSON 并执行 ... handoff lola --json - (字段 narrative/decisions/pending/next/mode/session; 旧快照自动轮转存 states/, 保 3 份)
5. 改器需试验时先跑 ... selftest (临时户全流程, 5 项判据), 过关才动 residents/lola/psyche 正账

## Pitfalls
- 本机 python3=3.9: 类型注解需 from __future__ import annotations 置于 docstring 后首行, 否则 str|None 语法炸
- events.jsonl 只增不改 (append-only 是定律); 要修错误状态改 state.json, 不要回写日志
- workspace 路径由脚本相对自身定位 (REPO/residents/<id>/psyche), 拷 psyhe.py 别拷离学院仓, 或用 --home 覆写
- handoff 交互模式无 stdin JSON 时会弹四个 input —— 管道场景务必 --json -
- 器部试炼未毕 (毕业条件: 连开两日凭 load 复现 T0, 候主人圈点), 勿提前搬进业务仓

## Verification
1. psyche.py load lola 输出含四段: 身份层/工作层/模式层/日志层, 且工作层 updated 是最近一次 handoff 时间
2. psyche.py status lola 一行概览 mode/updated/events 计数与预期一致
3. 每次 event 后 wc -l events.jsonl 加一; 每次 handoff 后 states/ 快照数 ≤3 且 state.json 内容为己读
4. mode set 未注册名返回非零并拒绝切换