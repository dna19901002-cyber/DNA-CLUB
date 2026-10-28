# 独立尾盘隔夜云链

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

本地镜像可核：`meoz-cloud-deploy/project/app/overnight.py`负责A主链；experiment_jobs.py为B/C实验；machine_observation.py为机器观察；selection_sync.py、selection_contract.py处理选择；exit_review.py负责次晨A复核；github-sync/publish_json.py发布公开JSON。供应器有MeoZ与TuShare模块。它们不接管跑预设的账户、Champion或Monitor。

逻辑链：猫站/供应器 → 云端归档及规则校验 → public客观JSON → GitHub → GPT研究 → 人工选择检查点 → 既有selection同步校验 → 次晨复核。机器候选、GPT选择、模拟成交与券商成交是四类事实。A正式快照、B/C实验、机器观察、R2档案分别保留，不混计样本。

|用途|代码/产物路径|时点解释|
|---|---|---|
|A下午|public/latest_1430.json、latest_1450.json；history/<trade_date>/|本地timer镜像14:30:05、14:50:05；不是GPT选择截止|
|A次晨|public/latest_exit_0950.json|本地timer镜像09:50:05；分钟source_latest_time与生成时间分开|
|B/C|latest_experiment_1450.json、latest_experiment_exit.json、experiment_status.json、experiment_history.json|LIVE_EXPERIMENT；代码有exit0950/exit0955，后者窗口09:55；实际云端timer本轮未连机核实|
|人工选择|latest_selection.json、latest_experiment_selection.json|发布器列为EXTERNAL_FILES；不能用旧选择推断数据采集故障|
|机器观察|machine_observation.py及独立归档|不是GPT确认，不计可靠模拟买入|
|R2|research/R2_Lite_github_history_audit_20261027/|历史回放与前瞻冻结分离；不并入B/C|

公开字段版本包括ashare-exit-review-v1、ashare-experiment-snapshot-v1、ashare-experiment-exit-v1。逐件记录trade_date、run_context、generated_at_cn、接收/首次可读/提交时刻，缺项null。最新main读取的日期/模式/原始SHA由public_snapshot_evidence.json固定，不能沿用旧交接页的10/27日期。本地镜像不证明当前服务器字节或timer一致。

14:52在历史R2文档为读取冻结截止；10/27首个相关提交14:52:07，早于此时读取旧副本被拒绝。09:52是旧交接页报告的研究复盘时点，实际任务配置本轮不可访问，标PRIOR_REPORTED；09:50/09:55客观数据窗口不等于09:52时已得到09:55完整数据。用户已取消14:45迁移，本次不实施。

凭据边界：发布器代码从环境变量GITHUB_PAT、GITHUB_REPOSITORY取写入凭据；本地systemd镜像通过EnvironmentFile注入。未读取环境文件内容。服务器目前存放方式、PAT有效期与告警实际部署UNKNOWN。建议由维护者按最小写权限轮换、验证新凭据后撤销旧凭据，并对401/403、重试积压、发布时间过期告警；这是建议，不宣称已安装。PAT失效影响GitHub发布，不等于行情Token失效。本机本次GitHub连接器授权与服务器PAT隔离。

10/28异常：MAIN_CODEX_REPAIR_REPORTED / NO_DUPLICATE_ASSIGNMENT。既有报告称单位校验退市名称回退修复15:16:40部署、192/192离线通过；今日LIVE仍DATA_INSUFFICIENT。离线重放为RETROSPECTIVE_REPLAY，不回填历史模拟成交；下一真实交易日自然运行验收未在本轮完成。现场部署SHA只按原报告记录，当前云机NOT_ACCESSIBLE。

GPT跨会话仍由人工检查点继续；本次没有自动选择或自动回写。RETROSPECTIVE依据事后可用资料；PROSPECTIVE必须有事前冻结、来源取得和首次可读凭据。Git commit时间只是其中一项，不能代替首次读取证据。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
