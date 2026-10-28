# R2-Lite历史证据交接（只读）

- 状态：PUBLIC_SANITIZED_EVIDENCE_HANDOFF / NOT_PRODUCTION_APPROVED
- 来源：用户先前上传的V0.6 `delivery_reports(1).zip`；各文件从该ZIP按原始字节提取，SHA256/Git blob见 `source_manifest.json`。
- 内容：V0.6 72项离线测试报告、实际日志、版本/补丁检查、时序验收及手动研究模板；不是完整V0.6源码或已应用生产补丁。
- 结论：`PATCH_REVIEW=PASS`（只证明候选兼容），`R2_LITE_PROTOCOL=READY`（软件层），`R2_LITE_END_TO_END=NOT_READY`，`PRODUCTION_INTEGRATION=NOT_AUTHORIZED`，`HISTORICAL_RETURN_BACKTEST=NO_GO`。72/72只是离线测试，并非72个交易样本。
- `47_handback_text_extracted.md`：2026-10-28交付文本，从ChatGPT文件库读取后重新导出，不是原始47项测试日志/ZIP；后续接入候选 `47/47 PASS` 是交付声明，尚未经独立重跑。
- 47项原始测试日志、隔离源码补丁、`results/portable_verification.json`、原始候选ZIP：**本轮未取得**。请主Codex或用户在本地另行提供经脱敏的原件与SHA，不要按摘要补造。
- 按日数据归档：本仓库main当前未证实 `history/`、`research/` 目录存在；用户本地研究路径示例 `<PRIVATE_WORKSPACE>/research/theme_continuation_v0_6/`、`<PRIVATE_WORKSPACE>/research/R2_Lite_local_evidence_candidate_20261027/`、`<PRIVATE_WORKSPACE>/research/R2_Lite_github_history_audit_20261027/`。这些目录只是历史交付中声明的相对位置，**本轮未读取本机或腾讯云真实目录/按日文件**；不得把目录声明当成可用历史数据。
- 可公开读取的客观现行入口：`DNA-CLUB/main`中的 `latest_1430.json`、`latest_1450.json`、`latest_exit_0950.json`、`latest_experiment_1450.json`、`latest_experiment_exit.json`。它们是滚动latest，不能代替逐日冻结历史资产或事前首读证据。请以实际trade_date、SHA、首次可读时间核验。
- 研究规则：仅复用已有合法存档；回放和前瞻必须分开，同股同日样本去重，缺未来路径按data_missing处理；不额外请求MeoZ/TuShare，不调用实盘，不能把机器提案或事后重放变成已冻结模拟交易。
- 操作权限：公开分支只读研究，任何生产补丁安装、服务启动、自动冻结、定时任务、券商执行仍需另外批准；不修改main、现有正式程序或任何交易契约。
