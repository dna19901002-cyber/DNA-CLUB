# V0.6 手动操作说明（模板，未执行真实试验）

仅在用户明确手动触发的真实交易日使用。先准备既有服务器完整当日 C 原始 JSON 和已认证交易日历文件（含信号日、间隔休市日及次一开市日），保存到独立 V0.6 incoming 子目录并核对原始来源和 SHA。不得覆盖旧研究、调用新增行情或自己把未知日历改标 VERIFIED。没有这些材料就停止并报告缺失。

项目根目录检查软件状态，不会等待或获取行情：

```powershell
.\.portable_python\python.exe -X utf8 -B research\theme_continuation_v0_6\manual_chain.py status
```

14:50 已真实读取到完整快照后、留足 14:52:00 前完成时间，由用户手动运行一次（下列尖括号均为必须替换的模板）：

```powershell
.\.portable_python\python.exe -X utf8 -B research\theme_continuation_v0_6\manual_chain.py freeze --snapshot "<完整latest_experiment_1450.json或history/日期/1450.json绝对路径>" --snapshot-sha256 "<已核对的原始SHA256>" --calendar "<已认证日历绝对路径>" --calendar-sha256 "<日历SHA256>" --codes "<当日原始C中代码>"
```

最多两个代码；不需要 GPT selection，不会自动排名或补选。若无候选，省略 --codes；C 非空但不选时增加 --no-signal-reason "<事前理由>"。不要把测试中出现的历史代码当作当日推荐。

输出 result_status=FROZEN 才有合格准入记录。保存 acceptance_receipt、SHA、records 的原始路径和 payload SHA；frozen_at 是引擎不可变记录写入完成后的实测时刻。任何 DATA_INSUFFICIENT 都不产生有效信号。原始文件已生成而文件尚未发布/取得时不得借用生成时刻；不要另开存储目录绕过当日锁。提交时刻可选且仅为旁注，不能当首读证据。

次一真实交易日 09:50—09:55 之前与 09:55 之后各手动触发一次 observe。输入 document 为 V0.4 原格式早盘证据描述（code、observation_trade_date、source_available_at、market_cutoff、RAW_UNADJUSTED price_basis、5m resolution、minute_bars、original_source.path/sha256），必须逐项对应已保存原始行情。原始行情自身或匹配的原始 observation 条目必须有可核验价格基准、5m 精度和生成/保存时间；单独在描述文件填这些值不能认证原始数据。没有原件就记录缺失，不生成价格结果。原始 source_available_at 可以来自其明确 recorded_at_cn，但不能把本次读取时刻回填为服务器生成时刻。

```powershell
.\.portable_python\python.exe -X utf8 -B research\theme_continuation_v0_6\manual_chain.py observe --acceptance "<冻结接纳凭据绝对路径>" --signal "<其中对应signal原始路径>" --document "<早盘证据描述绝对路径>" --document-sha256 "<描述SHA256>" --calendar "<认证日历绝对路径>" --calendar-sha256 "<日历SHA256>" --cutoff 0950
```

09:55 阶段使用当时新取得的对应描述与 SHA，--cutoff 改为 0955；09:50 已过窗口不能事后补成点时观察。若冻结是 NO_SIGNAL，--signal 使用字面量 NO_SIGNAL，document 使用原始早盘机器快照（须有 trade_date、selection_trade_date、cutoff、真实 generated_at_cn 或 retrieved_at_cn、LIVE/LIVE_EXPERIMENT），只记阶段到达凭据，不记股票价格或收益。

真实首读凭据独立保存在 manual/reads 与 manual/first_reads，原字节为 manual/sources 的内容寻址副本；记录 SHA 校验不代表第三方时间公证。core records 是未准入原件层，必须连同包装器 attempts 凭据检查；失败或迟到原件不可统计。测试只在 test_scratch、TEST_PROTOCOL_ONLY 下运行并清理，禁止拷入 manual/ 或 records/。

保持三次机会与失败记录；三次真实机会都无法取得合格凭据则停止自动集成研究。用户需另行提供可验证的发布路径或人工执行方式变化才重评，不通过删记录重试。任何生产源码 SHA、保护文件变化、来源不完整、次日无法对应或需要新增接口/调度才可能通过，都先停止集成路径。本说明没有授权部署或自动执行。
