# Codex 下一步优化任务清单

## 实现 pair_attribution.json 与 stoploss/ROI 归因

- 优先级：high
- 问题证据：多 run 出现 ROI 为正但 fixed stoploss 绝对亏损更大；pair_leaderboard 不能稳定回答哪一币造成大亏。
- 修改目标：每个 run 输出 pair/month/exit_reason 维度归因。
- 具体修改点：
  - 记录 pair、month、entry_tag、exit_reason、profit_abs、profit_pct
  - 聚合 roi_profit_abs、stoploss_profit_abs、trailing_profit_abs
  - 在 summary 写 worst_pairs_by_stoploss
- 验收标准：任意 run 结束后存在 pair_attribution.json，且能列出每个 pair 的 ROI/SL/trailing 贡献。

## 重写 strategy_family_leaderboard 统计和失败降权

- 优先级：high
- 问题证据：breakout_momentum 高频/亏损后仍被推荐；family_leaderboard 缺少 high_frequency_failure/zero_trade/runtime_error 准确计数。
- 修改目标：family 推荐必须由 round_history 真实聚合驱动。
- 具体修改点：
  - 从 round_history 统一统计 generated/valid/near_miss/high_frequency/zero/runtime
  - 高频或 runtime family 冷却 N 个 run
  - preferred_families_for_next_optimize 输出推荐原因和禁用原因
- 验收标准：构造含 0 交易、高频、runtime 的 summary 后，leaderboard 计数完全匹配。

## 实现 near-miss follow-up 机制

- 优先级：high
- 问题证据：最接近成功策略常因 stoploss_to_roi 过高或单月验证亏损失败，下一轮未定向修复。
- 修改目标：让 near-miss 自动成为受控 parent/mutation 来源。
- 具体修改点：
  - 保存 nearest_candidate 的失败标签
  - 风控型 near-miss 生成 tighten_stoploss/entry_filter mutation
  - 验证期好但训练少的候选生成 widen_entry_controlled mutation
- 验收标准：下一轮 prompt 明确引用上一轮 near-miss run/version/指标和修复方向。

## parent selection 可审计化

- 优先级：medium
- 问题证据：pre_run 推荐 parent 与 actual_session_parent 是否一致无法稳定审计。
- 修改目标：summary 中明确 historical_best/nearest/pre_run/actual 的选择链。
- 具体修改点：
  - 记录 pre_run_recommended_parent
  - 记录 actual_session_parent_source
  - 若不采用 pre_run 推荐，写 reason
- 验收标准：每个 run 的 last_run_summary 能回答 actual_session_parent 是否生效。

## runtime static check 拦截 Bollinger/字段 KeyError

- 优先级：medium
- 问题证据：日志需求指出曾有 Bollinger 字段 KeyError；运行时才失败浪费迭代。
- 修改目标：代码生成后、回测前做 dataframe 字段静态/轻量动态检查。
- 具体修改点：
  - AST 扫描 df["col"] 使用
  - 检查 indicator 创建顺序
  - 用最小 dataframe smoke test populate_indicators
- 验收标准：含未创建 bb_lowerband 字段的策略在 static_check 阶段失败，failure_reason 清晰。

## scoring 增加 worst-month 与 validation 高频惩罚

- 优先级：high
- 问题证据：202603/202604 验证经常拖累，202605 训练接近导致过拟合。
- 修改目标：final_score 更重视最差验证月份。
- 具体修改点：
  - 加入 min(validation_profit_pct) 惩罚
  - 加入 validation_total_trades > limit invalid
  - 加入 stoploss_to_roi_ratio 惩罚
- 验收标准：一个 202605 盈利但 202603/202604 明显亏损的策略 final_score<=0 且不可 official best。
