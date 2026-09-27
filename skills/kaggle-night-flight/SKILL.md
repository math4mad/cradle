---
name: "kaggle-night-flight"
description: "chora 实验夜航 Kaggle 的装弹-发射-回收全流程（含双席位限制、torchao 疫苗、日志即幸存者协议）"
version: 8
created: "2026-09-22"
updated: "2026-09-27"
---
## When to Use
需要把 chora 实验推到 Kaggle GPU 跑、或在双席位限制下排夜航批次时

## Procedure
1. 臂目录三件套: experiments/<案>/<arm>/{run.py, kernel-metadata.json}；metadata 的 model_sources 用挂载名 qwen-lm/qwen2.5/transformers/<size>/1（0.5b/1.5b/3b/7b-instruct 皆在册, llama 候网页许可）
2. run.py 头部必带疫苗二代行 (pip uninstall torchao) —— Kaggle 镜像 torchao 0.10 与 transformers 不兼容, 是 0919 矩阵全灭真死因
3. 大语料走 matrix_corpus_embed.py 内嵌 (b64+sha256 自校验), 勿依赖多文件推送 (kernel push 只带 code_file, import 会 NameError)
4. 舱内结构法 (0927 立律): 每一训/每一配对/每一汇总算完即 json.dump 落 /kaggle/working 并 print REPORT_LINE(b64) —— 严禁把汇总统计排在训练循环之后; 逐训附一行 RSS-MARK 自证内存
5. 发射: /opt/miniconda3/envs/default/bin/kaggle kernels push -p <dir>; GPU 双席位满时报 'Maximum batch GPU session count of 2 reached'; push 后必 kernels pull 回远端对 sha (id==slug 亦须验)
6. 排队勿死磕: 用 Concept-Space-Sphere/bin/queue.py add <id> --cmd '<射手/回收脚本>' [--tries N --base 秒] 补位 (巡 status, 席位空即推下一臂); 脚本放案目录免 shell 引号之祸
7. 回收: 哨挂 kaggle kernels output <ref> -p <dir> (COMPLETE/ERROR/CANCEL 皆收 —— 日志即幸存者), 四验: 完赛 + 格数齐 + 哨兵专属词 + 无 0 字节壳
8. 判读: 新尺新刀先跑 --selftest 复册旧案在册数字 (容差先冻), 过了才判新数据; 产物入册 matrix_out/ 或案目录 out/, 数字入 LEDGER 前先对 sha256, >95MB 大件出册留盘上云灶
9. 收账三落: 战报 md 入案目录 + cora-atlas/LEDGER.md 追行 (append 后必验 git diff 只加不减) + bin/remind.sh done/add 销挂账; 站点改动走 scripts/build-site.sh 后 curl 活站 grep 验字
## Pitfalls
- kaggle CLI 的 OAuth access token 约 12h 过期; kaggle auth login --force 会起浏览器回调, 需主人点一下
- kernel-metadata id 必须全小写连字符 —— 且必须与 title 推导 slug 完全一致; id≠slug 连环灾 (LADDER-2 实测): 首 push 按 title-slug 建档, 此后按手写 id 重推=409 静默失败, status 打的还是旧版尸体 —— 防法: 以 push 返回的实际 URL 为唯一真址回填 id, 重推后 kernels pull 回远端 grep 新改动行验落版
- queue add 必带 --cmd 旗标 (位置参数会报「✘ 缺 --cmd」), 且 --cmd 里有 $() 时外层必须单引号, 否则创建时即被本地 shell 展开咬坏
- bitsandbytes 4bit 臂 (7B) 与 bf16 臂偏差要在 PREREG 注记
- Kaggle 镜像 transformers 5.x: apply_chat_template 返回 dict 须 return_dict=False; from_pretrained 用 dtype= (非 torch_dtype)
- 外层 except 只存 repr(e) 丢验尸线索 —— 错误臂必带 traceback.format_exc() 尾入报告, 哨兵词收紧到成功专属词
- model_sources 挂载名不是 HF repo id —— from_pretrained 须走 /kaggle/input/models/<slug>; 对齐 exp11: dtype=bfloat16 + padding_side right + 不设 local_files_only
- scp -q 静默失败灾 (LADDER-2b 首射 02:44 实测): 网络抖断时不回错、远端留旧版 —— 铁律: 远端执行任何脚本前先本地+远端 sha256 对撞, 不符不开火
- pkill 余爆吞新进程 (2b 二射 02:48 实测): 起新弹前先 mv 旧日志留尸改名, setsid nohup 起, sleep 12 后 ps -p 验活
- AutoDL 仓适配 (LADDER-2b 水土二连): 迁 AutoDL 须改 OUT=/root/autodl-tmp、MPATH 走环境变量; torch 升级一律 setsid 后台 + 短探针轮询
- 配对滞后于训练循环 = 一死即零收 (2026-09-27 PB23c v4 实证): 先跑完 36 训再统一算 66 对, OOM 一 Killed 就抹掉 25/36 训 (残报 runs: []) —— 铁律: 配对/汇总即算即落盘, 逐籽成环 (算完该籽即 fl() 落盘再弃其内存); PB23b 保住 15 对只是侥幸, 侥幸不得当律
- 单灶制 (build-once, deepcopy 全案仅一处) 仍会 OOM (0927 PB23c v4): 逐训 deepcopy 不是唯一病源 —— 逐训打 RSS-MARK (resource.getrusage→MB) 落日志才定得区位; 缓解三件套: 逐籽 del + gc.collect() + torch.cuda.empty_cache()、PB_SEEDS 单籽分弹、RSS 曲线留痕
- 判读仪必先 selftest 复册旧案数字方许判新案 (0927 PB16c-formal 立园例): 容差先冻 (如 TOL=1e-3 记三位/四位舍入积), 9/9 合了才动新数据; 恒等哨 (A*_R≡0 一类构造为真) 只验器, 不得计为对假说的支持
- 仓内禁 >95MB 块 (0927 push 连败十二次案): 预提交尺寸闸 chora/bin/hooks/pre-commit → bin/guard-size.sh (装法 git config core.hooksPath bin/hooks); 误入未推历史则 rm --cached + filter-branch 只重写 origin/main..HEAD (先 refs/backup 留退路, 推成后拔锚 + reflog expire + gc); 大件走云灶 bin/yunpan.sh put + OVERSIZE-REGISTER.txt 存目
## Verification
1. kernels status 到 COMPLETE
2. 回收目录内 report_*.json 可解析且 adapter safetensors >1KB
3. queue list 该回收任务为 ☑