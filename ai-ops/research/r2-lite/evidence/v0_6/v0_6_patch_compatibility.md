# DNA-CLUB V0.6 日期候选兼容性复核

PATCH_REVIEW = **PASS**，含义仅限明确版本上的候选兼容性；没有应用生产补丁，生产日期异常不能因此宣称已经修复。核验时间：2026-10-25T21:03:03.758841+08:00。

首先检查字节 SHA，通过版本门槛后才复制候选并测试。当前主源码严格等于用户指定的 `fea9aff9df21eee14ab96d0a8b3e0b86fc6816818763e0335a69cb11dc6d70bb`。UI 和 minute helper 的前置版本依据 V0.5 保存的安装凭据，候选及 date helper 依据 V0.5 patch_manifest，V0.4 记录器依据原交付 SHA；逐项如下。

|文件|当前 SHA256（与所需版本完全相同）|结果|
|---|---|---|
|`stock_v9_5_ui_condition_order_profile.py`|`c720ca6caa335b221885b05bd9301736a23655d6f6210f66d4f4f185eefcf945`|MATCH|
|`pretrade_feature_builder.py`|`fea9aff9df21eee14ab96d0a8b3e0b86fc6816818763e0335a69cb11dc6d70bb`|MATCH|
|`minute_cache_evidence.py`|`c3773a9b973eabdb4ba2c1d4546b88b9a124ab3d2ff41166c7c163e70149f589`|MATCH|
|`research/theme_continuation_v0_5/patch_candidate/pretrade_feature_builder.py`|`80dd6c6677a77d89e229ca75421d4d17145a6d4d3d54f6a8c445838d4b630a31`|MATCH|
|`research/theme_continuation_v0_5/patch_candidate/market_history_integrity.py`|`8bb93dcdd2df603e993c8f0fb054d16a0a0bcc898177ae8c4fe0759e9b0afbda`|MATCH|
|`research/theme_continuation_v0_5/production_fix_proposal.patch`|`70f14aff820b93b06eee40ad86d7e1ceff28ca2d6f71069a09dc2251699da83e`|MATCH|
|`research/theme_continuation_v0_4/r2_recorder.py`|`0b8fc1b0d338e1b8ca38ea8525cf9ca4a0c647e5eca32a9e6c71714f82ff27fb`|MATCH|
|`research/theme_continuation_v0_4/strict_core.py`|`eda4c6b5876b1cc3c5d670d867d733ddf68fc8060473852a75ac769a930eea20`|MATCH|

完整可机读核验见 `evidence/version_gate.json`。`sandbox_source/` 是独立副本，未覆盖根目录文件。新增日期 helper 仍仅存在于研究候选目录，不安装到生产。`theme_logic.py` 的当前依赖身份另存于该文件，属于本轮依赖凭据，不是对旧基线的重新认证。

`evidence/dependencies.json` 记录直接本地依赖 SHA 和语法检查。objective_behavior_profiles、objective_minimal_bundle、objective_opportunity_repair、p0_data_quality、quality_time_semantics 均与 V0.5 开工凭据相同；新增 minute helper 与已披露安装凭据一致。日期与分钟两个标准库 helper 在同一进程导入并运行，通过无命名、字段及价格基准串用测试。分钟 `known_at` 仍只是 adapter 处理时钟，`provider_finality=unconfirmed`，不会被转成市场首读时间或原生分钟认证。

专项回归覆盖重复行情日只有一个样本、周末/节假日和跨午夜仍使用原行情日、旧行情及较早盘中快照不覆盖最新状态、缺日期/日期冲突/未来日期拒绝，以及相同 asof 经济字段冲突保留异常不择优。V0.5 保存的 20 条原始运行记录及 6 个认证行情日预览只作已有证据兼容性检查，未重跑历史数据审计。原始缺证据记录保留，不能自动继承为有效样本。

`last_5_runs` 保留既有字段名和列表结构，语义明确改为最近五个不同市场交易日。7 个独立日期再重复最后一天的测试证明趋势比较仍基于最后两天，不受同日重跑影响。候选 `latest_valid_market_regime_entry`、compact、history 函数在隔离缓存上串联通过；市场风险/风格原有输出字段与当前函数逐项比较相同（有意改变的日期字段除外）。除 V0.5 已声明的 5 个改动函数和 1 个新增函数，其余顶层函数 AST 一致，分钟处理函数未被改写。

已知消费者检索见 `evidence/known_consumers.json`：根目录 Python 和保存的 server app 范围内仅主 builder 命中，涉及历史继承、生成及向现有 JSON 输出传递。保留 `last_5_runs/risk_label_change/style_state_change/breadth_trend` 四个兼容键，实际隔离消费与 JSON 序列化测试通过。未保存的外部消费者、运行服务器和完整 GUI/生产入口未执行，不能据此给出全系统上线保证；旧便携副本及历史报告不属于本次修改范围。

共 72 项 V0.6 专项离线测试通过，详见 `v0_6_test_results.json`、`evidence/tests.log`。额外执行 `git apply --check` 退出码 0，仅检查原补丁上下文，未运行应用命令。没有部署、更新缓存或基线。291 项本轮保护文件 SHA 结束校验无变化；旧 V0.1—V0.4 已披露的历史漂移不因此变成 PASS。
