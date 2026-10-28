# AI-OPS 交接试验入口（公开安全版）

状态：PILOT_ONLY / 2026-10-28。分支：`ai-handoff/pilot-20261028`。此目录尚未合并 main；绝不是正式部署、交易指令或自动任务配置。

## 参与者（统一称呼）
- **主GPT**：正式研究、跨项目规则核对、风险裁决与验收建议。
- **主Codex**：已有系统代码、部署与工程修复的原负责人。
- **宝宝GPT**：独立复核、读取交接包、发现矛盾后提出问题；不直接发布正式状态。
- **宝宝Codex**：新电脑/新账号的隔离验证与辅助维护；先审核、再在个人分支或fork提交补丁，禁止直接写主系统。

## 新会话只读恢复顺序
1. 读取本文件并记下分支名和实际 Git SHA。
2. 读取 `STATUS_20261028.md`，区分 VERIFIED_FILE / PRIOR_REPORTED / NEEDS_RECHECK。
3. 读取 `tasks/BABY-CODEX-001.md`，先回述任务、限制、数据来源与当前疑点。
4. 只读查看 main 上 `status.json`、`latest_1450.json`、`latest_exit_0950.json`、`experiment_status.json`。
5. 记录本次所读文件的取证 Git Commit SHA、Git Blob SHA、原始字节 SHA-256，以及实际交易日期、生成时间和运行模式的真实 JSON 路径；字段不存在写 `ABSENT`，存在但值为 `null` 时保留 `null`。只对独立试验任务作最小更新。
6. 工作结束回报：发现、改动、验证、证据、仍待人工确认。绝不能把读取成功或离线测试通过冒充生产执行许可。

## 字段路径与三种SHA的记录方式

逐份检查 `trade_date`、`run_context`、`generated_at_cn`，记录实际 JSON 路径和值，不假定它们都在顶层。例如 `experiment_status.json` 顶层字段缺失时，分别写 `$.trade_date=ABSENT`、`$.run_context=ABSENT`、`$.generated_at_cn=ABSENT`；若存在相关嵌套字段，再单列 `$.selection_audit.trade_date`、`$.selection_audit.market_data_health.A.run_context`、`$.selection_audit.market_data_health.BC.run_context` 的实际值。`checked_at_cn`、`retrieved_at_cn`、`last_verified_at_cn` 只按自身语义和真实路径记录，不替代缺失的生成时间。以上是路径示例，仍须核对当次原文件；`LIVE` 标签也不代表读取时刻的实时数据。

| 记录项 | 定位或校验对象 | 使用边界 |
| --- | --- | --- |
| Git Commit SHA | 一次提交，固定本轮完整仓库版本 | 先记录分支与解析出的实际提交SHA，再按该提交读取文件；分支名会移动，不能代替提交SHA |
| Git Blob SHA | 该提交树中某个路径对应的文件内容对象 | 记录文件路径与blob SHA，校验读取的字节属于该提交；blob不包含文件名，相同字节可出现在多个路径或提交中 |
| SHA-256 | 下载所得文件原始字节的内容摘要 | 用于比对JSON中的快照引用或独立校验下载内容；保持原始编码和换行，不在重新序列化JSON后计算并冒充原文件摘要 |

三者分别记录，不互相替代。Git Blob SHA包含Git对象头参与哈希，与对文件原始字节直接计算的SHA-256不是同一种值；Git对象ID使用仓库的对象格式，本仓库现有Commit/Blob SHA为40位SHA-1对象ID。结果文件自身的最终blob/SHA-256和Commit SHA放在提交后回执中，避免自引用。

## 严格边界
- 公开仓库**不得发布**任何券商账户/真实持仓明细、个人身份信息、API密钥、私有策略契约原文、原始预测台账。
- Astock「跑预设」与「尾盘隔夜A/B/C模拟」是两个项目体系；两者共享工程审计方法，但**不得合并交易状态、实验样本或执行授权**。
- 当前隔夜模拟属于研究；A/B/C分别保留样本，机器提案不等于GPT确认选股，更不等于模拟成交或真实收益。
- 不改 main 的行情 JSON、策略选择文件、历史账本、腾讯云脚本、计时器、自动化任务或实盘配置。
- 主GPT/宝宝GPT不得用记忆覆盖券商事实；宝宝Codex的核查结果只是待复核工程证据。
- 缺证据标 UNKNOWN / NEEDS_RECHECK，不能凭本文件将 NO_GO 升级。
- 任何写入先确认目标仓库、分支和目标路径。跨账号没有写权限时fork或只返回patch；不能索要主账号Token。

## 当前首个练习
`tasks/BABY-CODEX-001.md`：只核对三份公开JSON的日期/模式及实验隔离；在个人工作区完成**文档级**更新，不碰正式状态。

完整私人状态来源：ChatGPT「Astock跑预设」Project Sources中唯一active的规则契约与三份同轮同步滚动状态；由于此仓库公开，本试验不复制这些资料。
