# BABY-CODEX-003｜2026-10-28 尾盘单位校验异常，跨Codex分工审计

status: READY_FOR_BABY_CODEX
execution_scope: READ_ONLY_PUBLIC_DATA + DOC_ONLY_RESULT
assigned_role: 宝宝Codex
reviewer: 主GPT
engineering_follow_up: 主Codex（取得本地源码后的独立工作，尚未授权部署）
base_commit: 56e668dae576a8895cfd76deac2c386fe2e9fe27
created_at_cn: 2026-10-28
scope_branch: ai-handoff/baby-codex-003-20261028

## 本次要验证什么

不是把异常直接修好，而是验证在没有主Codex私有代码、没有主GPT历史对话的前提下，宝宝Codex能否：
1. 读固定证据、区分已证实事实与假设；
2. 追踪 A 14:30→A 14:50→B/C 的错误传播；
3. 提出可由主Codex离线证伪的具体工程假设和最小验证步骤；
4. 只通过GitHub的隔离文档提交结果，供主GPT审阅；
5. 不修改正式交易/行情仓库数据和运行权限。

## 必读文件
- `ai-ops/README.md`、`ai-ops/AGENTS.md`、`ai-ops/STATUS_20261028.md`
- `ai-ops/tasks/BABY-CODEX-001.md` 和 `ai-ops/tasks/BABY-CODEX-001-RESULT.md`：继承字段路径、ABSENT/null、SHA类型审计经验
- `ai-ops/evidence/BABY-CODEX-003-EVIDENCE.md`：本轮冻结的公开快照索引
- 本文件

## 固定公开证据（不要把 main 当前值和旧提交混读）

对2026-10-28取证，统一先用固定commit `b8d2ed00131a40b1277430c457d7f4861bb374f7`；
对2026-10-27对照，使用 `a09787593e17a560e307647770a513009a9c581d`。
如需核对当前main，**另列**新的main SHA和真实数据日期，不覆盖固定取证版本。

对照文件：
- 10/28 `latest_1430.json`：A 14:30
- 10/28 `latest_1450.json`：A 14:50
- 10/28 `latest_experiment_1450.json`：B/C
- 10/28 `experiment_status.json`：选择流程 / 当前状态
- 10/27 `latest_1450.json`、`latest_experiment_1450.json`：对照

## 事实边界（须自己独立复核）
- 10/28的A 14:30及14:50均为 `DATA_INSUFFICIENT`，原因 `SCREENING_UNIT_VALIDATION_NOT_COMPLETED`。
- `data_quality.unit_validation.result=NOT_CHECKED`、`validated_at=null`、`checked_rows=0`、`detail=REQUIRED_AMOUNT_OR_VOLUME_MISSING`。
- 对应screening请求 HTTP 200，rows=5656；**HTTP成功和覆盖数量不证明字段内容齐备**。
- B/C 14:50 `data_quality.screening_source=EXACT_A_1450_SAVED_ROWS`，两组同样 `DATA_INSUFFICIENT`，并非独立确认了两个供应商故障。
- 10/27 A 14:50 `NO_TRADE/CORE_INDEX_GATE_FAILED`，A本次未进入筛选校验 `NOT_CHECKED`；但10/27 B/C在单独请求后，筛选单位校验 `PASS`，checked_rows=5568。**不能宣称10/27 A的单位校验PASS**。
- 10/28 B/C的降级标记不能推导真实交易收益，也不能改变GPT选择权限或自动化配置。

## 宝宝Codex执行步骤
1. 从GitHub读取固定版本原始公开JSON，记下commit SHA、各文件Blob SHA、数据内SHA-256（若声明）、实际JSON路径、trade_date、generated_at_cn、retrieved_at_cn、run_context。缺失写 `ABSENT`，JSON null写 `null`。
2. 用一个时点表说明：10/27对照→10/28 14:30→14:50 A→14:50 B/C；区分请求成功、验证失败、市场闸门失败、GPT选择缺失四个独立概念。
3. 列至少三种**待证伪而非既定根因**：供应商某批行的amount/volume缺失或格式异常；原始字段/转换层映射变化；校验器对样本行选择、停牌/特殊行或null处理有边界漏洞；A/B/C复用/发布的错误传播。每个假设列：所需本地源证据、可在离线fixture上运行的最小回归、证伪条件。
4. 特别查找已有公开JSON是否保留原始screening每行字段；若未保留，明确写 `RAW_SCREENING_ROWS_NOT_AVAILABLE_IN_PUBLIC_SNAPSHOT`，不能仅从单位检查摘要推定真实原始行的数据内容。
5. 建议给主Codex一个**纯离线**定位方案：冻结已有原始数据与源代码，不发起MeoZ/TuShare新调用；先原样回放当日失败fixture，保留字段键名/类型/空值比例/异常行ID、源时间和schema_signature；对比10/27合格fixture；核实 `screening` rows→映射→验证→保存→B/C复用每一级的相同数据；旧日窗口的数据缓存不得伪装新鲜性。源fixture不足时报告阻断，不编造样本。
6. 创建**唯一新增文件** `ai-ops/tasks/BABY-CODEX-003-RESULT.md`，明确：已证实事实、证据SHA与路径、假设及最小离线验证、未取得证据、权限边界、主Codex任务清单。
7. 在**宝宝自己的隔离分支**提交该结果并push GitHub（不推main、不直接修改本任务）。报告实际远程分支、Commit SHA、对照文件diff、远程回读的Blob SHA；若写入失败，提交本地补丁+错误回执，不索取Token。

## 明确禁止
- 不修改仓库根目录任何JSON、选择/历史/策略文件、GitHub Actions、腾讯云服务器或主Codex生产源码。
- 不新增任何MeoZ/TuShare请求；不执行日内模拟交易或真实券商操作。
- 不为了“通过验证”降低单位校验门槛、不推断amount/volume缩放、不把代理数据拼成原生。
- 不对Task22、Monitor、R2及独立尾盘系统宣称部署READY；不混用这些系统的状态或样本。
- 不自动创建或合并指向main的PR。人工验收前不部署。

## 交付的验收判据
- 可以基于固定时点JSON自我解释故障传播，而不把 `HTTP 200` 误当单位校验PASS。
- 不把2026-10-27 A `NO_TRADE` 错误地说成2026-10-28的单位校验症状。
- 能写出最小可复现、可证伪的工程调查任务；不在缺源码/缺原始行时虚构修复。
- 所有实际改动仅一份结果文档，可审计Commit和远程回读。
- 主GPT审阅后，再向主Codex分派离线源码复核；是否修复/发布需单独批准。

> 本任务是证据分级和AI协作测试，不构成任何交易建议。
