# BABY-CODEX-003 冻结证据索引｜公开行情/实验JSON

source_repository: dna19901002-cyber/DNA-CLUB
captured_by: 主GPT GitHub连接器
capture_date_cn: 2026-10-28
not_an_account_snapshot: true
not_raw_meoz_response: true

## 固定版本

- 对照日（2026-10-27）GitHub commit: `a09787593e17a560e307647770a513009a9c581d`
- 当日（2026-10-28）GitHub commit: `b8d2ed00131a40b1277430c457d7f4861bb374f7`

| 时点 | 路径 | Git Blob SHA | 日期/数据模式 | 可验证结果 |
|---|---|---|---|---|
| 10/27 14:50 A | `latest_1450.json` @10/27提交 | `de00a96a95745c66d0e33b3eb77fa73467e6f27a` | 20261027 / LIVE | NO_TRADE / CORE_INDEX_GATE_FAILED；unit_validation.result=NOT_CHECKED，只有indexdaily请求15行记录，不得称单位PASS |
| 10/27 14:50 B/C | `latest_experiment_1450.json` @10/27提交 | `9a47357c65b40c4955b2e71769d5b762ccd2096b` | 20261027 / LIVE_EXPERIMENT | screening 5656行，unit_validation.PASS，checked_rows=5568；B=NO_TRADE；C=机器观察提案 |
| 10/28 14:30 A | `latest_1430.json` @10/28提交 | `7e43faf7e657ab5b841ff18b1d487260e8eddb57` | 20261028 / LIVE | DATA_INSUFFICIENT / SCREENING_UNIT_VALIDATION_NOT_COMPLETED；screening HTTP200 5656行 |
| 10/28 14:50 A | `latest_1450.json` @10/28提交 | `420cbffd86c9085087b20503106e3cf4bbad1f31` | 20261028 / LIVE | DATA_INSUFFICIENT / SCREENING_UNIT_VALIDATION_NOT_COMPLETED；screening HTTP200 5656行 |
| 10/28 14:50 B/C | `latest_experiment_1450.json` @10/28提交 | `12052718a93020ce4bcd6c528556478aafdb68d6` | 20261028 / LIVE_EXPERIMENT | data_quality.DEGRADED；B/C=DATA_INSUFFICIENT；screening_source=EXACT_A_1450_SAVED_ROWS |
| 10/28实验状态 | `experiment_status.json` @10/28提交 | `a318af5b71322ce6a3585d82827bd8b7391bdf00` | latest_experiment_trade_date=20261028 | selection_workflow.mode=CONVERSATION_MANUAL_CHECKPOINT |

## 10/28 核心字段（14:30与14:50都存在）

`$.data_quality.unit_validation`:
- `result=NOT_CHECKED`
- `validated_at=null`
- `checked_rows=0`
- `detail=REQUIRED_AMOUNT_OR_VOLUME_MISSING`
- `schema_signature=e062261fe7bdf4feeebe7a09b4636e09292353869d2d39cf43dfeacd45d4159b`
- `amount_unit=yuan` 和 `volume_unit=lots_100_shares` 是声明值，**不能替代对原始行的确认**。

### 14:30 A
- `$.generated_at_cn=2026-10-28T14:30:14+08:00`
- `$.data_quality.request_summary[*]`：stockbasic HTTP200 rows5656、indexdaily HTTP200 rows15、screening HTTP200 rows5656。
- `$.market_gate.ma5=true`；`turnover=null`、`advance_ratio=null`。

### 14:50 A
- `$.generated_at_cn=2026-10-28T14:50:09+08:00`
- `$.data_quality.request_summary[*]`：indexdaily HTTP200 rows15、screening HTTP200 rows5656。
- `$.market_gate.ma5=true`；`turnover=null`、`advance_ratio=null`。

### 14:50 B/C
- `$.data_quality.screening_source=EXACT_A_1450_SAVED_ROWS`
- `$.data_quality.issues` 包含 `SCREENING_UNIT_VALIDATION_NOT_COMPLETED`
- 两个组 `decision=DATA_INSUFFICIENT`，没有可证实的有效GPT选择或模拟成交。

## 限定解释
这些是**GitHub已发布的程序摘要**，本次没有取到10/28原始screening行、映射函数源码、供应商协议原始payload或可独立复放的fixture。因此 `REQUIRED_AMOUNT_OR_VOLUME_MISSING` 只证明校验器给出了该错误标签，**不证明供应商原始响应必然缺字段**。HTTP200、5656 rows及全市场覆盖也不能证明单位字段符合要求。

10/27 B/C的PASS与10/28 A失败之间存在可调查差异，但来源/采样/验证路径可能不同；需要主Codex拿本地证据逐级复核，不得拍脑袋修复。

锁定出处：
- [10/27 A](https://github.com/dna19901002-cyber/DNA-CLUB/blob/a09787593e17a560e307647770a513009a9c581d/latest_1450.json)
- [10/27 B/C](https://github.com/dna19901002-cyber/DNA-CLUB/blob/a09787593e17a560e307647770a513009a9c581d/latest_experiment_1450.json)
- [10/28 14:30 A](https://github.com/dna19901002-cyber/DNA-CLUB/blob/b8d2ed00131a40b1277430c457d7f4861bb374f7/latest_1430.json)
- [10/28 14:50 A](https://github.com/dna19901002-cyber/DNA-CLUB/blob/b8d2ed00131a40b1277430c457d7f4861bb374f7/latest_1450.json)
- [10/28 B/C](https://github.com/dna19901002-cyber/DNA-CLUB/blob/b8d2ed00131a40b1277430c457d7f4861bb374f7/latest_experiment_1450.json)

核证条件：只读、禁新增API调用、无资金动作、无自动策略变更。若当日GitHub主分支继续更新，须保留固定证据与新数据并列，不得静默覆盖。
