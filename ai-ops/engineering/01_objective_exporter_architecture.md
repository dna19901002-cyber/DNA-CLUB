# 本地客观数据包导出器

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

入口由实际CMD确认：`启动A股数据包导出中心.cmd`启动`stock_export_center_ui.py`；CLI包装`stock_objective_export.py`通过runpy执行`objective_info_preset.py`，后者显式引用`stock_v9_5_ui_condition_order_profile.py`。文件名V9.5并不能证明用户当前正在运行旧GUI。本轮不启动任何入口。

依赖链：GUI/CLI → objective_info_preset → 核心行情模块 → p0_data_quality / quality_time_semantics / pretrade_feature_builder；券商文本由broker_text_import、broker_text_pdf辅助。Python便携运行时与pandas/akshare/tushare、PDF相关库按本机已有环境使用，安装版本本轮NOT_VERIFIED。

代码可见TuShare兼容客户端、AkShare与公开报价/日线回退路径；TuShare兼容入口不自动等于官方直连。历史分钟报告明确存在第三方兼容网关rt_min_daily；MeoZ模块已在独立云链核实，不能据此认定本地所有分钟也来自MeoZ。通达信providers/tdx_local属于独立只读历史研究，README明确未被正式日常链自动调用。

|输入|时点与质量处理|限制|
|---|---|---|
|日K/指数|核心模块日线请求、缓存、失败回退|原生日K与报价合成当日K应保留来源；最新报价不证明正式收盘K|
|5m/30m|原生分钟、缓存来源、期待bar与时间语义研究|跨边界缺末根要显式降级；历史bar可靠不代表最新完成bar已证|
|财务/公告/新闻|核心模块相关可选路径|权限、当前源可用性与覆盖未联网核验，不作SLA保证|
|券商截图/TXT|人工来源、解析、快照证据与警告|截图拍摄、OCR处理、本地mtime、券商导出/服务器时间分别记录；缺失不补造|

objective_info_preset中`publish`相关流程生成JSON/PDF：JSON必须含`bundle_identity.bundle_id`；PDF元数据/Subject对应BundleID，JSON记录PDF SHA256，`validate_json_pdf_pair`重新核验ID、pair_status和PDF哈希，配对后通过目录事务发布。`pair_status=verified`证明所验文件配对，不证明供应商数据最终性、券商余额或投资结论。不要将JSON的PDF哈希误写成双向循环哈希。

分钟质量需分四层阅读：fetch_status（取得与否）、bar_validity（行及单位是否有效）、path_freshness（截至冻结时点期待路径是否齐全新鲜）、execution_evidence（严格执行所需最终性/来源证据）。这些是核查维度，不保证每份旧包都包含同名顶层字段；字段必须按具体版本取证。源端bar时点、HTTP接收时点、整包冻结时点不得互换。35/35只是该次目标集合覆盖，不能外推每个历史卡点有效，更不证明供应商finality。

Task22报告仍为新1m协议NOT_RUN、Task20 0/18严格边界；10月23日V3.2为L1离线候选冻结、生产NO_GO。后续单股实测另有报告，不能把单股等同完整正式JSON/PDF验收。哪些修复已装须逐版本比对SHA，不能用最新文件mtime推断。固定算法标签属于工程产物；方向、PB、Forecast/Decision由GPT依据正式契约判断。

持仓/现金/成交/可卖旧仓/条件单证据应分别追溯券商原件；文件未提供、时间不明、成交链不全时保留UNKNOWN。程序净额推算、缓存、服务器时钟均不能替代券商事实。公开只给结构说明，例如`{"bundle_identity":{"bundle_id":"EXAMPLE_ONLY","pair_status":"UNVERIFIED_EXAMPLE"}}`，非真实包、非可导入协议；不发布账户字段实例。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
