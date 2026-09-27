# 经验总结 - 从 Chora/Cora 到 Cradle（内部别名 Credo）

> 蒸馏日 2026-09-28，奉主人圈点：「只带经验，不带具体数据」。
> 名实：正名 **Cradle**（摇篮）— Baby Concept Space 之义所当然；**Credo**（信条）留作内部别名，记所继承之信条。最轻微的脚步也会留下痕迹。

## 核心架构模式（经验层，已携入）
- **三层持久化**：Identity（直注、恒在）+ Working Memory（state.json 快照轮转保 3）+ Event Log（events.jsonl 只增不改）
- **模式切换**：表现层可换（forge/ledger/scribe 三帖在 `workflows/`），身份层恒在——切换不换魂
- **Handoff 协议**：session 结束时生成 narrative + decisions + pending + next + mode + session，快照自动轮转
- **开机律**：会话开场先读提醒账（bin/remind.sh read），再凭开机束回 T0，**禁全量重读历史**
- **铁五条**：日期只走偏移量 / 读卷走批量取属性 / git 障碍重试律 / 产出必落 git / 判据先冻后用，改尺另册新冻

## 工作流与技能（SKILL.md 六件在 `skills/`）
- lola-psyche-bootstrap — 开机/收尾三层持久化流程
- kaggle-night-flight — 实验夜航（装弹-发射-回收，双席位限制）
- cora-atlas-quarto-site — 碑体站建站-改版-发布
- publish-dialogue-pages / garden-outbound-letter / protect-core-dialogues-papers — 出版、外信、核心资产保全

## 勘实挂账（蒸馏时如实登记，不造空架）
- 原卷所指 `~/.chora/workspace-laura/`、独立 `workflows/`、`config.yaml`、KGDL 计算框架：本机**查无实体**。
  - workflows 以 psyche/modes 三帖充任（最近似）；config.yaml 为 Cradle 首册新建；KGDL 未携入，待 Cradle 建设期按需重立。
  - 数据处理管道通用形（语料清洗 → 特征提取 → 模型训练 → 评估）作为方法论保留，实现随新数据重炼。

## Cradle 项目定位
- 基于儿童语料库（TalkBank 等）的概念空间分析——婴儿概念如何在语言输入中成形
- 继承 Chora/Cora 的架构模式与教训，但不带旧项目数据、密钥、对话记录
- 架构疑难可回娘家查阅（路径见 `reference/chora-cora-path.txt`；只读、临时、目的性）

## 注意事项
- Cradle 是独立项目，与 Chora/Cora 保持弱联系
- 回溯是按需操作，不是全量加载
- 实验判据先冻后用；儿童语料涉及真人儿童数据，**公开性与授权问题须先请示主人再外发**
