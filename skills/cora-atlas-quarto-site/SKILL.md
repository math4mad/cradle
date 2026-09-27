---
name: "cora-atlas-quarto-site"
description: "cora-atlas 碑体站 (quarto) 的建站-改版-发布全流程"
version: 4
created: "2026-09-22"
updated: "2026-09-27"
---
## When to Use
需要改碑体站导航/加页/重建发布 cora-atlas docs 站时

## Procedure
1. quarto 在 ~/tools/bin/quarto (1.10.18, tar 解于 ~/tools 根, 非子目录); 网络断时按 resume 律续传
2. 项目在 cora-atlas/site/: 内容直接放 site 根 (勿套 src/ —— quarto 会把 input 子目录原样搬进 URL 路径)
3. 0924 改版后信息架构: 碑前 index.qmd = chora 式暗金铭牌 (plate-hero + 五 hall 卡片); 四殿一匣在 site/parts/{philosophy,biology,statistics,experiments,fragments}.qmd —— 只列归类指针, 原件 (papers/, iterations/) 不挪路径不改 URL; 新页上站 = 写进对应 parts 页一行 + iterations/papers 原目录挂链
4. _quarto.yml 三坑: input 不是 project 的合法键; navbar 项无 sections 键; output-dir 用 ../docs-quarto。改版后导航 = 碑前/Φ/Β/Σ/Ε/Μ 六项 + 右置 台账/术语碑/Repo
5. 加新页: iterations/*.md 或 papers 案文 cp 成 .qmd 即入网; 索引在 iterations/index.qmd、papers/index.qmd 手动挂行; 并补一行进所属殿 (parts/*.qmd)
6. style.css 已盘化 (dark plate: --bg #0e0f12/金 #c9a959/Palatino); 改样式勿引新色板, 用 :root 变量
7. 重建发布: bash scripts/build-site.sh (render → docs-quarto → rsync 合流 docs/, PDF/figs/gaiden/apple-style 旧物不动)
8. push 走 bin/queue.py add <id> --cmd "git -C <repo> push -q origin main" 再 queue.py tick (慢网勿手推死磕; 注意 add 需 --cmd 旗标)
9. 验证: build 尾行页数 + grep 关键词入 LEDGER.html/GLOSSARY.html + 改版后必 curl 六站 (碑前+五殿页) 验 200 并 grep 内容
## Pitfalls
- quarto render 的 WARN 未解析链接多为旧站外链, 不阻塞
- mv 配置时勿把 _quarto.yml 弄丢 (本园踩过: 移动链断一半)
- docs/index.html 会被新站 index 覆盖, 旧入口按钮内容已迁 papers/index.qmd
- pandoc 管道表必须独立成块: 表前后各留一空行, 否则整表被吞成裸竖线 (2026-09-23 PB15 §6 bug)
- docs/ 会被并发会话 git reset --hard 覆写 (0923 实证); 发布后必 curl 活站验 200 + grep 内容, 勿只信本地; 发现分叉用 git merge origin/main 双行收口, 禁 force push
- LEDGER.md 旧尸灾 (2026-09-27 20:39 实证): 盘上 LEDGER.md 竟是当日 09:00 前的旧版 —— 今日 09:16→16:29 已 commit 的九行账在 HEAD 却在盘上蒸发, 且节题前夹一枚游手文字 (旧编辑缓冲回写之征)。园笔 append 之后才 v 到 git diff 出现 9 行『删除』。治法三条: ①落共享账前先 `git diff --numstat <file>` + `wc -l` 对撞 HEAD, 见盘上已缺行先 `git show HEAD:<file>` 还魂再写; ②append 后必验 `git diff <file> | grep -c '^-[^-]'` == 0 (只加不减); ③提醒主人: 编辑器若开着旧窗口, 一律 reload 勿 save
- 本机每仓皆有第二 clone 在 ~/Programming/code-2026/<repo> (chora / cora-atlas 皆双具, 各自 publisher 在跑): 验分叉用 `git -C <另一具> rev-list --left-right --count origin/main...HEAD`, 修好一具必回头看另一具是否同步
## Verification
1. math4mad.github.io/cora-atlas 出碑体导航七项
2. iterations 索引含最新日期手记
3. git push 成功 (queue ☑)