# DNA-CLUB V0.6 R2-Lite 手动链路验收

R2_LITE_PROTOCOL = **READY**；R2_LITE_END_TO_END = **NOT_READY**。本轮完成离线软件准备，不具备真实交易日验收凭据。2026-10-25 本轮准备未触发交易试验，不自动等待交易窗口。真实试验 0 次，真实冻结信号 0，可靠交易 0。

继续使用 SHA 封存的 V0.4 `r2_recorder.py` / `strict_core.py`，未改其源码、选股阈值或分类。`manual_chain.py` 仅提供实际读取、时序、失败凭据和准入包装；两个 HERE 存储根在该研究进程中指向 V0.6，所有写入留在独立目录。真实路径固定为 V0.6 `manual/` 凭据及 `records/` 引擎记录，测试路径只允许 `test_scratch/`，时钟注入只允许 TEST_PROTOCOL_ONLY。没有生产调度入口。

|验收条件|本轮证据|状态|
|---|---|---|
|原始快照与交易日历 SHA|SHA 校验、失败留痕、独立存储已离线测试；真实当日日历/快照组合待提供|PENDING|
|实际首次可读时间|读字节前后实际时钟、monotonic 耗时、原始字节 SHA、独立首读凭据及其 SHA 已实现；尚无真实窗口凭据|PENDING|
|14:52:00 前完成冻结|引擎完成不可变候选/来源/batch 写入后测量，比较到微秒；迟到 1 微秒拒绝测试通过|PENDING_REAL_TRIAL|
|候选来自原始 C 或 NO_SIGNAL|只接收完整 latest_experiment_1450.json / history/<日期>/1450.json；额外要求 groups.C.candidates 结构，缩减提案拒绝|SOFTWARE_PASS|
|次日 09:50 / 09:55|各自原始来源、读取凭据和可得时刻；分阶段、缺失、错序、重复、跳空、双触发测试通过|PENDING_REAL_TRIAL|
|零新增接口/任务/订单/正式改动|纯本地文件读取；保护 SHA 无变化|PASS_LOCAL_SCOPE|

`source_first_read_at` 是固定研究读取器对该路径与该内容第一次成功实际读取完成的时刻。它不是网络上全局最早可访问时间，也不等于提交时间。首读旁证独立于快照元数据，引用每次实测 read receipt；来源字节、首次/本次读取凭据分开封存。时钟信任边界是本机 OS 时钟加 monotonic 耗时，不声称第三方公证或服务器网络可达性证明；真实试验时还须核对传输来源、时区/时钟及这些凭据。可选 source_commit_at 若仅手填会标为 UNVERIFIED_MANUAL_ATTRIBUTION，不参与首读或截止判断。

冻结接收当日原始 C 中明确选择的 0—2 只，保留 V0.4 尾盘和交易资格验证。没有 GPT selection 也可独立验证完整 C 提案。数据不合格不补选；只有来源完整且没有明确 DATA_INSUFFICIENT 的空 C 才可锁定 NO_SIGNAL；C 非空而不选择时必须显式写原因。组身份、日期及 candidate_codes 与原始候选行必须一致。日期锁和引擎不可变批次阻止重复冻结、更换候选及事后新增。若引擎文件已写入后才发现超过截止，保留 provisional_freeze_completed_at 与失败记录，frozen_at=null、DATA_INSUFFICIENT；这些文件为隔离的未准入证据，不能直接用 V0.4 summary 计为有效样本。后续观察必须持有本包装器对应的 FROZEN 接纳凭据。

每次试验必存 market_trade_date、server_snapshot_generated_at、source_commit_at（可空）、source_first_read_at、read_started_at/read_completed_at、frozen_at、snapshot_sha256/calendar_sha256、selected_codes、eligibility_result 和 result_status。真实字段当前全缺或未开始，详见 `v0_6_timing_evidence.json`；prepared_at 仅是报告生成时间。

09:50 读取/记录必须在 09:55 之前，09:55 相关变化与全窗口 MFE/MAE 保持 null。09:55 只在对应快照已真实生成可读后处理。价格路径必须能定位到原始文件 SHA、股票、交易日、原始未复权基准和每根 5m bar；完整路径为 0935/0940/0945/0950/0955 的结束标签。显式 bar 身份/基准冲突拒绝，缺根、重复、错序和来源归一化不一致保留 DATA_INSUFFICIENT。正常标签的父层代码/日期/基准证据会写入每根 bar 的来源映射，不把 descriptor 自填的 RAW_UNADJUSTED 当作原始基准认证。

同 K 双触发顺序不明保留 hypothetical_tp_sl.order=UNRELIABLE；开盘越过门槛先按已知开盘进行假设标记，仍不产生交易收益。actual_fill=false、reliable_trade=false、gross_return=null、estimated_net_return=null 强制不变。NO_SIGNAL 次日仅可保存两个阶段的空股读取凭据，标为 NO_SIGNAL_STAGE_RECEIPT_ONLY / complete_stock_price_path=false，所有价格变化 null；不得把 positions=[] 宣称为完整股票价格观察。

沿用 V0.5 已保存参考，不重新联网：10/21、10/22 的提交分别为 14:50:33 / 14:50:31，10/23 完整 C 的提交为 14:52:07。三者都没有本读取器当时的首读及冻结凭据，不能补填，也不算本阶段三次真实机会。10/23 原始 C 还明确为 DATA_INSUFFICIENT / FULL_MARKET_SNAPSHOT_INCOMPLETE，空候选不能升级为合格 NO_SIGNAL，包装器已有专项拒绝测试。现有 09:55 latest exit 的 positions=[] 不能证明有完整观察；已有机器 ledger 价格条目也不能转成事前冻结样本。缺原始价格基准和 09:50 当时可得证明仍是实际障碍。

只在用户手动触发时执行真实试验，不启动后台等待、轮询或任务。固定读取器按有认证日历的独立真实交易日计数，使用一次当日机会后不接受同日替换或再次尝试；重复调用只保存拒绝、不加日期次数。快照尚未发布也计作该日机会。若三次仍没有合格首读/冻结或 NO_SIGNAL 锁定凭据，软件停止后续试验入口；不得删除机会记录复位。日历缺失导致日期无法认证的尝试单独保留失败，不虚构为已认证交易日，也不能据此无限延长人工的三机会门槛。

一次全部真实条件通过只证明一次链路可运行。未来自动集成还需至少三个独立真实日的端到端时序/失败复核，且必须另获人工授权。当前 PRODUCTION_INTEGRATION=NOT_AUTHORIZED，HISTORICAL_RETURN_BACKTEST=NO_GO。手动命令及输入要求见 `manual_runbook.md`。
