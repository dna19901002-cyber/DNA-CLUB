# BABY-CODEX-001 | 跨账号理解与最小维护试验

status: READY_FOR_BABY_CODEX
write_read_test: VERIFIED_BY_MAIN_GPT_20261028
scope: PUBLIC_DOC_ONLY
owner: 宝宝Codex（尚未实际认领）
reviewer: 主GPT/宝宝GPT（验收建议），人工决定合并
created: 2026-10-28
branch: ai-handoff/pilot-20261028

## 背景
新电脑、新Codex账号能否不借助主账号私人对话，单独理解“跑预设”与“尾盘隔夜A/B/C”两项目的区别；会不会误把旧selection、机器观察提案和生产数据质量状态混为一谈。本项目只测试文档交接，没有交易执行。

## 请宝宝Codex完成的四件事
1. **先不修改任何文件**：依次阅读 `ai-ops/README.md`、`ai-ops/STATUS_20261028.md`、`ai-ops/AGENTS.md` 和本任务，并向用户回述：两个系统的目标、3个已核事实、3个仍待验问题、不可碰的文件和为什么不得下交易指令。
2. **独立复核**：固定并记录 `main` 的实际 Git Commit SHA，只读查看该提交中的 `latest_1450.json`、`experiment_status.json` 和 `latest_experiment_1450.json`。逐份记录 `trade_date`、`run_context`、`generated_at_cn` 的真实 JSON 路径和值；字段不存在写 `ABSENT`，存在但值为 `null` 时保留 `null`。顶层缺失时不得用嵌套字段冒充顶层值，可另列相关字段的真实路径和值；审计/检查时间、抓取时间不能替代生成时间。例如 `experiment_status.json` 可另列 `$.selection_audit.trade_date`、`$.selection_audit.market_data_health.A.run_context`、`$.selection_audit.market_data_health.BC.run_context` 和 `$.selection_audit.checked_at_cn`，仍以当次原文件实际存在的字段为准。另记每份文件路径对应的 Git Blob SHA、原始字节 SHA-256，不与 Commit SHA 混用。特别区分：A组 `NO_TRADE`、B组 `NO_TRADE`、C组机器观察提案≠GPT确认交易。若文件被更新，以新事实为准并标明原索引过期。
3. **执行最小维护练习**：在你自己的隔离分支或fork下，新建 `ai-ops/tasks/BABY-CODEX-001-RESULT.md`，只记录上述核查回执、发现的1个文档小问题及建议修改方案。不得编辑根目录JSON、正式策略或当前主分支。若无提交权限，只输出diff和回执。
4. **发送回主GPT**：提供Commit SHA、分支、URL、修改文件、测试/读回记录、权限问题和仍未解决事项。禁止把文件写入当成已部署/已验收。

## 验收标准
- 能正确解释两个不同股票系统的边界；
- 能区分历史记录、实时数据、机器候选、GPT判断、真实成交；
- 准确复述 `CONVERSATION_MANUAL_CHECKPOINT` 不意味着GitHub行情坏了；
- 仅新增一个结果文档，无生产文件改变；
- 可回读结果文档且给出实际SHA、Commit；
- 独立报告任何文档不一致，而不是自动修复已核正式状态。

## 禁止事项
不调用MeoZ/TuShare；不触发任何定时任务、选股写入或券商操作；不修改A/B/C策略、不推断当日模拟成交；不请求主账户Token；不公布私有交易信息。

## 当前主GPT侧初始回执
- 主GPT已准备此测试任务；宝宝Codex尚未执行。
- 任务处于 READY_FOR_BABY_CODEX，非完成状态。

## BABY-CODEX-002 文档修订记录（2026-10-28）

本次仅澄清第2项的字段路径、缺失值和SHA记录规则；上方状态、owner和“当前主GPT侧初始回执”保留为任务创建时的历史记录，不代表本次执行进度。首次实际执行记录仍见 `BABY-CODEX-001-RESULT.md`，本次不修改该文件或已出具的首次外部回执。人工技术审阅情况不在本任务中改写为合并或生产验收。

修订前版本保存在 Git Commit `4348786f33f7b7b31fd12fe2d9e893ae29ce77cc`（任务文件blob为 `92385e2c099697b4cc58adc84ddc821ea10de442`）；其中第2项原文保留如下：

> 2. **独立复核**：只读查看 `main` 的 `latest_1450.json`、`experiment_status.json` 和 `latest_experiment_1450.json`，记录每份 `trade_date`、`run_context`、SHA。特别区分：A组 `NO_TRADE`、B组 `NO_TRADE`、C组机器观察提案≠GPT确认交易。若文件被更新，以新事实为准并标明原索引过期。
