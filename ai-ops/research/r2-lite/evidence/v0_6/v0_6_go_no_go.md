# DNA-CLUB V0.6 决策

|项目|结论|适用范围|
|---|---|---|
|PATCH_REVIEW|PASS|当前源码字节匹配 V0.5 前置版本，候选副本与分钟 helper 兼容；生产补丁未应用|
|R2_LITE_PROTOCOL|READY|保留 V0.4 规则，手动实测读取、冻结完成时限、分阶段观察及失败留痕软件通过离线测试|
|R2_LITE_END_TO_END|NOT_READY|真实交易日试验 0 次，首读/冻结完成/次日两阶段证据尚未获得|
|PRODUCTION_INTEGRATION|NOT_AUTHORIZED|没有应用补丁、部署、改调度或更新历史保护基线|
|HISTORICAL_RETURN_BACKTEST|NO_GO|不开展历史收益回测；可靠模拟交易仍为 0|

只对下一阶段**用户手动触发的证据试验**给出 CONDITIONAL_GO；不授权自动集成。最关键缺口是完整 C 原始快照在截止前的实测首读和冻结完成凭据，以及可逐股票对应、价格基准明确、点时可得的次日 09:50/09:55 原始分钟证据。没有已认证日历也不能启动。现有提交时间和 positions=[] 都不能补足这些缺口。

本轮 72 项专项离线测试通过，原 V0.5 补丁 --check 通过（未应用）；291 项本轮保护 SHA 无变化。只读核对本地源码、缓存、服务器镜像和旧研究，不声称重新验证了运行服务器。未重跑 V0.1—V0.5 全审计、未更改画像人工字段、未重设旧基线，旧漂移与其历史报告原样保留。

新增 MeoZ/TuShare/外部行情调用 0，联网数据获取 0，新定时任务 0，订单 0，购买/PAT 请求 0。测试使用明确 TEST_PROTOCOL_ONLY 的隔离副本，没有加入实际研究样本；价格观察与成交、净收益完全分开，gross_return 和 estimated_net_return 均为 null，不形成胜率或盈利能力结论。

下一次只有用户手动触发才执行。一次成功不证明稳定性；如未来申请自动集成，至少复核三个独立真实交易日端到端时序和失败情况，再单独申请授权。三次机会都无合格凭据则停止该研究路径，保持协议 READY、端到端 NOT_READY，等待可验证的发布或人工执行条件变化。本轮到此交付，不自动等待、创建任务或继续部署。

交付：v0_6_patch_compatibility.md、v0_6_timing_evidence.json、v0_6_end_to_end_acceptance.md、v0_6_test_results.json、v0_6_go_no_go.md。另附 manual_chain.py、manual_runbook.md、独立副本、专项 tests 与 evidence。保留原 V0.5 五份交付 SHA 于 timing evidence，便于复核连续性。
