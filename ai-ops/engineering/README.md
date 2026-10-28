# 工程知识库交接入口（公开脱敏版）

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

本目录为独立分支工程文档交付，尚未合并main；不代表正式部署或交易许可。本地导出器只收集/整理客观JSON/PDF，GPT在用户手动上传后负责分析，Monitor只发提醒；独立尾盘隔夜不与跑预设账户体系合并。

三份GPT检查点由用户/主GPT成组导出并人工更新Sources，生成文件不等于自动替换。公开仓库不存私人契约原文或状态全量。

当前代码为VERIFIED_CODE；历史测试/部署为PRIOR_REPORTED；本轮无现场VERIFIED_RUNTIME。Monitor可见有限API_ACCEPTED记录、用户收件未确认、全天NO_GO；Task22旧报告仍NO_GO。尾盘10/28修复已reported，192项离线PASS不能替代下一真实日验证。私人Sources与云机最新状态UNKNOWN。

宝宝Codex可审计本目录、公开固定commit和脱敏SHA索引；对本地非公开源和云机当前字节需要维护者另供证据。原有ai-ops试验位于ai-handoff/pilot-20261028，不在本次main基线上；不修改其历史回执，也不宣称已合并。

|文件|状态/证据|
|---|---|
|[总览与边界](00_system_map_and_boundaries.md)|已生成；来源与SHA见证据索引|
|[客观导出器](01_objective_exporter_architecture.md)|已生成；来源与SHA见证据索引|
|[Monitor与验收](02_monitor_architecture_and_acceptance.md)|已生成；来源与SHA见证据索引|
|[独立尾盘云链](03_overnight_pipeline_architecture.md)|已生成；来源与SHA见证据索引|
|[GPT角色与检查点索引](04_gpt_project_contract_and_checkpoint_bridge.md)|已生成；来源与SHA见证据索引|
|[整改测试台账](05_change_log_validation_register.md)|已生成；来源与SHA见证据索引|
|[维护与安全](06_maintenance_handoff_and_safety.md)|已生成；来源与SHA见证据索引|

本次用户直接要求上传至Git；文档中的待审批阶段作为参考，由本次直接上传请求授权独立分支文档发布。不得据此获得生产、实网采集、消息或交易授权。自动扫描及内容复核完成后才发布；实际远程commit与逐文件SHA在本地交付回执记录。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
