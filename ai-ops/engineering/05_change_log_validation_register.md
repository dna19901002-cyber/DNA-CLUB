# 历史整改、测试与发布台账

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

所有测试结果为既有回执PRIOR_REPORTED，本轮未重新运行研究或生产验证。last_code_sha无法绑定历史精确字节则UNKNOWN，不拿当前SHA冒充当时版本。

|project|component|last_code_sha|report_path|evidence_sha256|test_scope|PASS/FAIL/SKIP|installed|real_market_tested|production_accepted|remaining_blocker|owner|as_of_cn|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|跑预设|Task22|UNKNOWN（索引有当前源码SHA）|reports/22_data_release_separated_gates.md|c549da18389b1718766b15ac2ff867bf42a1295175ca31a7049600f4b308ec34|51项离线；新1m未运行|PASS_OFFLINE / SKIP_LIVE|UNKNOWN|NO|NO_GO|供应商最终性与真实边界未证|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|跑预设|分钟V3.2|UNKNOWN（索引有当前源码SHA）|reports/分钟数据修复_GPT终验与冻结_20261023.md|ff71f1f7d241373d83d6301c6424cc34b7a9b1d27f3053abcb7b2619c0c2a95c|59专项；81受影响回归（不相加）|PASS_REPORTED|NO（该候选报告时）|NO（该报告）|NO_GO|完整正式链与源语义未验|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|Monitor|生命周期|UNKNOWN（索引有当前源码SHA）|research/V9_monitor_lifecycle_fix_20261027/handback_for_gpt.md|3ccd5eb1243bf1387ac0b6e2d8f413c860c5176953e9bbf98b3de92959a5fc81|83/83离线候选|PASS_REPORTED|SEE_LATER_RECEIPTS|NO（该报告）|NO_GO|离线不等于全天验收|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|Monitor|真实单次|UNKNOWN（索引有当前源码SHA）|research/V9_controlled_start_20261028/session_result.json|c9da55ba99566777520bdd79557c4f5010d2ff69bafa0eaeb42673ddd42b1d81|3GET/1POST；用户收件未确认|PASS_API_ONLY|REPORTED|LIMITED|NO_GO|收件确认及全天证据缺|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|Monitor|下午有限运行|UNKNOWN（索引有当前源码SHA）|research/V9_continuous_prepare_20261028/afternoon_live/result.json|c4d3169d2cbf9c2debbe7cddb90dc6dfe5276628765c153671a6bde956d6e30c|3GET/2POST；POST预算后停止|PASS_API_ONLY|REPORTED|LIMITED|NO_GO|非全天|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|Monitor|下一日准备|UNKNOWN（索引有当前源码SHA）|research/V9_prepare_20261029/preparation_result.json|c412a7d87edaee91a2b2964d2ef606d94c486e34bfa42c5c01d046be0beb0ed9|只读SHA/边界检查；approved=false|PARTIAL / SKIP_LIVE|UNCHANGED_REPORTED|NO|NO_GO|午休与截止护栏阻断|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|尾盘|10/28单位修复|UNKNOWN（索引有当前源码SHA）|meoz-cloud-deploy/repair-20261028/单位校验专项调查报告.md|4afb17178916801f5cd463e93d832d70cc87281afdac69f492123b4eda81be5c|192/192离线+历史重放|PASS_REPORTED|YES_REPORTED|NEXT_DAY_PENDING|UNKNOWN|MAIN_CODEX_REPAIR_REPORTED / NO_DUPLICATE_ASSIGNMENT|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|R2-Lite|历史资产|UNKNOWN（索引有当前源码SHA）|research/R2_Lite_github_history_audit_20261027/一页中文结论.md|1e3b1627f8d6a8f7c01110f2011485b4380a473043877d0aa50a7a2cf630d202|6日信号回放；18离线测试；有效前瞻0|PASS_REPLAY / OUTCOME_INSUFFICIENT|NOT_AUTHORIZED|NO|NO_GO|首次可读、冻结及次晨路径缺|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|P3-C|双池日常|UNKNOWN（索引有当前源码SHA）|reports/13_p3c_forward_daily_operations.md|8ac2ec73741a5e246029f56e5128758e6d82770c86f8b27b601b5b59ae2d55dd|同特征日幂等；仅1独立日期|PARTIAL|RESEARCH_ONLY|LIMITED_REPORTED|UNKNOWN|两个真实特征日稳定性未证|主Codex/用户验收|2026-10-28T19:02:46+08:00|
|任务14|量价承接H0|UNKNOWN（索引有当前源码SHA）|reports/14_orderflow_h0_plan.md|a34feffae9f765178a619d0f9231015e47ed06b96bcc260a8eb801b6f9735e4e|研究预声明与可行性|SKIP_LIVE|NO|NO|NO_GO|单位/接收时刻/前瞻证据缺|主Codex/用户验收|2026-10-28T19:02:46+08:00|

缺陷复现PASS≠已修好；离线PASS≠已部署；API接受≠用户收到≠全天验收；35/35≠每卡点有效；机器候选≠GPT选择≠成交；旧文件mtime≠券商当日最终账户证据。云端reported部署SHA：f0d0a8995e84ed558d57f370fe57f11b7c8e491603cd785f6c8d5fc7b196aa5c。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
