# Monitor提醒程序与验收

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

实际CMD入口`启动A股实时盯盘.cmd`启动`stock_realtime_wecom_monitor.py --autostart`。本轮只读，不执行。可见代码引用monitor_lifecycle.py、alert_presentation.py。运行目录为`<LOCALAPPDATA>/AStockMonitor/`，含config.json、state.json、logs、lifecycle账本；当前指令路径按源码COMMAND_FILE解析，不复制原文件。

SCHEMA_CODE_VERIFIED / RUNTIME_SCHEMA_NOT_RETESTED：真实接收函数validate_monitor_command可见，schema_version必须为a_stock_monitor_v1，notify_only必须true；requires_manual_confirmation缺省true且显式给出时只能true。交付侧必须明确写true。未知字段被拒绝。

顶层允许字段：schema_version、command_id、generated_at、source_analysis_as_of、valid_until、notify_only、requires_manual_confirmation、market_gate、rules。单规则允许字段：id、code、name、stock_name、action、quantity、condition、active_after、active_before、once、cooldown_minutes、confirm_seconds、requires_manual_confirmation、manual_checks、note、message、reason、description、title、text、action_execution_status。允许字段不等于全部必填；生命周期层仍有附加约束。代码6位、规则id非空且唯一、action非空；数量与冷却/确认秒数为非负整数；开始/截止时刻须有效且有序。

条件枚举（由当前源码AST抽取）：all、any、cross_above、cross_below、m30_close_above、m30_close_below、pct_change_above、pct_change_below、price_above、price_below、price_in_range、pullback_from_high_pct、time_after。
all/any递归且子项非空；区间须lower/upper且有序；pullback需pct；time_after校验真实时钟；其余价/比例条件需要数值。仅公布字段与枚举，不发布含真实价位的可运行规则。

报价路径含腾讯公开报价，现有程序还需按具体quote分支核对回退。发送链为报价→条件确认→生命周期PENDING→既定企业微信HTTP→持久送达状态。可信报价触价属于观察；有合格新鲜30m/5m依据的结构提醒另审。m30条件名称本身不证明严格结构证据合格。触价不升级S3；PB4不得跳过健康回踩。

生命周期代码：command_id绑定完整指纹，同ID异内容拒绝；完成语义层去重，改ID/改名不复活；命令和层级撤销墓碑持久化；战术到期后，仅有效独立授权、有效来源SHA和期限内的硬风险可保留。事务PREPARED/COMMITTED、启动恢复和reload回滚管理磁盘/内存一致；发送前PENDING，超时或发送后持久化不确定进入DELIVERY_UNKNOWN并停机，人工核销前不自动重发。不承诺网络与磁盘严格一次事务。

|验收维度|证据与状态|
|---|---|
|CODE_INSTALLED|当前工作副本SHA与10/29准备回执所列生产源码SHA比较见证据索引；历史安装不等于当前进程已加载|
|OFFLINE_PASS|10/27报告83/83候选离线；10/28连续观察准备9/9；仅PRIOR_REPORTED，本轮未重跑|
|LIVE_MESSAGE_CONFIRMED|上午单次记录POST=1、下午有限运行POST=2，均API_ACCEPTED；user_receipt=NOT_CONFIRMED；不能声称用户收件确认|
|FULL_SESSION_ACCEPTANCE|UNKNOWN / NO_GO，记录all_day_GO=false|
|PRODUCTION_GO|NO_GO；10/29准备approved=false，午休与截止护栏仍有阻断|

所有提醒notify_only=true、requires_manual_confirmation=true。done表示提醒生命周期，不等于真实成交或可卖权限。本任务不发送测试通知、不恢复/修改current、不启动监控。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
