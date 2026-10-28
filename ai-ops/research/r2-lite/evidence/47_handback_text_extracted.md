<!-- PROVENANCE: extracted_text_from_ChatGPT_library_handback_for_gpt(2).md; original_binary_and_47_test_log_NOT_AVAILABLE; NOT_INDEPENDENT_TEST_VALIDATION -->
# R2-Lite本地前瞻证据接入候选｜隔离验收

任务于2026-10-27开始，2026-10-28完成。**47/47新增离线测试通过；未安装、未启用、未新增行情、通知、任务或订单。有效前瞻样本仍为0。** 本轮未重跑六日历史回放、72项旧协议测试或V9监控测试。

独立解包复跑结果见results/portable_verification.json。测试已处理Windows深层解包路径长度限制，使用本机生成的隔离路径，不依赖旧归档Windows路径；Linux服务器真实运行仍未验证。

| 最终问题 | 裁决 |
|---|---|
| 能否零新增行情获得14:52前读取/冻结凭据？ | 工程上可复用本地完整1450归档；候选记录真实读取和子进程完成时刻，超时拒绝。真实服务器时序尚未验证，不能保证下一次一定完成。 |
| 是否需要变更人工选择政策？ | 仅留痕不需要；要取消每日人工selected并自动冻结合格原C，需要单独批准AUTO_ACCEPT_ELIGIBLE_NATIVE_C。默认仍为MANUAL_SELECTED_V06且关闭。 |
| 09:50/09:55能覆盖谁？ | 09:50只复用A已有分钟，只有实际重合代码才可能覆盖。09:55现有同一批请求已包含原机器C最多2只；还受原配额、错误和完整性限制。 |
| 最小生产接入缺什么？ | 当前服务器源码/单元实时复核、代码及控制文件安装批准、候选政策批准、服务完成尾段最多1.6秒等待批准、唯一真实交易日及第三次机会授权。 |
| 能否申请一次真实日受控验收？ | 可提交附条件申请；**当前还不能执行，也不能宣称端到端READY**。先关闭下述生产运行阻断，再决定是否消耗第三次机会。 |

## 实际补丁与保护

只在隔离副本的experiment_jobs.py新增两个异常隔离调用：完整C的1450.json原子落盘之后、现有exit结果落盘之后。移除这两个可选调用块后，AST与原源码完全一致；原请求/筛选/交易逻辑未改。保留并调用原V0.4 Recorder.freeze及纯验证函数，三份原协议源码逐字节不变。

目标原SHA：`44a5ab7f17a70f64ab59ddf530635d601a967d13a1134ea111c44182f94b5c64`；候选SHA：`28993ff5e422174a564ec8e89fa0803962b560bc647401045f83555b4760944f`。本机当前SHA与10月27日18:49服务器封存证据一致；本轮没有可用远程会话，**不把封存证据当成当前服务器版本或进程状态**。本轮所核对300/300项本地保护文件SHA一致；历史保护基线未重设。

## 时序与生命周期

研究子进程只读完整归档，不使用缩减machine_proposals替代。保存原始字节SHA、日历SHA、真实读取起止、该固定研究读取器首次读取时刻、固定规则拒绝原因和冻结候选。原V0.4记录仅作临时核心产物，必须再有supervisor完成收据才能接纳。最终frozen_at使用实际观察到子进程退出并完成写盘后的时刻上界，不用mtime、Git提交或生成时间代替读取。

发现原collect单元为oneshot。仅start_new_session不会脱离systemd控制组，父服务退出可能清理研究子进程。候选因此加入**独立授权门**：在原main已释放API锁、完成原输出后，通过atexit最多等待自身研究子进程1.6秒；不等待其它进程、不新增service/timer、不改KillMode。未经这项授权不启动研究分支。该尾段会影响服务结束时刻，不能描述为零额外耗时或已验证不影响OnSuccess。

本机Windows测量：关闭态hook中位0.314ms；空Python进程Popen父侧中位25.29ms、最大44.70ms；原记录器TEST时钟落盘中位81.97ms。均非Linux生产测速，不含真实NTP探测及调度拥塞。合作式工作预算1秒，独立supervisor对自身worker超时上限1.5秒；操作系统创建进程和磁盘卡顿不能承诺硬实时零延迟。

systemd依据：[官方systemd.kill](https://www.freedesktop.org/software/systemd/man/latest/systemd.kill.html)、[官方systemd.service](https://www.freedesktop.org/software/systemd/man/259/systemd.service.html)。本轮没有查询或更改服务器单元；归档单元仅支持候选设计，真实有效配置仍需复核。

## 失败测试与未完成项

测试覆盖：截止后、写盘跨截止、旧日、缩减文件、参考价冲突、日历SHA失配、时钟正/反跳、无同步证明、IO失败、真实测试子进程退出、重复读取/更换候选、审批SHA不匹配、未批准自动选择、未批准退出等待，以及原collect/exit异常后继续。原请求代码静态相同，注入相同输入的实际函数输出与请求列表相同。测试依赖的provider、时钟、业务辅助和存储适配有桩，不能宣称已覆盖生产所有故障或Linux系统断电。

09:50不补09:55字段，缺根、重复及缺代码不补写。即使已有完整标签，价格基准与供应商时间契约未认证仍DATA_INSUFFICIENT，所有参考变化及收益字段保持null；不会调用原observe制造完整结果。现有MeoZ保存的是允许字段的解析行，HTTP响应正文未留存，不能将parsed rows宣称为完整供应商原始HTTP包。

## 三次机会与最终状态

原10月26、27日两次机会和失败记录按SHA封存，opportunity_actuals.csv仅有这两行实际记录。下一次若获批是第3次，不是新计数起点。本轮测试在TEST_PROTOCOL_ONLY目录，不计机会或样本。达到原停止条件后停止自动集成研究，不能再无条件增加第4次；发布/执行方式变化须有可验证证据和独立复核。

R2_LITE_PROTOCOL=READY；R2_LITE_END_TO_END=NOT_READY；R2_STRICT=NOT_READY；HISTORICAL_RETURN_BACKTEST=NO_GO；PRODUCTION_INTEGRATION=NOT_AUTHORIZED。当前只有申请资格，未授权任何生产接入或自动选择。

另纠正复核文字中的名称：688151为华强科技，奥比中光代码为688322；既有3个历史通过代码不变，本轮没有重做历史结论。
