---
name: "protect-core-dialogues-papers"
description: "核心资产（对话母本与论文源件）三存保全与销账流程"
version: 1
created: "2026-09-23"
updated: "2026-09-23"
---
## When to Use
主人令「保住对话与论文」/ 会话开场查家底 / 发现某资产只有盘上一份 / 需要重封或验证保险柜时。园体优先级：① 对话与论文（必保）② 实验室诸案（锦上添花）③ 依附品（让路）。

## Procedure
1. 清点: bash ~/Programming/code-2026/Concept-Space-Sphere/bin/vault-core.sh —— 扫 external/ 对话母本 + cora-atlas/papers 论文源件 + chora letters/docs + PREREG_*.md, 出 sha256 总账 ~/Cora-Vault/MANIFEST.tsv 并打当日封存袋 core-YYYYMMDD.tar.gz (第三方版权书 z-lib/Springer 与含密钥备份禁入库, 只登哈希)
2. 查三存: ① 盘上工作树 ② 本地封存袋 ~/Cora-Vault/ ③ off-disk 私有仓 math4mad/cora-vault (private); 单存即险, 立刻上报
3. 补 off-disk: cd ~/Cora-Vault && git add -A && git commit → 走 ~/Programming/code-2026/Concept-Space-Sphere/bin/queue.py add push-vault --cmd 'cd ~/Cora-Vault && git push -q origin main && [ "$(git rev-list HEAD..origin/main --count)" = "0" ]' (慢网勿手推死磕)
4. 验仓库对表: 各仓 git rev-list --count origin/main..HEAD 须为 0 (chora 曾积 28 发未推=对话录裸账, 用 push-chora-core 队列补)
5. 验字节: bash bin/vault-core.sh verify —— 变字节 0 · 失物 0 才算成
6. 销账: 结果落 cora-atlas/LEDGER.md (凡扎必回血), 再 bash scripts/build-site.sh 归化上站并 curl 活站复验

## Pitfalls
- Concept-Space-Sphere/.gitignore 第 4 行整片罩住 external/ —— 主人自产对话母本与手记 (技术哲学手记 5.1 万字等) 天下只有一份, 公开 git 救不到它
- 本机 Time Machine 无目的盘、iCloud Drive brctl 拒访, 别把「同盘的本地袋」当异地备份上报
- 私有仓只放自产件与哈希总账; 第三方原书 (z-lib/Springer) 与含密钥 pouchdb 备份一律禁传
- github 断连时按主人重试律: 快试 3 次 → 10/20/40 分钟各一次 → 收兵挂账, 禁止高频死磕
- cora-vault 与 ~/Cora-Vault 同名不同物: 前者是私有仓, 后者是本地封存目录

## Verification
1. MANIFEST.tsv 件数 > 0 且 verify 报「变字节 0 · 失物 0」
2. gh api repos/math4mad/cora-vault --jq '.visibility' 出 private, 且 git ls-remote 与本地 HEAD 同 sha
3. chora / cora-atlas / cora-vault 三仓 ahead=0
4. cora-atlas/LEDGER.md 有本期账行, 活站 LEDGER.html 内含该关键词