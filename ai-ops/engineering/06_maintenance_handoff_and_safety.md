# 维护交接与安全边界

updated_at: 2026-10-28T19:02:46+08:00
source_commit: 0e345374a190d9bd3fb57419381284f87fc8ecc8 (本机工作副本，逐文件SHA为准)
owner: 主Codex（工程）/用户与主GPT（策略与状态）
evidence_level: VERIFIED_CODE + PRIOR_REPORTED；无本轮 VERIFIED_RUNTIME
next_recheck: 下一次正式验收；云端修复另待下一真实交易日

|环境|维护人|依赖/边界|
|---|---|---|
|本地客观导出器|主Codex|Python便携运行时、供应器、PDF与人工券商事实；只导出客观资料|
|本地Monitor|主Codex；用户授权提醒|接收器/生命周期/既定报价与通知；不下单|
|腾讯云尾盘服务|主Codex受控部署|Python/systemd/供应器/GitHub写入；本次不可变更|
|GitHub工程文档|主Codex维护；宝宝Codex隔离审计|公开脱敏信息；独立分支，不自动合并main|
|GPT私人Sources|用户和主GPT|唯一active契约与三状态；Codex无权改写|

阅读顺序：总览→组件架构→逐文件SHA/原回执→风险与未知→获得对应批准后独立测试。只读审计可以检查公开文档/已有字节，受控生产变更另须实际版本、保护清单、测试和部署验收。宝宝Codex可在隔离分支审计补证，不同时修改生产源文件、不重拉全市场、不抢供应商配额、不修改云timer或越过主Codex部署。

本轮发布仅ai-ops/engineering/，不复制源码、原配置、.env、私钥、账户/持仓、截图、Forecast或供应商原始数据。源路径为脱敏相对定位索引，源文件多数不在公开仓库，不要当作可点击下载地址。公网只交付SHA和说明，原件由用户另行私下授权审阅。

架构只在正式修复/部署/能力变更验收后更新对应证据与版本；未验收修复只能记reported，不抬升能力。保留updated_at、source_commit、owner、evidence_level、next_recheck。日报与架构分开；旧回执不覆盖、不把报道标签改成现场验证。

缺口：私人GPT契约与三状态NOT_ACCESSIBLE；云端当前进程、部署字节、timer、PAT有效期和告警NOT_ACCESSIBLE；另一电脑工作区NOT_ACCESSIBLE。需要维护者提供脱敏部署/运行证据，以及用户或主GPT提供获准公开的角色索引确认；不要求公开私人原文或凭据。

[返回总入口](README.md) · [证据索引](architecture_evidence_index.json)
