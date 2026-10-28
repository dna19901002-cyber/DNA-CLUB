# GPT契约角色与检查点索引

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

GPT_CONTRACT_CONTENT_NOT_READ。以下来自用户交接提示词的角色要求，USER_PROVIDED / GPT_OWNS / CODEX_CANNOT_ACCESS。本轮未读取私人对话或Project Sources，不使用旧文件拼接现行契约。

|角色|权属/可见性|
|---|---|
|唯一active Champion|GPT_OWNS / CODEX_CANNOT_ACCESS|
|唯一active输出契约|GPT_OWNS / CODEX_CANNOT_ACCESS|
|缠论原始来源|GPT_OWNS / CODEX_CANNOT_ACCESS|
|模型迭代/预测复盘契约|GPT_OWNS / CODEX_CANNOT_ACCESS|
|唯一active执行闸门Challenger|GPT_OWNS / CODEX_CANNOT_ACCESS|
|交易状态当前JSON|GPT_OWNS / CODEX_CANNOT_ACCESS|
|模型自适应当前JSON|GPT_OWNS / CODEX_CANNOT_ACCESS|
|预测待验证当前JSON|GPT_OWNS / CODEX_CANNOT_ACCESS|

高层流程（USER_PROVIDED，非本轮验收的自动接口）：用户手动上传导出配对包 → GPT校验八源及上一轮真实持仓/成交 → GPT分析并生成Forecast、Decision → 到期结算/学习 → 条件提醒拟稿 → 三份状态按现行规则同步导出。

提示词规定三份当前JSON共享state_cycle_id、source_bundle_id、cycle_as_of、sync_status、peer_state_refs。普通盘中不默认导出；用户要求检查点、跨会话或重大成交时三份一起intraday_partial；每日最后一次真正完整收盘复盘三份一起close_complete。文件生成不代表自动替换Project Sources；来源不同步时停止拼接。这是职责/接口索引，实际契约原文及本轮状态未取得，不能据此声称三份已同步。

公开交接页对私人Sources的VERIFIED_FILE是其原作者历史结论，本轮只记PRIOR_REPORTED，不能继承成Codex亲自读过。不得公开八源原文、滚动状态全量或Forecast台账。Codex不能晋升Challenger、改Champion、作实盘裁决或捏造Monitor可导入字段。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
