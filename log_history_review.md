# Freqtrade-AI 历史运行日志复盘

- 仓库：https://github.com/b8vipvip/freqtrade-ai-logs
- 复盘时间：2026-05-31T15:01:37.271716Z
- 数据源说明：git clone GitHub failed in this environment with CONNECT tunnel 403 / DNS failure; analysis used /workspace/freqtrade-ai-logs copied to /tmp as fallback, not /opt/freqtrade-ai-logs.
- 分析 run 数：63；策略轮数：315；缺失文件统计：{'goal.runtime.json': 2, 'last_run_summary.json': 1, 'iteration_stats.json': 13, 'leaderboard.json': 14, 'strategy_family_leaderboard.json': 53, 'pre_run_ai_review.json': 41, 'recommended_pairs.json': 32, 'pair_leaderboard.json': 32, 'pair_strategy_vote.json': 50}

## 总结

- 历史最值得继续的父策略：优先使用 nearest_candidate 中的风控型 near-miss，而不是盲目 historical_best；当前可解析数据里训练正收益策略常被 stoploss 吞噬，说明需要围绕止损/过滤改良。
- 当前最值得继续的 family 是 trend_following；breakout_momentum 与 strict_risk_filter 建议暂停；pullback_reversal 需要放宽但加交易数上限；low_volatility_mean_reversion 可小样本继续。
- 最大共性问题是 ROI 小盈利被固定止损吞噬、训练期 202605 接近但 202603/202604 验证差、family 失败后降权和统计不够可靠。

## 时间线

### 20260529_102352 — `20260529_102352`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=None ideal_min=None ideal_max=None max=None
- 父策略：historical_best=None；nearest_candidate=None；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=n/a

### 2026-05-29T13:24:07.413561 — `20260529_132126`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260529_102352_v031；nearest_candidate=None；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=交易数偏低; 验证区间表现不稳定; 最差验证月份拖累整体表现

### 2026-05-29T13:24:07.413561 — `20260530_000822`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260529_102352_v031；nearest_candidate=None；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=交易数偏低; 验证区间表现不稳定; 最差验证月份拖累整体表现

### 2026-05-30T00:16:45.349235 — `20260530_001414`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260529_102352_v031；nearest_candidate=MultiCoin_AI_Strategy_20260530_001414_v001；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=验证区间表现不稳定; 最差验证月份拖累整体表现

### 2026-05-30T00:22:45.312677 — `20260530_001814`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260529_102352_v031；nearest_candidate=MultiCoin_AI_Strategy_20260530_001814_v003；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=交易数偏低; 验证区间全部亏损; 固定止损吞噬 ROI

### 2026-05-30T00:35:33.376653 — `20260530_003430`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260529_102352_v031；nearest_candidate=None；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=PF 低

### 2026-05-30T00:47:19.560861 — `20260530_004239`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=n/a
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_004239_v002；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=None
- 本 run 无可解析 round_history。
- near-miss=False；更新 official best=False；主要失败=验证区间全部亏损; 固定止损吞噬 ROI

### 2026-05-30T01:12:39.787591 — `20260530_010736`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_010736_v002；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_004239_v002
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 47 | 0.071 | 1.054 | 0.925 | 12.971 | -13.175 | 0.911 | -12.282 | False | False | final_score<=0 |
| v002 | unknown | pair_specific_filter | 53 | -0.050 | 0.969 | 1.162 | 15.053 | -16.467 | 0.911 | 18.896 | True | True | 通过 |
| v003 | unknown | tighten_entry_trigger | 15 | -0.328 | 0.501 | 0.627 | 3.303 | -6.588 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=True；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数低于目标下限; 验证区间表现不稳定

### 2026-05-30T03:04:58.319540 — `20260530_014158`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_014158_v045；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_010736_v002
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 25 | 0.210 | 1.318 | 0.566 | 8.191 | -6.588 | 0.494 | 24.578 | True | True | 通过 |
| v002 | unknown | add_entry_filter | 16 | 0.241 | 1.731 | 0.268 | 5.208 | -3.293 | 0.494 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v003 | unknown | pair_specific_filter | 16 | 0.241 | 1.731 | 0.268 | 5.208 | -3.293 | 0.494 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v004 | unknown | adjust_roi | 25 | 0.025 | 1.038 | 0.596 | 5.598 | -6.588 | 1.238 | 23.542 | True | False | 通过 |
| v005 | unknown | adjust_stoploss | 25 | 0.252 | 1.504 | 0.407 | 7.011 | -4.988 | 0.493 | 20.946 | True | False | 通过 |
| v006 | unknown | adjust_roi | 25 | -0.070 | 0.893 | 0.612 | 4.645 | -6.588 | 1.238 | -6.428 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v007 | unknown | tighten_entry_trigger | 23 | -0.293 | 0.691 | 0.824 | 6.551 | -9.480 | 0 | -8.420 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v008 | unknown | pair_specific_filter | 27 | 0.075 | 1.091 | 0.700 | 8.492 | -8.235 | 0.494 | 22.630 | True | False | 通过 |
| v009 | unknown | adjust_stoploss | 25 | 0.295 | 1.528 | 0.466 | 8.191 | -5.587 | 0.344 | -8.204 | False | False | final_score<=0 |
| v010 | unknown | disable_or_adjust_trailing | 25 | 0.092 | 1.139 | 0.566 | 7.011 | -6.588 | 0.493 | 24.905 | True | True | 通过 |
| v011 | unknown | pair_specific_filter | 16 | 0.236 | 1.716 | 0.268 | 5.208 | -3.293 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v012 | unknown | remove_bad_entry_condition | 22 | 0.221 | 1.448 | 0.402 | 6.710 | -4.940 | 0.444 | 24.190 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v013 | unknown | cooldown_or_protection | 25 | 0.092 | 1.139 | 0.566 | 7.011 | -6.588 | 0.493 | 24.905 | True | False | 通过 |
| v014 | unknown | pair_specific_filter | 17 | 0.071 | 1.144 | 0.432 | 5.208 | -4.939 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v015 | unknown | tighten_volume_filter | 23 | 0.226 | 1.457 | 0.402 | 6.706 | -4.940 | 0.493 | 24.886 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v016 | unknown | cooldown_or_protection | 25 | 0.092 | 1.139 | 0.566 | 7.011 | -6.588 | 0.493 | 24.905 | True | False | 通过 |
| v017 | unknown | pair_specific_filter | 22 | 0.001 | 1.002 | 0.596 | 6.109 | -6.588 | 0.493 | 23.799 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v018 | unknown | cooldown_or_protection | 25 | 0.092 | 1.139 | 0.566 | 7.011 | -6.588 | 0.493 | 24.905 | True | False | 通过 |
| v019 | unknown | pair_specific_filter | 18 | 0.101 | 1.205 | 0.432 | 5.508 | -4.940 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v020 | unknown | cooldown_or_protection | 41 | -0.086 | 0.935 | 0.985 | 11.827 | -13.177 | 0.493 | 21.354 | True | False | 通过 |
| v021 | unknown | cooldown_or_protection | 24 | 0.087 | 1.132 | 0.596 | 7.014 | -6.587 | 0.444 | 20.344 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v022 | unknown | disable_or_adjust_trailing | 25 | -0.082 | 0.876 | 0.566 | 4.805 | -6.588 | 0.967 | -6.330 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v023 | unknown | pair_specific_filter | 28 | -0.013 | 0.984 | 0.700 | 7.611 | -8.235 | 0.493 | 22.675 | True | False | 通过 |
| v024 | unknown | reduce_trade_frequency | 27 | -0.043 | 0.948 | 0.700 | 7.311 | -8.235 | 0.493 | 22.618 | True | False | 通过 |
| v025 | unknown | pair_specific_filter | 24 | 0.087 | 1.131 | 0.536 | 7.010 | -6.588 | 0.444 | 24.343 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v026 | unknown | reduce_trade_frequency | 25 | 0.092 | 1.139 | 0.566 | 7.011 | -6.588 | 0.493 | 24.905 | True | False | 通过 |
| v027 | unknown | pair_specific_filter | 16 | 0.236 | 1.716 | 0.268 | 5.208 | -3.293 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v028 | unknown | reduce_trade_frequency | 40 | -0.385 | 0.740 | 1.248 | 10.478 | -14.825 | 0.493 | -9.034 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v029 | unknown | pair_specific_filter | 27 | -0.043 | 0.948 | 0.700 | 7.311 | -8.235 | 0.493 | 25.339 | True | True | 通过 |
| v030 | unknown | pair_specific_filter | 16 | 0.236 | 1.716 | 0.268 | 5.208 | -3.293 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v031 | unknown | tag_specific_filter | 27 | -0.043 | 0.948 | 0.700 | 7.311 | -8.235 | 0.493 | 23.158 | True | False | 通过 |
| v032 | unknown | pair_specific_filter | 27 | -0.043 | 0.948 | 0.700 | 7.311 | -8.235 | 0.493 | 23.158 | True | False | 通过 |
| v033 | unknown | tag_specific_filter | 22 | 0.246 | 1.498 | 0.348 | 6.956 | -4.940 | 0.444 | 24.884 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v034 | unknown | pair_specific_filter | 16 | 0.041 | 1.083 | 0.433 | 4.906 | -4.940 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v035 | unknown | pair_specific_filter | 37 | -0.107 | 0.907 | 1.001 | 9.961 | -11.530 | 0.493 | 21.882 | True | False | 通过 |
| v036 | unknown | pair_specific_filter | 35 | 0.027 | 1.028 | 0.838 | 9.661 | -9.882 | 0.493 | 23.027 | True | False | 通过 |
| v037 | unknown | pair_specific_filter | 17 | 0.071 | 1.144 | 0.432 | 5.208 | -4.939 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v038 | unknown | pair_specific_filter | 46 | -0.045 | 0.968 | 1.023 | 12.625 | -13.173 | 0.837 | 18.322 | True | False | 通过 |
| v039 | unknown | pair_specific_filter | 38 | -0.084 | 0.925 | 0.929 | 10.339 | -11.179 | 0 | 21.083 | True | False | 通过 |
| v040 | unknown | pair_specific_filter | 28 | -0.013 | 0.984 | 0.700 | 7.611 | -8.235 | 0.493 | 23.738 | True | False | 通过 |
| v041 | unknown | pair_specific_filter | 16 | 0.236 | 1.716 | 0.268 | 5.208 | -3.293 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v042 | unknown | adjust_roi | 30 | -0.128 | 0.817 | 0.646 | 5.708 | -6.985 | 0 | -7.697 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v043 | unknown | pair_specific_filter | 18 | 0.101 | 1.205 | 0.432 | 5.508 | -4.940 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v044 | unknown | adjust_roi | 29 | -0.218 | 0.718 | 0.711 | 4.861 | -7.734 | 0.693 | -8.124 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v045 | unknown | pair_specific_filter | 25 | 0.002 | 1.003 | 0.665 | 7.011 | -7.583 | 0.593 | 26.078 | True | True | 通过 |
| v046 | unknown | pair_specific_filter | 28 | -0.128 | 0.865 | 0.824 | 7.611 | -9.480 | 0.593 | -6.255 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v047 | unknown | pair_specific_filter | 17 | 0.167 | 1.390 | 0.367 | 5.512 | -4.285 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v048 | unknown | adjust_roi | 29 | -0.168 | 0.704 | 0.547 | 3.856 | -5.686 | 0.149 | -37.423 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v049 | unknown | pair_specific_filter | 16 | 0.186 | 1.492 | 0.318 | 5.208 | -3.789 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v050 | unknown | adjust_roi | 29 | -0.219 | 0.614 | 0.547 | 3.344 | -5.686 | 0.149 | -7.909 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=True；主要失败=final_score<=0; 交易数偏低; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，仅作为候选参考; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-30T03:28:22.946333 — `20260530_031150`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_031150_v004；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_014158_v045
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 27 | -0.158 | 0.834 | 0.824 | 7.311 | -9.480 | 0.593 | -4.466 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | tighten_entry_trigger | 8 | -0.199 | 0.476 | 0.348 | 1.802 | -3.789 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | adjust_roi | 25 | -0.167 | 0.780 | 0.726 | 5.911 | -7.583 | 0 | -6.180 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v004 | unknown | add_entry_filter | 25 | 0.002 | 1.003 | 0.665 | 7.011 | -7.583 | 0.593 | 24.975 | True | True | 通过 |
| v005 | unknown | pair_specific_filter | 28 | -0.128 | 0.865 | 0.824 | 7.611 | -9.480 | 0.593 | -7.080 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v006 | unknown | tag_specific_filter | 19 | 0.041 | 1.073 | 0.477 | 5.506 | -5.686 | 0.593 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v007 | unknown | adjust_stoploss | 25 | 0.345 | 1.617 | 0.466 | 9.034 | -5.587 | 0 | -7.854 | False | False | final_score<=0 |
| v008 | unknown | cooldown_or_protection | 25 | 0.002 | 1.003 | 0.665 | 7.011 | -7.583 | 0.593 | 24.975 | True | False | 通过 |
| v009 | unknown | pair_specific_filter | 16 | 0.186 | 1.492 | 0.318 | 5.208 | -3.789 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v010 | unknown | remove_bad_entry_condition | 37 | -0.446 | 0.706 | 1.303 | 10.261 | -15.167 | 0.444 | -10.355 | False | False | 固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=True；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数低于目标下限; 验证区间表现不稳定

### 2026-05-30T04:05:09.621424 — `20260530_033035`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_033035_v015；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_031150_v004
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 27 | -0.158 | 0.834 | 0.824 | 7.311 | -9.480 | 0.593 | -7.174 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | tighten_entry_trigger | 18 | -0.193 | 0.700 | 0.566 | 4.008 | -6.432 | 0.494 | 0.000 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | adjust_roi | 25 | -0.199 | 0.738 | 0.695 | 4.902 | -7.583 | 0.693 | -7.864 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v004 | unknown | adjust_stoploss | 25 | 0.284 | 1.509 | 0.466 | 8.191 | -5.587 | 0.238 | -7.946 | False | False | final_score<=0 |
| v005 | unknown | add_entry_filter | 18 | 0.202 | 1.532 | 0.318 | 5.212 | -3.789 | 0.593 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v006 | unknown | remove_bad_entry_condition | 16 | 0.186 | 1.492 | 0.318 | 5.208 | -3.789 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v007 | unknown | cooldown_or_protection | 28 | -0.128 | 0.865 | 0.824 | 7.611 | -9.480 | 0.593 | -7.080 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v008 | unknown | tag_specific_filter | 16 | 0.186 | 1.492 | 0.318 | 5.208 | -3.789 | 0.444 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v009 | unknown | reduce_trade_frequency | 24 | 0.231 | 1.407 | 0.452 | 7.557 | -5.686 | 0.444 | 21.948 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v010 | unknown | tighten_volume_filter | 18 | 0.012 | 1.021 | 0.477 | 5.805 | -5.686 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v011 | unknown | pair_specific_filter | 37 | -0.272 | 0.795 | 1.145 | 9.961 | -13.273 | 0.593 | -8.171 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v012 | unknown | pair_specific_filter | 28 | -0.128 | 0.865 | 0.824 | 7.611 | -9.480 | 0.593 | -7.080 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v013 | unknown | pair_specific_filter | 22 | -0.048 | 0.937 | 0.641 | 6.658 | -7.579 | 0.444 | 20.943 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v014 | unknown | adjust_roi | 20 | 0.158 | 1.256 | 0.536 | 7.769 | -6.187 | 0 | 21.896 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v015 | unknown | pair_specific_filter | 25 | 0.003 | 1.004 | 0.665 | 7.462 | -7.583 | 0.149 | 25.599 | True | True | 通过 |
| v016 | unknown | pair_specific_filter | 25 | 0.003 | 1.004 | 0.665 | 7.462 | -7.583 | 0.149 | 25.599 | True | False | 通过 |
| v017 | unknown | disable_or_adjust_trailing | 27 | -0.167 | 0.824 | 0.824 | 7.763 | -9.480 | 0.050 | -7.057 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v018 | unknown | pair_specific_filter | 27 | -0.157 | 0.835 | 0.824 | 7.763 | -9.480 | 0.149 | -4.925 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v019 | unknown | pair_specific_filter | 22 | 0.043 | 1.075 | 0.491 | 5.670 | -5.686 | 0.444 | 22.955 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v020 | unknown | pair_specific_filter | 29 | -0.097 | 0.898 | 0.794 | 8.365 | -9.480 | 0.149 | 22.746 | True | False | 通过 |
- near-miss=True；更新 official best=True；主要失败=final_score<=0; 交易数偏低; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-30T04:26:36.978058 — `20260530_042343`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_033035_v015；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_033035_v015
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 21 | 0.118 | 1.207 | 0.476 | 6.861 | -5.686 | 0 | 27.207 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
- near-miss=True；更新 official best=False；主要失败=交易数偏低; 训练区间交易数略低于目标下限，仅作为候选参考

### 2026-05-30T05:06:30.390273 — `20260530_043127`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_043127_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_033035_v015
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | adjust_roi | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | True | 通过 |
| v002 | unknown | pair_specific_filter | 28 | -0.251 | 0.735 | 0.844 | 6.821 | -9.480 | 0.149 | -7.787 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v005 | unknown | tag_specific_filter | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
| v006 | unknown | remove_bad_entry_condition | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v007 | unknown | adjust_stoploss | 26 | -0.051 | 0.926 | 0.482 | 6.319 | -6.983 | 0.149 | 21.446 | True | False | 通过 |
| v008 | unknown | disable_or_adjust_trailing | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v009 | unknown | tighten_volume_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v010 | unknown | adjust_stoploss | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v011 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
| v012 | unknown | reduce_trade_frequency | 21 | -0.017 | 0.970 | 0.492 | 5.369 | -5.691 | 0.149 | 25.340 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v013 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
- near-miss=True；更新 official best=True；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数略低于目标下限，仅作为候选参考; 验证区间表现不稳定

### 2026-05-30T05:44:43.221646 — `20260530_052915`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_043127_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_043127_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 27 | -0.276 | 0.709 | 0.844 | 6.570 | -9.480 | 0.149 | -8.287 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | adjust_roi | 26 | -0.273 | 0.640 | 0.711 | 4.850 | -7.583 | 0 | -5.922 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v005 | unknown | remove_bad_entry_condition | 19 | 0.178 | 1.470 | 0.302 | 5.569 | -3.789 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 验证区间表现不稳定

### 2026-05-30T05:44:43.221646 — `20260530_071933`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_043127_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_043127_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 27 | -0.276 | 0.709 | 0.844 | 6.570 | -9.480 | 0.149 | -8.287 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | adjust_roi | 26 | -0.273 | 0.640 | 0.711 | 4.850 | -7.583 | 0 | -5.922 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v005 | unknown | remove_bad_entry_condition | 19 | 0.178 | 1.470 | 0.302 | 5.569 | -3.789 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 验证区间表现不稳定

### 2026-05-30T07:54:38.332189 — `20260530_075203`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_043127_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_043127_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 23 | -0.151 | 0.800 | 0.680 | 6.070 | -7.583 | 0 | -7.559 | False | False | 固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=交易数偏低; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-30T08:42:37.659487 — `20260530_075936`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=25 ideal_min=None ideal_max=None max=80
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_075936_v013；nearest_candidate=MultiCoin_AI_Strategy_20260530_004239_v003；actual_session_parent=MultiCoin_AI_Strategy_20260530_043127_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 23 | -0.161 | 0.787 | 0.680 | 5.819 | -7.583 | 0.149 | -6.960 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | adjust_roi | 27 | -0.346 | 0.544 | 0.717 | 4.125 | -7.583 | 0 | -7.785 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | tighten_entry_trigger | 20 | -0.037 | 0.935 | 0.491 | 5.317 | -5.686 | 0 | 26.245 | False | False | 训练区间交易数略低于目标下限，仅作为候选参考 |
| v004 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | True | 通过 |
| v005 | unknown | disable_or_adjust_trailing | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v006 | unknown | add_entry_filter | 2 | -0.145 | 0.236 | 0.190 | 0.449 | -1.897 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v007 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
| v008 | unknown | remove_bad_entry_condition | 20 | 0.213 | 1.577 | 0.292 | 5.819 | -3.691 | 0 | 23.513 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v009 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v010 | unknown | tighten_volume_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v011 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
| v012 | unknown | cooldown_or_protection | 25 | -0.111 | 0.853 | 0.680 | 6.320 | -7.583 | 0.149 | 23.629 | True | False | 通过 |
| v013 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | True | 通过 |
| v014 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
| v015 | unknown | adjust_stoploss | 25 | -0.204 | 0.705 | 0.587 | 4.458 | -6.588 | 0.091 | -8.598 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v016 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
| v017 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
| v018 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
| v019 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
| v020 | unknown | cooldown_or_protection | 25 | -0.051 | 0.926 | 0.620 | 6.320 | -6.984 | 0.149 | 24.321 | True | False | 通过 |
- near-miss=True；更新 official best=True；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，仅作为候选参考; 训练区间交易数略低于目标下限，验证强度不足

### 2026-05-30T09:15:14.954006 — `20260530_084755`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_084755_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_075936_v013
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 20 | 0.033 | 1.064 | 0.471 | 5.569 | -5.236 | 0 | 24.500 | True | True | 通过 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v005 | unknown | tighten_entry_trigger | 6 | -0.309 | 0.245 | 0.383 | 1.003 | -4.089 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v006 | unknown | adjust_roi | 63 | -0.969 | 0.505 | 1.456 | 9.723 | -19.561 | 0.149 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v007 | unknown | cooldown_or_protection | 20 | 0.033 | 1.064 | 0.471 | 5.569 | -5.236 | 0 | 24.500 | True | False | 通过 |
| v008 | unknown | adjust_stoploss | 45 | -0.787 | 0.427 | 1.123 | 5.850 | -13.715 | 0 | -90.015 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v009 | unknown | cooldown_or_protection | 20 | 0.033 | 1.064 | 0.471 | 5.569 | -5.236 | 0 | 24.500 | True | False | 通过 |
| v010 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v011 | unknown | disable_or_adjust_trailing | 20 | 0.033 | 1.064 | 0.471 | 5.569 | -5.236 | 0 | 24.500 | True | False | 通过 |
| v012 | unknown | cooldown_or_protection | 20 | 0.033 | 1.064 | 0.471 | 5.569 | -5.236 | 0 | 24.500 | True | False | 通过 |
| v013 | unknown | tighten_volume_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=True；主要失败=PF 低; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 训练区间交易数超过目标上限; 验证区间全部亏损

### 2026-05-30T10:31:01.318809 — `20260530_102832`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_102832_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_084755_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 24 | -0.066 | 0.905 | 0.595 | 6.320 | -6.984 | 0 | 23.989 | True | True | 通过 |
- near-miss=True；更新 official best=True；主要失败=n/a

### 2026-05-30T10:41:35.137100 — `20260530_103417`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_103417_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_102832_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 23 | -0.091 | 0.869 | 0.595 | 6.070 | -6.984 | 0 | 23.774 | True | True | 通过 |
| v002 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v005 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=True；主要失败=PF 低; 策略与本次 run 已测试策略高度重复

### 2026-05-30T11:10:01.802140 — `20260530_110100`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_110100_v004；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_103417_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 20 | -0.166 | 0.762 | 0.621 | 5.319 | -6.984 | 0 | -7.478 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | 0.000 | 0.000 | 0.000 | 0 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v004 | unknown | cooldown_or_protection | 23 | -0.091 | 0.869 | 0.595 | 6.070 | -6.984 | 0 | 23.774 | True | True | 通过 |
| v005 | unknown | tighten_entry_trigger | 4 | 0.120 | 0.000 | 0.000 | 1.198 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
- near-miss=True；更新 official best=True；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间无交易; 验证区间表现不稳定

### 2026-05-30T11:31:40.133412 — `20260530_112143`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_112143_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_110100_v004
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 23 | -0.091 | 0.869 | 0.620 | 6.070 | -6.984 | 0 | 23.233 | True | True | 通过 |
| v002 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v005 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=True；主要失败=PF 低; 策略与本次 run 已测试策略高度重复

### 2026-05-30T11:41:56.489838 — `20260530_113157`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_113157_v004；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_112143_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 22 | -0.116 | 0.833 | 0.620 | 5.820 | -6.984 | 0 | -6.955 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | tighten_entry_trigger | 10 | -0.270 | 0.485 | 0.349 | 2.543 | -5.238 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v004 | unknown | cooldown_or_protection | 23 | -0.091 | 0.869 | 0.620 | 6.070 | -6.984 | 0 | 23.233 | True | True | 通过 |
| v005 | unknown | tighten_volume_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=True；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 验证区间表现不稳定

### 2026-05-30T12:00:39.102981 — `20260530_115450`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_115450_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_113157_v004
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 25 | -0.041 | 0.941 | 0.595 | 6.570 | -6.984 | 0 | 24.654 | True | True | 通过 |
| v002 | unknown | tag_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=True；主要失败=PF 低; 策略与本次 run 已测试策略高度重复

### 2026-05-30T12:29:56.227968 — `20260530_121702`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_121702_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_115450_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 20 | 0.033 | 1.063 | 0.446 | 5.569 | -5.236 | 0 | 24.516 | True | True | 通过 |
| v002 | unknown | adjust_roi | 18 | -0.048 | 0.931 | 0.615 | 5.810 | -6.984 | 0.692 | -5.461 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v005 | unknown | tighten_entry_trigger | 20 | 0.033 | 1.063 | 0.446 | 5.569 | -5.236 | 0 | 24.516 | True | False | 通过 |
- near-miss=True；更新 official best=True；主要失败=PF 低; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数略低于目标下限，仅作为候选参考

### 2026-05-30T12:45:15.278871 — `20260530_123200`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_121702_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_121702_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | adjust_roi | 22 | -0.133 | 0.746 | 0.482 | 3.906 | -5.236 | 0 | -37.031 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 16 | 0.133 | 1.381 | 0.297 | 4.818 | -3.489 | 0 | 23.240 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | tighten_entry_trigger | 19 | -0.191 | 0.726 | 0.620 | 5.066 | -6.981 | 0 | -7.265 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v004 | unknown | adjust_stoploss | 24 | -0.142 | 0.747 | 0.378 | 4.170 | -5.585 | 0 | -38.361 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v005 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数略低于目标下限，仅作为候选参考; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-30T13:11:39.179706 — `20260530_130417`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_121702_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_121702_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | adjust_roi | 21 | -0.203 | 0.673 | 0.419 | 4.161 | -6.186 | 0 | -37.493 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 16 | 0.133 | 1.381 | 0.297 | 4.818 | -3.489 | 0 | 23.240 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-30T14:05:21.512756 — `20260530_133336`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260530_121702_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_121702_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 0 | 0.000 | 0.000 | 0.000 | 0 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v002 | unknown | adjust_roi | 44 | -0.581 | 0.464 | 0.918 | 5.021 | -10.827 | 0 | -59.269 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v003 | unknown | pair_specific_filter | 19 | 0.008 | 1.015 | 0.446 | 4.870 | -5.236 | 0.444 | 23.937 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v004 | unknown | adjust_stoploss | 23 | -0.088 | 0.831 | 0.372 | 4.343 | -5.227 | 0 | -39.180 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v005 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v006 | unknown | cooldown_or_protection | 20 | 0.033 | 1.067 | 0.417 | 5.273 | -4.940 | 0 | -7.303 | False | False | final_score<=0 |
| v007 | unknown | remove_bad_entry_condition | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v008 | unknown | tighten_volume_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v009 | unknown | disable_or_adjust_trailing | 20 | -0.143 | 0.726 | 0.477 | 3.753 | -5.236 | 0.049 | -5.914 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v010 | unknown | tag_specific_filter | 24 | -0.158 | 0.717 | 0.379 | 4.006 | -5.585 | 0 | -38.630 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v011 | unknown | reduce_trade_frequency | 12 | 0.250 | 0.000 | 0.000 | 2.503 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
- near-miss=True；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 训练区间交易数超过目标上限; 训练区间无交易; 验证区间表现不稳定

### 2026-05-31T00:47:04.176855 — `20260531_003105`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_003105_v005；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260530_121702_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 45 | -0.401 | 0.706 | 0.844 | 9.605 | -13.612 | 0 | -91.554 | False | False | 固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v002 | unknown | pair_specific_filter | 32 | 0.004 | 1.005 | 0.595 | 7.619 | -7.583 | 0 | 22.501 | True | True | 通过 |
| v003 | unknown | tighten_entry_trigger | 32 | 0.059 | 1.078 | 0.576 | 8.177 | -7.583 | 0 | 22.455 | True | False | 通过 |
| v004 | unknown | adjust_roi | 31 | 0.142 | 1.187 | 0.566 | 9.001 | -7.583 | 0 | 21.254 | True | False | 通过 |
| v005 | unknown | pair_specific_filter | 32 | 0.009 | 1.012 | 0.605 | 7.675 | -7.583 | 0 | 22.718 | True | True | 通过 |
- near-miss=True；更新 official best=True；主要失败=交易数过高; 固定止损吞噬 ROI; 训练区间交易数超过目标上限; 验证区间全部亏损

### 2026-05-31T02:03:53.047181 — `20260531_014140`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_003105_v005
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 36 | -0.064 | 0.932 | 0.773 | 8.838 | -9.480 | 0 | 22.955 | True | True | 通过 |
| v002 | unknown | add_entry_filter | 27 | -0.050 | 0.935 | 0.605 | 7.087 | -7.583 | 0 | 22.502 | True | False | 通过 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | unknown | tighten_entry_trigger | 24 | -0.110 | 0.854 | 0.606 | 6.478 | -7.583 | 0 | 21.603 | True | False | 通过 |
| v005 | unknown | add_entry_filter | 25 | -0.182 | 0.740 | 0.453 | 5.168 | -6.983 | 0 | -8.364 | False | False | 固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=True；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T02:48:34.628744 — `20260531_023806`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 31 | 0.015 | 1.020 | 0.605 | 7.737 | -7.583 | 0 | 22.691 | True | False | 通过 |
| v002 | unknown | tighten_entry_trigger | 34 | -0.105 | 0.889 | 0.774 | 8.426 | -9.480 | 0 | 21.464 | True | False | 通过 |
| v003 | unknown | adjust_roi | 41 | -0.282 | 0.749 | 0.844 | 8.406 | -11.225 | 0 | -40.414 | False | False | 固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=True；更新 official best=False；主要失败=交易数过高; 固定止损吞噬 ROI; 训练区间交易数超过目标上限; 验证区间全部亏损

### 2026-05-31T03:02:48.859268 — `20260531_025322`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=False；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 36 | -0.064 | 0.932 | 0.773 | 8.838 | -9.480 | 0 | 22.955 | True | False | 通过 |
| v002 | unknown | add_entry_filter | 35 | -0.085 | 0.910 | 0.774 | 8.627 | -9.480 | 0 | 23.621 | True | False | 通过 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=PF 低; 策略与本次 run 已测试策略高度重复

### 2026-05-31T03:02:48.859268 — `20260531_031506`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 36 | -0.064 | 0.932 | 0.773 | 8.838 | -9.480 | 0 | 22.955 | True | False | 通过 |
| v002 | unknown | add_entry_filter | 35 | -0.085 | 0.910 | 0.774 | 8.627 | -9.480 | 0 | 23.621 | True | False | 通过 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=PF 低; 策略与本次 run 已测试策略高度重复

### 2026-05-31T03:34:41.619204 — `20260531_032659`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 24 | -0.110 | 0.854 | 0.606 | 6.478 | -7.583 | 0 | 21.603 | True | False | 通过 |
| v002 | unknown | pair_specific_filter | 24 | 0.295 | 1.778 | 0.267 | 6.736 | -3.789 | 0 | 23.462 | True | False | 通过 |
| v003 | unknown | tighten_entry_trigger | 11 | 0.157 | 2.124 | 0.140 | 2.965 | -1.396 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
- near-miss=True；更新 official best=False；主要失败=交易数偏低; 训练区间交易数低于目标下限

### 2026-05-31T03:34:41.619204 — `20260531_033637`
- 模式：optimize；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=None ideal_max=None max=40
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 24 | -0.110 | 0.854 | 0.606 | 6.478 | -7.583 | 0 | 21.603 | True | False | 通过 |
| v002 | unknown | pair_specific_filter | 24 | 0.295 | 1.778 | 0.267 | 6.736 | -3.789 | 0 | 23.462 | True | False | 通过 |
| v003 | unknown | tighten_entry_trigger | 11 | 0.157 | 2.124 | 0.140 | 2.965 | -1.396 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
- near-miss=True；更新 official best=False；主要失败=交易数偏低; 训练区间交易数低于目标下限

### 2026-05-31T04:07:44.649536 — `20260531_035556`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=38 ideal_min=None ideal_max=None max=110
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 42 | -0.084 | 0.926 | 0.962 | 10.541 | -11.378 | 0 | 22.360 | True | False | 通过 |
| v002 | unknown | add_entry_filter | 36 | -0.064 | 0.932 | 0.773 | 8.838 | -9.480 | 0 | 22.955 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | adjust_roi | 55 | -0.418 | 0.685 | 1.081 | 9.093 | -13.271 | 0 | -8.886 | False | False | 固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-31T04:39:13.118630 — `20260531_042952`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=38 ideal_min=None ideal_max=None max=110
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 40 | -0.183 | 0.825 | 0.769 | 8.640 | -10.470 | 0 | -10.351 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | add_entry_filter | 41 | -0.310 | 0.723 | 0.831 | 8.076 | -11.177 | 0 | -8.282 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | adjust_roi | 44 | -0.404 | 0.560 | 0.688 | 5.143 | -9.182 | 0 | -39.717 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 验证区间表现不稳定

### 2026-05-31T06:25:37.045306 — `20260531_061917`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=38 ideal_min=None ideal_max=None max=110
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | tighten_entry_trigger | 20 | -0.306 | 0.505 | 0.463 | 3.127 | -6.185 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | remove_bad_entry_condition | 36 | -0.037 | 0.961 | 0.753 | 9.102 | -9.475 | 0 | 21.624 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=PF 低; 交易数偏低; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-31T06:25:37.045306 — `20260531_062850`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=38 ideal_min=None ideal_max=None max=110
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | tighten_entry_trigger | 20 | -0.306 | 0.505 | 0.463 | 3.127 | -6.185 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | remove_bad_entry_condition | 36 | -0.037 | 0.961 | 0.753 | 9.102 | -9.475 | 0 | 21.624 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=PF 低; 交易数偏低; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-31T06:25:37.045306 — `20260531_064143`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, DOGE/USDT, ETH/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=38 ideal_min=None ideal_max=None max=110
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260530_084755_v008；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | tighten_entry_trigger | 20 | -0.306 | 0.505 | 0.463 | 3.127 | -6.185 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | remove_bad_entry_condition | 36 | -0.037 | 0.961 | 0.753 | 9.102 | -9.475 | 0 | 21.624 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=PF 低; 交易数偏低; 固定止损吞噬 ROI; 策略与本次 run 已测试策略高度重复; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_064426`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_070718`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_070924`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_071130`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_073558`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_073812`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T06:51:49.129539 — `20260531_074019`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=BTC/USDT, ETH/USDT, OP/USDT, SOL/USDT, TOTAL, XRP/USDT
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 36 | -0.700 | 0.519 | 0.845 | 7.559 | -14.556 | 0 | -12.021 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | pair_specific_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v003 | unknown | adjust_roi | 50 | -0.750 | 0.618 | 1.119 | 12.138 | -19.634 | 0 | -13.769 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T07:52:47.929135 — `20260531_074505`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | tighten_entry_trigger | 23 | -0.229 | 0.725 | 0.475 | 6.037 | -8.323 | 0 | -39.414 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | unknown | add_entry_filter | 15 | 0.011 | 1.028 | 0.377 | 3.881 | -3.776 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v003 | unknown | remove_bad_entry_condition | 17 | 0.221 | 1.705 | 0.130 | 5.344 | -3.134 | 0 | -6.883 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 交易数偏低; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

### 2026-05-31T08:05:06.349280 — `20260531_075834`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 28 | -0.536 | 0.531 | 0.665 | 6.053 | -11.410 | 0 | -58.691 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v002 | unknown | tighten_entry_trigger | 4 | 0.080 | 0.000 | 0.000 | 0.801 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v003 | unknown | add_entry_filter | 66 | -0.764 | 0.618 | 1.048 | 12.349 | -19.993 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=False；更新 official best=False；主要失败=PF 低; 交易数过高; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 训练区间交易数超过目标上限; 验证区间表现不稳定

### 2026-05-31T08:14:01.866585 — `20260531_080542`
- 模式：optimize；pre_run_ai_review=True；recommended_pairs=True；pairs=OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 25 | -0.115 | 0.835 | 0.356 | 5.808 | -6.957 | 0 | -24.100 | False | False | 高频风险：交易数超过目标上限 1.5 倍 |
| v002 | unknown | tighten_entry_trigger | 17 | -0.006 | 0.985 | 0.272 | 3.670 | -4.178 | 0.444 | -9.091 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | adjust_roi | 25 | -0.016 | 0.971 | 0.489 | 5.506 | -5.668 | 0 | -24.518 | False | False | 高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=True；更新 official best=False；主要失败=交易数过高; 最差验证月份拖累整体表现; 训练区间交易数略低于目标下限，验证强度不足; 训练区间交易数超过目标上限; 验证区间表现不稳定

### 2026-05-31T08:14:01.866585 — `20260531_082234`
- 模式：pair-scan；pre_run_ai_review=False；recommended_pairs=True；pairs=OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 25 | -0.115 | 0.835 | 0.356 | 5.808 | -6.957 | 0 | -24.100 | False | False | 高频风险：交易数超过目标上限 1.5 倍 |
| v002 | unknown | tighten_entry_trigger | 17 | -0.006 | 0.985 | 0.272 | 3.670 | -4.178 | 0.444 | -9.091 | False | False | 训练区间交易数略低于目标下限，验证强度不足 |
| v003 | unknown | adjust_roi | 25 | -0.016 | 0.971 | 0.489 | 5.506 | -5.668 | 0 | -24.518 | False | False | 高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=True；更新 official best=False；主要失败=交易数过高; 最差验证月份拖累整体表现; 训练区间交易数略低于目标下限，验证强度不足; 训练区间交易数超过目标上限; 验证区间表现不稳定

### 2026-05-31T08:45:13.621122 — `20260531_084250`
- 模式：pair-scan；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 12 | 0.242 | 2.388 | 0.174 | 4.165 | -1.744 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
- near-miss=True；更新 official best=False；主要失败=交易数偏低; 训练区间交易数低于目标下限

### 2026-05-31T08:50:13.049963 — `20260531_084648`
- 模式：pair-scan；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=20 ideal_max=20 max=24
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | add_entry_filter | 24 | -0.103 | 0.815 | 0.344 | 4.109 | -5.584 | 0.444 | -71.704 | False | False | 固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 验证区间表现不稳定

### 2026-05-31T09:05:40.228862 — `20260531_085339`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | tighten_entry_trigger | 71 | -0.816 | 0.618 | 1.185 | 13.219 | -21.092 | 0 | -176.995 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v002 | pullback_reversal | add_entry_filter | 111 | -2.318 | 0.037 | 2.318 | 0.252 | -3.624 | -19.810 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v003 | breakout_momentum | reduce_trade_frequency | 27 | -0.441 | 0.376 | 0.512 | 2.329 | 0 | -6.740 | -209.626 | False | False | Profit factor 低于 baseline |
| v004 | low_volatility_mean_reversion | adjust_roi | 11 | -0.251 | 0.000 | 0.251 | 0 | 0 | -2.507 | 0.000 | False | False | Profit factor 低于 baseline |
| v005 | strict_risk_filter | disable_or_adjust_trailing | 161 | -3.376 | 0.031 | 3.376 | 0 | 0 | -33.758 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；最大回撤超过目标；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 交易数过高; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 训练区间交易数超过目标上限; 验证区间全部亏损

### 2026-05-31T09:28:35.564199 — `20260531_091409`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 26 | -0.305 | 0.594 | 0.436 | 4.466 | -7.322 | 0 | -73.107 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | pullback_reversal | tighten_entry_trigger | 23 | -0.273 | 0.561 | 0.365 | 3.465 | -6.223 | 0 | -77.338 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | breakout_momentum | remove_bad_entry_condition | 195 | -2.922 | 0.502 | 3.383 | 29.501 | -58.368 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；最大回撤超过目标；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v004 | low_volatility_mean_reversion | pair_specific_filter | 14 | 0.090 | 1.856 | 0.105 | 1.945 | -1.048 | 0 | 0.000 | False | False | 训练区间交易数低于目标下限 |
| v005 | strict_risk_filter | cooldown_or_protection | 42 | -0.823 | 0.342 | 0.853 | 4.268 | -12.497 | 0 | -74.725 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 验证区间全部亏损

### 2026-05-31T10:27:47.920613 — `20260531_101403`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 24 | -0.520 | 0.234 | 0.551 | 1.258 | 0 | -6.264 | -76.947 | False | False | Profit factor 低于 baseline |
| v002 | pullback_reversal | tighten_entry_trigger | 15 | -0.156 | 0.650 | 0.224 | 2.896 | -4.457 | 0 | 0.000 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | breakout_momentum | adjust_roi | 148 | -2.743 | 0.134 | 2.743 | 3.444 | 0 | -30.435 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍 |
| v004 | low_volatility_mean_reversion | remove_bad_entry_condition | 125 | -2.265 | 0.309 | 2.265 | 9.447 | -7.076 | -25.022 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍 |
| v005 | strict_risk_filter | cooldown_or_protection | 170 | -2.791 | 0.367 | 2.866 | 12.847 | -2.048 | -38.628 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 交易数过高; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 验证区间全部亏损

### 2026-05-31T10:44:19.907908 — `20260531_103001`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 173 | -2.336 | 0.602 | 2.717 | 35.280 | -58.392 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v002 | pullback_reversal | tighten_entry_trigger | 34 | -1.159 | 0.232 | 1.164 | 3.495 | -15.082 | 0 | -46.544 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | breakout_momentum | reduce_trade_frequency | 170 | -2.501 | 0.515 | 2.869 | 26.614 | -50.842 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍 |
| v004 | low_volatility_mean_reversion | pair_specific_filter | 1 | 0.005 | 0.000 | 0.000 | 0.049 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v005 | strict_risk_filter | cooldown_or_protection | 5 | -0.331 | 0.070 | 0.331 | 0.250 | -3.565 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 验证区间全部亏损

### 2026-05-31T11:10:09.118073 — `20260531_110237`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 13 | -0.067 | 0.759 | 0.266 | 2.107 | -2.777 | 0 | 0.000 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | pullback_reversal | tighten_entry_trigger | 19 | -0.443 | 0.364 | 0.453 | 2.532 | -6.966 | 0 | -73.930 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | breakout_momentum | remove_bad_entry_condition | 172 | -2.952 | 0.511 | 3.418 | 30.881 | -59.710 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；最大回撤超过目标；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；high_frequency_failu |
- near-miss=True；更新 official best=False；主要失败=PF 低; 交易数偏低; 固定止损吞噬 ROI; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间全部亏损

### 2026-05-31T11:24:37.502346 — `20260531_111202`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 66 | -0.146 | 0.868 | 0.468 | 9.551 | -10.974 | 0 | -60.345 | False | False | 高频风险：交易数超过目标上限 |
| v002 | pullback_reversal | tighten_entry_trigger | 0 | 0.000 | 0.000 | 0.000 | 0 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v003 | breakout_momentum | adjust_roi | 51 | -0.929 | 0.499 | 1.072 | 9.238 | -18.525 | 0 | -212.787 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v004 | low_volatility_mean_reversion | reduce_trade_frequency | 64 | -0.769 | 0.491 | 1.066 | 7.411 | -15.106 | 0 | -122.812 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 |
| v005 | strict_risk_filter | pair_specific_filter | 13 | -0.172 | 0.450 | 0.232 | 1.410 | -3.135 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 训练区间交易数低于目标下限; 训练区间交易数超过目标上限; 训练区间无交易; 验证区间全部亏损

### 2026-05-31T11:55:19.182921 — `20260531_114018`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_014140_v001
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | unknown | pair_specific_filter | 39 | 0.030 | 1.032 | 0.768 | 9.760 | -9.462 | 0 | 22.184 | True | False | 通过 |
| v002 | unknown | add_entry_filter | 30 | -0.152 | 0.825 | 0.451 | 7.203 | -8.727 | 0 | -40.844 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | unknown | pair_specific_filter | 28 | 0.159 | 1.279 | 0.451 | 7.272 | -5.685 | 0 | -9.370 | False | False | final_score<=0 |
| v004 | unknown | adjust_roi | 43 | 0.093 | 1.123 | 0.648 | 8.051 | -7.565 | 0.444 | 21.740 | True | False | 通过 |
| v005 | unknown | tighten_entry_trigger | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 验证区间表现不稳定

### 2026-05-31T12:13:38.434596 — `20260531_115919`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_064426_v003
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 43 | -0.313 | 0.666 | 0.693 | 6.222 | -9.349 | 0 | -9.641 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v002 | pullback_reversal | tighten_entry_trigger | 0 | 0.000 | 0.000 | 0.000 | 0 | 0 | 0 | 0.000 | False | False | Profit factor 低于 baseline |
| v003 | breakout_momentum | remove_bad_entry_condition | 106 | -0.845 | 0.694 | 1.402 | 18.836 | -27.272 | 0.376 | 0.000 | False | False | 训练区间亏损超过 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 |
| v004 | trend_following | adjust_roi | 41 | -0.276 | 0.779 | 0.816 | 9.720 | -12.480 | 0 | -11.471 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v005 | pullback_reversal | adjust_stoploss | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 训练区间回测失败 |
- near-miss=True；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数严重超过目标上限; 训练区间回测失败; 训练区间无交易; 验证区间表现不稳定

### 2026-05-31T12:53:37.555421 — `20260531_122748`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_064426_v003
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | pair_specific_filter | 32 | -0.083 | 0.908 | 0.507 | 8.144 | -8.971 | 0 | -14.077 | False | False | final_score<=0 |
| v002 | pullback_reversal | tighten_entry_trigger | 5 | -0.344 | 0.092 | 0.344 | 0.350 | -3.788 | 0 | 0.000 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v003 | breakout_momentum | add_entry_filter | 113 | -2.028 | 0.431 | 2.345 | 15.359 | -35.016 | 0 | 0.000 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 |
| v004 | trend_following | adjust_roi | 51 | -0.620 | 0.457 | 0.730 | 5.224 | -11.423 | 0 | -40.894 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v005 | pullback_reversal | remove_bad_entry_condition | 42 | -0.412 | 0.670 | 0.561 | 8.371 | -12.490 | 0 | -75.603 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v006 | trend_following | cooldown_or_protection | 54 | -1.046 | 0.434 | 1.245 | 6.261 | -18.465 | 1.749 | -62.201 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v007 | breakout_momentum | tighten_volume_filter | 66 | -0.853 | 0.560 | 1.285 | 10.372 | -18.811 | 0.465 | -93.594 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 |
| v008 | trend_following | adjust_stoploss | 56 | -0.808 | 0.361 | 0.890 | 4.564 | -12.646 | 0 | -43.248 | False | False | 训练区间亏损超过 baseline；Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
- near-miss=True；更新 official best=False；主要失败=PF 低; final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 训练区间交易数严重超过目标上限; 训练区间交易数低于目标下限; 训练区间交易数超过目标上限; 验证区间表现不稳定

### 2026-05-31T14:47:54.371735 — `20260531_142333`
- 模式：explore-strategy-family；pre_run_ai_review=True；recommended_pairs=True；pairs=BNB/USDT, BTC/USDT, DOGE/USDT, OP/USDT, SOL/USDT, TOTAL
- 交易数目标：min=20 ideal_min=25 ideal_max=45 max=60
- 父策略：historical_best=MultiCoin_AI_Strategy_20260531_014140_v001；nearest_candidate=MultiCoin_AI_Strategy_20260531_064426_v003；actual_session_parent=MultiCoin_AI_Strategy_20260531_064426_v003
| version | family | mutation | trades | train% | PF | DD% | ROI_abs | SL_abs | trail_abs | score | valid | new_best | failure |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| v001 | trend_following | add_entry_filter | 16 | -0.440 | 0.380 | 0.532 | 2.699 | -7.096 | 0 | -44.073 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v002 | trend_following | pair_specific_filter | 34 | -0.356 | 0.695 | 0.775 | 8.105 | -11.664 | 0 | -13.292 | False | False | 固定止损亏损吞噬 ROI 收益。 |
| v003 | trend_following | add_entry_filter | 0 | n/a | n/a | n/a | 0 | 0 | 0 | 0.000 | False | False | 策略与本次 run 已测试策略高度重复 |
| v004 | trend_following | cooldown_or_protection | 30 | -0.428 | 0.601 | 0.668 | 6.451 | -10.727 | 0 | -10.644 | False | False | Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。 |
| v005 | trend_following | tighten_entry_trigger | 103 | -0.700 | 0.786 | 1.187 | 25.725 | -32.139 | 0 | 0.000 | False | False | severe_high_frequency_failure：交易数超过 max_trades * 1.5；固定止损亏损吞噬 ROI 收益。 |
- near-miss=False；更新 official best=False；主要失败=final_score<=0; 固定止损吞噬 ROI; 最差验证月份拖累整体表现; 策略与本次 run 已测试策略高度重复; 训练区间交易数严重超过目标上限; 训练区间交易数略低于目标下限，验证强度不足; 验证区间表现不稳定

## 最佳策略排名 Top 10

1. `20260530_031150` `v007` `MultiCoin_AI_Strategy_20260530_031150_v007` family=unknown mutation=adjust_stoploss train=0.345% PF=1.617 trades=25 val_avg=n/a% score=-7.854; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：final_score<=0; 建议继续作 nearest_candidate。
2. `20260531_032659` `v002` `MultiCoin_AI_Strategy_20260531_032659_v002` family=unknown mutation=pair_specific_filter train=0.295% PF=1.778 trades=24 val_avg=n/a% score=23.462; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：通过; 建议继续作 nearest_candidate。
3. `20260531_033637` `v002` `MultiCoin_AI_Strategy_20260531_032659_v002` family=unknown mutation=pair_specific_filter train=0.295% PF=1.778 trades=24 val_avg=n/a% score=23.462; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：通过; 建议继续作 nearest_candidate。
4. `20260530_014158` `v002` `MultiCoin_AI_Strategy_20260530_014158_v002` family=unknown mutation=add_entry_filter train=0.241% PF=1.731 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
5. `20260530_014158` `v003` `MultiCoin_AI_Strategy_20260530_014158_v003` family=unknown mutation=pair_specific_filter train=0.241% PF=1.731 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
6. `20260530_014158` `v011` `MultiCoin_AI_Strategy_20260530_014158_v011` family=unknown mutation=pair_specific_filter train=0.236% PF=1.716 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
7. `20260530_014158` `v027` `MultiCoin_AI_Strategy_20260530_014158_v027` family=unknown mutation=pair_specific_filter train=0.236% PF=1.716 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
8. `20260530_014158` `v030` `MultiCoin_AI_Strategy_20260530_014158_v030` family=unknown mutation=pair_specific_filter train=0.236% PF=1.716 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
9. `20260530_014158` `v041` `MultiCoin_AI_Strategy_20260530_014158_v041` family=unknown mutation=pair_specific_filter train=0.236% PF=1.716 trades=16 val_avg=n/a% score=0.000; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数低于目标下限; 建议继续作 nearest_candidate。
10. `20260531_074505` `v003` `MultiCoin_AI_Strategy_20260531_074505_v003` family=unknown mutation=remove_bad_entry_condition train=0.221% PF=1.705 trades=17 val_avg=n/a% score=-6.883; 接近原因：交易数/PF/收益相对接近且非语法/0交易失败；未过 best：训练区间交易数略低于目标下限，验证强度不足; 建议继续作 nearest_candidate。

## Near-miss 列表

- `20260530_031150` `v007` MultiCoin_AI_Strategy_20260530_031150_v007：family=unknown，train=0.345% PF=1.617 trades=25，val_avg=n/a%，risk_control=False，失败：final_score<=0
- `20260531_032659` `v002` MultiCoin_AI_Strategy_20260531_032659_v002：family=unknown，train=0.295% PF=1.778 trades=24，val_avg=n/a%，risk_control=False，失败：通过
- `20260531_033637` `v002` MultiCoin_AI_Strategy_20260531_032659_v002：family=unknown，train=0.295% PF=1.778 trades=24，val_avg=n/a%，risk_control=False，失败：通过
- `20260530_014158` `v002` MultiCoin_AI_Strategy_20260530_014158_v002：family=unknown，train=0.241% PF=1.731 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v003` MultiCoin_AI_Strategy_20260530_014158_v003：family=unknown，train=0.241% PF=1.731 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v011` MultiCoin_AI_Strategy_20260530_014158_v011：family=unknown，train=0.236% PF=1.716 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v027` MultiCoin_AI_Strategy_20260530_014158_v027：family=unknown，train=0.236% PF=1.716 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v030` MultiCoin_AI_Strategy_20260530_014158_v030：family=unknown，train=0.236% PF=1.716 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v041` MultiCoin_AI_Strategy_20260530_014158_v041：family=unknown，train=0.236% PF=1.716 trades=16，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260531_074505` `v003` MultiCoin_AI_Strategy_20260531_074505_v003：family=unknown，train=0.221% PF=1.705 trades=17，val_avg=n/a%，risk_control=False，失败：训练区间交易数略低于目标下限，验证强度不足
- `20260530_014158` `v009` MultiCoin_AI_Strategy_20260530_014158_v009：family=unknown，train=0.295% PF=1.528 trades=25，val_avg=n/a%，risk_control=False，失败：final_score<=0
- `20260530_075936` `v008` MultiCoin_AI_Strategy_20260530_075936_v008：family=unknown，train=0.213% PF=1.577 trades=20，val_avg=n/a%，risk_control=False，失败：训练区间交易数略低于目标下限，验证强度不足
- `20260531_084250` `v001` MultiCoin_AI_Strategy_20260531_084250_v001：family=unknown，train=0.242% PF=2.388 trades=12，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_033035` `v004` MultiCoin_AI_Strategy_20260530_033035_v004：family=unknown，train=0.284% PF=1.509 trades=25，val_avg=n/a%，risk_control=False，失败：final_score<=0
- `20260530_014158` `v005` MultiCoin_AI_Strategy_20260530_014158_v005：family=unknown，train=0.252% PF=1.504 trades=25，val_avg=n/a%，risk_control=False，失败：通过
- `20260530_014158` `v033` MultiCoin_AI_Strategy_20260530_014158_v033：family=unknown，train=0.246% PF=1.498 trades=22，val_avg=n/a%，risk_control=False，失败：训练区间交易数略低于目标下限，仅作为候选参考
- `20260531_091409` `v004` MultiCoin_AI_Strategy_20260531_091409_v004：family=low_volatility_mean_reversion，train=0.090% PF=1.856 trades=14，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_033035` `v005` MultiCoin_AI_Strategy_20260530_033035_v005：family=unknown，train=0.202% PF=1.532 trades=18，val_avg=n/a%，risk_control=False，失败：训练区间交易数低于目标下限
- `20260530_014158` `v015` MultiCoin_AI_Strategy_20260530_014158_v015：family=unknown，train=0.226% PF=1.457 trades=23，val_avg=n/a%，risk_control=False，失败：训练区间交易数略低于目标下限，仅作为候选参考
- `20260530_014158` `v012` MultiCoin_AI_Strategy_20260530_014158_v012：family=unknown，train=0.221% PF=1.448 trades=22，val_avg=n/a%，risk_control=False，失败：训练区间交易数略低于目标下限，验证强度不足

## strategy_family 对比

| family | attempts | valid | near_miss | high_freq | zero | runtime | avg_train% | avg_val% | avg_PF | best | worst | 建议 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|---|---|
| trend_following | 17 | 0 | 3 | 2 | 1 | 0 | -0.579 | n/a | 0.609 | 20260531_110237/v001 -0.067% | 20260531_085339/v001 -0.816% | 继续作为主线，但加入 worst-month 和止损归因惩罚。 |
| pullback_reversal | 10 | 0 | 0 | 1 | 3 | 0 | -0.567 | n/a | 0.290 | 20260531_101403/v002 -0.156% | 20260531_091409/v002 -0.273% | 放宽入场阈值并加交易数下限/冷却，避免 0 交易。 |
| breakout_momentum | 9 | 0 | 0 | 6 | 0 | 0 | -1.802 | n/a | 0.469 | 20260531_115919/v003 -0.845% | 20260531_111202/v003 -0.929% | 暂停或极低权重，直到高频降权生效。 |
| low_volatility_mean_reversion | 5 | 0 | 1 | 1 | 0 | 0 | -0.638 | n/a | 0.531 | 20260531_091409/v004 0.090% | 20260531_111202/v004 -0.769% | 小样本低频探索，强制低交易数。 |
| strict_risk_filter | 5 | 0 | 0 | 2 | 0 | 0 | -1.499 | n/a | 0.252 | 20260531_111202/v005 -0.172% | 20260531_091409/v005 -0.823% | 暂停，除非能证明低交易数仍有正收益。 |

## pair 对比

日志存在 pair_leaderboard，但缺少统一 pair_attribution.json；因此 pair 结论为中等置信度。
- BTC/USDT: samples=31 avg_train=-0.060% avg_val=-0.005% avg_PF=0.686 total_ROI=23.436 total_SL=-34.152 worst=[{'run_id': '20260531_031506', 'train_profit_pct': -0.05953445000000002, 'validation_avg_profit_pct': -0.004641625333333339, 'train_stoploss_profit_abs': -1.89734821}, {'run_id': '20260531_032659', 'train_profit_pct': -0.05953445000000002, 'validation_avg_profit_pct': -0.004641625333333339, 'train_stoploss_profit_abs': -1.89734821}]
- OP/USDT: samples=31 avg_train=0.230% avg_val=0.077% avg_PF=0.000 total_ROI=41.325 total_SL=0 worst=[{'run_id': '20260531_031506', 'train_profit_pct': 0.22958400199999998, 'validation_avg_profit_pct': 0.07703828133333335, 'train_stoploss_profit_abs': 0.0}, {'run_id': '20260531_032659', 'train_profit_pct': 0.22958400199999998, 'validation_avg_profit_pct': 0.07703828133333335, 'train_stoploss_profit_abs': 0.0}]
- SOL/USDT: samples=31 avg_train=0.078% avg_val=-0.058% avg_PF=1.206 total_ROI=82.271 total_SL=-68.194 worst=[{'run_id': '20260531_031506', 'train_profit_pct': 0.07820143200000003, 'validation_avg_profit_pct': -0.05830823800000001, 'train_stoploss_profit_abs': -3.7885796}, {'run_id': '20260531_032659', 'train_profit_pct': 0.07820143200000003, 'validation_avg_profit_pct': -0.05830823800000001, 'train_stoploss_profit_abs': -3.7885796}]
- BNB/USDT: samples=31 avg_train=0.175% avg_val=-0.304% avg_PF=0.000 total_ROI=31.533 total_SL=0 worst=[{'run_id': '20260531_031506', 'train_profit_pct': 0.175182753, 'validation_avg_profit_pct': -0.30390103733333335, 'train_stoploss_profit_abs': 0.0}, {'run_id': '20260531_032659', 'train_profit_pct': 0.175182753, 'validation_avg_profit_pct': -0.30390103733333335, 'train_stoploss_profit_abs': 0.0}]
- DOGE/USDT: samples=31 avg_train=0.000% avg_val=0.000% avg_PF=0.000 total_ROI=0 total_SL=0 worst=[{'run_id': '20260531_031506', 'train_profit_pct': 0.0, 'validation_avg_profit_pct': 0.0, 'train_stoploss_profit_abs': 0.0}, {'run_id': '20260531_032659', 'train_profit_pct': 0.0, 'validation_avg_profit_pct': 0.0, 'train_stoploss_profit_abs': 0.0}]

## 月份 / 市场环境对比

- 202602: samples=198 avg_profit=n/a% avg_PF=n/a profitable_ratio=0.000; worst_evidence=[{'run_id': '20260530_010736', 'version': 'v001', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}, {'run_id': '20260530_010736', 'version': 'v002', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}]
- 202603: samples=198 avg_profit=n/a% avg_PF=n/a profitable_ratio=0.000; worst_evidence=[{'run_id': '20260530_010736', 'version': 'v001', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}, {'run_id': '20260530_010736', 'version': 'v002', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}]
- 202604: samples=198 avg_profit=n/a% avg_PF=n/a profitable_ratio=0.000; worst_evidence=[{'run_id': '20260530_010736', 'version': 'v001', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}, {'run_id': '20260530_010736', 'version': 'v002', 'profit_pct': None, 'pf': None, 'trades': 0, 'family': 'unknown'}]
- 解释：202605 训练期接近但验证亏损，说明策略更适配近期局部行情；建议加入 market_regime_filter、avoid_choppy_market_filter、volatility contraction filter，并对 202603/202604 加 worst-month 惩罚或更高验证权重。

## 反复失败模式

### 固定止损吞噬 ROI 收益
- `20260530_010736` `v001`：ROI 12.97, stoploss -13.18, train 0.071%
- `20260530_010736` `v002`：ROI 15.05, stoploss -16.47, train -0.050%
- `20260530_010736` `v003`：ROI 3.30, stoploss -6.59, train -0.328%
- `20260530_014158` `v004`：ROI 5.60, stoploss -6.59, train 0.025%
- `20260530_014158` `v006`：ROI 4.64, stoploss -6.59, train -0.070%
### 训练期微利但验证期亏损
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。
### 202603 / 202604 经常亏损
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。
### 202602 经常相对更好
- `multiple` `validation`：202602 samples=198, avg_profit=n/a%, profitable_ratio=0.000
### 交易数超过 max_trades
- `20260530_084755` `v006`：trades 63 > max 40
- `20260530_084755` `v008`：trades 45 > max 40
- `20260530_133336` `v002`：trades 44 > max 40
- `20260531_003105` `v001`：trades 45 > max 40
- `20260531_023806` `v003`：trades 41 > max 40
### 验证期交易数爆炸
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。
### 0 交易
- `20260530_043127` `v003`：train trades 0, reason 策略与本次 run 已测试策略高度重复
- `20260530_043127` `v004`：train trades 0, reason 策略与本次 run 已测试策略高度重复
- `20260530_043127` `v006`：train trades 0, reason 策略与本次 run 已测试策略高度重复
- `20260530_043127` `v008`：train trades 0, reason 策略与本次 run 已测试策略高度重复
- `20260530_043127` `v009`：train trades 0, reason 策略与本次 run 已测试策略高度重复
### high winrate 但 PF 低
- `20260530_010736` `v002`：winrate 0.81, PF 0.9694858491556682
- `20260530_014158` `v006`：winrate 0.84, PF 0.8929994845655059
- `20260530_014158` `v007`：winrate 0.78, PF 0.6910402305655355
- `20260530_014158` `v020`：winrate 0.80, PF 0.9349850271839224
- `20260530_014158` `v022`：winrate 0.80, PF 0.8761399999726479
### 单笔亏损远大于单笔盈利
- `20260530_010736` `v001`：avg stoploss -1.65, avg ROI 0.35
- `20260530_010736` `v002`：avg stoploss -1.65, avg ROI 0.37
- `20260530_010736` `v003`：avg stoploss -1.65, avg ROI 0.30
- `20260530_014158` `v001`：avg stoploss -1.65, avg ROI 0.41
- `20260530_014158` `v002`：avg stoploss -1.65, avg ROI 0.40
### trailing 被用坏
- `20260531_085339` `v002`：moving/trailing stop profit -19.81, reason 训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍
- `20260531_085339` `v003`：moving/trailing stop profit -6.74, reason Profit factor 低于 baseline
- `20260531_085339` `v004`：moving/trailing stop profit -2.51, reason Profit factor 低于 baseline
- `20260531_085339` `v005`：moving/trailing stop profit -33.76, reason 训练区间亏损超过 baseline；Profit factor 低于 baseline；最大回撤超过目标；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍
- `20260531_101403` `v001`：moving/trailing stop profit -6.26, reason Profit factor 低于 baseline
### pullback_reversal 过严导致 0 交易
- `20260531_101403` `v002`：trades 15, reason 固定止损亏损吞噬 ROI 收益。
- `20260531_110237` `v002`：trades 19, reason Profit factor 低于 baseline；固定止损亏损吞噬 ROI 收益。
- `20260531_111202` `v002`：trades 0, reason Profit factor 低于 baseline
- `20260531_115919` `v002`：trades 0, reason Profit factor 低于 baseline
- `20260531_115919` `v005`：trades 0, reason 训练区间回测失败
### breakout_momentum 高频亏损
- `20260531_085339` `v003`：trades 27, train -0.441%
- `20260531_091409` `v003`：trades 195, train -2.922%
- `20260531_101403` `v003`：trades 148, train -2.743%
- `20260531_103001` `v003`：trades 170, train -2.501%
- `20260531_110237` `v003`：trades 172, train -2.952%
### low_volatility_mean_reversion 没有真正低频
- `20260531_101403` `v004`：trades 125 > max 60
- `20260531_111202` `v004`：trades 64 > max 60
### strict_risk_filter 交易数不足但仍亏
- `20260531_103001` `v005`：trades 5, train -0.331%
- `20260531_111202` `v005`：trades 13, train -0.172%
### 代码生成运行时错误
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。
### advisor_rules 没有完整加载
- `20260531_023806` `run`：日志提到 advisor_rules 加载缺失/不一致
- `20260531_025322` `run`：日志提到 advisor_rules 加载缺失/不一致
- `20260531_032659` `run`：日志提到 advisor_rules 加载缺失/不一致
- `20260531_035556` `run`：日志提到 advisor_rules 加载缺失/不一致
- `20260531_042952` `run`：日志提到 advisor_rules 加载缺失/不一致
### family_leaderboard 统计不准确
- `20260531_110237` `run`：日志提到 family_leaderboard 统计异常
- `20260531_111202` `run`：日志提到 family_leaderboard 统计异常
- `20260531_114018` `run`：日志提到 family_leaderboard 统计异常
- `20260531_115919` `run`：日志提到 family_leaderboard 统计异常
- `20260531_122748` `run`：日志提到 family_leaderboard 统计异常
### pre_run 建议没有真正影响 parent
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。
### bad family 被错误推荐
- `not_found` `n/a`：本次可解析结构化日志中未找到直接证据；建议增强日志字段。

## 当前系统问题

- **advisor_rules 实际加载数量和配置不一致**（medium）：证据=[{'run_id': '20260531_023806', 'version': 'run', 'evidence': '日志提到 advisor_rules 加载缺失/不一致'}, {'run_id': '20260531_025322', 'version': 'run', 'evidence': '日志提到 advisor_rules 加载缺失/不一致'}, {'run_id': '20260531_032659', 'version': 'run', 'evidence': '日志提到 advisor_rules 加载缺失/不一致'}]；建议=启动时打印配置数、文件数、合并后规则数并失败即中止
- **strategy_family_leaderboard 统计不准确**（high）：证据=[{'run_id': '20260531_110237', 'version': 'run', 'evidence': '日志提到 family_leaderboard 统计异常'}, {'run_id': '20260531_111202', 'version': 'run', 'evidence': '日志提到 family_leaderboard 统计异常'}, {'run_id': '20260531_114018', 'version': 'run', 'evidence': '日志提到 family_leaderboard 统计异常'}]；建议=从 round_history 统一生成 family leaderboard
- **high_frequency_failure 后降权不足**（high）：证据=[{'run_id': '20260531_085339', 'created_at': '2026-05-31T09:05:40.228862', 'version': 'v003', 'strategy_name': 'MultiCoin_AI_Strategy_20260531_085339_v003', 'strategy_family': 'breakout_momentum', 'mutation_type': 'reduce_trade_frequency', 'parent_source': 'baseline', 'total_trades': 27, 'profit_total_pct': -0.441052206, 'profit_total_abs': -4.41052206, 'profit_factor': 0.3755985169029096, 'max_drawdown_pct': 0.5123341192346951, 'roi_profit_abs': 2.3293253, 'stoploss_profit_abs': 0, 'moving_stop_profit_abs': -6.73984736, 'final_score': -209.62581207263412, 'valid': False, 'new_best': False, 'failure_reason': 'Profit factor 低于 baseline', 'validation_avg_profit_pct': None, 'validation_worst_profit_pct': None, 'validation_avg_pf': None, 'validation_total_trades': 0, 'validation_metrics': [{'period': 'valid_apr_2026', 'timerange': '20260401-20260430', 'metrics': {'total_trades': 43, 'profit_total_abs': -7.05467964, 'profit_total': -0.0070546796399999995, 'profit_total_pct': -0.705467964, 'profit_factor': 0.3756298711511648, 'max_drawdown': 0.007208911165269664, 'max_drawdown_pct': 0.7208911165269665, 'winrate': 0.32558139534883723, 'parsed': True, 'roi_count': 9, 'roi_profit_abs': 3.46683887, 'stop_loss_count': 0, 'stop_loss_profit_abs': 0.0, 'trailing_stop_loss_count': 34, 'trailing_stop_loss_profit_abs': -10.521518510000002, 'force_exit_count': 0, 'force_exit_profit_abs': 0.0, 'exit_signal_count': 0, 'exit_signal_profit_abs': 0.0, 'pairs': [{'key': 'BNB/USDT', 'trades': 1, 'profit_mean': -0.007933617209148213, 'profit_mean_pct': -0.79, 'profit_total_abs': -0.39727532, 'profit_total': -0.00039727531999999996, 'profit_total_pct': -0.04, 'duration_avg': '0:55:00', 'wins': 0, 'draws': 0, 'losses': 1, 'winrate': 0.0, 'cagr': -0.004988697768540273, 'expectancy': -0.39727532, 'expectancy_ratio': -1.0, 'sortino': -100.0, 'sharpe': -100.0, 'calmar': -65.87921784325953, 'sqn': -100.0, 'profit_factor': 0.0, 'max_drawdown_account': 0.0003972753199999488, 'max_drawdown_abs': 0.39727532}, {'key': 'SOL/USDT', 'trades': 11, 'profit_mean': -0.0011191781139816071, 'profit_mean_pct': -0.11, 'profit_total_abs': -0.6164707800000001, 'profit_total': -0.0006164707800000001, 'profit_total_pct': -0.06, 'duration_avg': '0:30:00', 'wins': 5, 'draws': 0, 'losses': 6, 'winrate': 0.45454545454545453, 'cagr': -0.007731379315778497, 'expectancy': -0.05604279818181812, 'expectancy_ratio': -0.13574971797567692, 'sortino': -17.503092239308984, 'sharpe': -0.9808415339334281, 'calmar': -27.362555581436283, 'sqn': -0.428, 'profit_factor': 0.7511255170445924, 'max_drawdown_account': 0.0014842404865564496, 'max_drawdown_abs': 1.4855303800000002}, {'key': 'BTC/USDT', 'trades': 4, 'profit_mean': -0.004273785843091909, 'profit_mean_pct': -0.43, 'profit_total_abs': -0.85603471, 'profit_total': -0.00085603471, 'profit_total_pct': -0.09, 'duration_avg': '0:32:00', 'wins': 1, 'draws': 0, 'losses': 3, 'winrate': 0.25, 'cagr': -0.010720960580210104, 'expectancy': -0.21400867750000002, 'expectancy_ratio': -0.5321042062947017, 'sortino': -243.460378935415, 'sharpe': -1.7301634142249431, 'calmar': -65.8792178432497, 'sqn': -1.1372, 'profit_factor': 0.2905277249403978, 'max_drawdown_account': 0.0008560347100000172, 'max_drawdown_abs': 0.85603471}, {'key': 'DOGE/USDT', 'trades': 11, 'profit_mean': -0.004705012522858537, 'profit_mean_pct': -0.47, 'profit_total_abs': -2.59163852, 'profit_total': -0.00259163852, 'profit_total_pct': -0.26, 'duration_avg': '0:32:00', 'wins': 3, 'draws': 0, 'losses': 8, 'winrate': 0.2727272727272727, 'cagr': -0.03213362150861998, 'expectancy': -0.23560350181818182, 'expectancy_ratio': -0.5907441966946136, 'sortino': -651.8646771264386, 'sharpe': -6.350668097966592, 'calmar': -64.7737805923384, 'sqn': -2.7713, 'profit_factor': 0.1877267295449063, 'max_drawdown_account': 0.0026358677395192168, 'max_drawdown_abs': 2.6359846300000003}, {'key': 'OP/USDT', 'trades': 16, 'profit_mean': -0.003236720721753883, 'profit_mean_pct': -0.32, 'profit_total_abs': -2.59326031, 'profit_total': -0.0025932603099999996, 'profit_total_pct': -0.26, 'duration_avg': '0:43:00', 'wins': 5, 'draws': 0, 'losses': 11, 'winrate': 0.3125, 'cagr': -0.032153428923255345, 'expectancy': -0.16207876937500001, 'expectancy_ratio': -0.44268584021935187, 'sortino': -23.549436633156667, 'sharpe': -5.473192784189741, 'calmar': -57.99021419669204, 'sqn': -2.011, 'profit_factor': 0.3560933233173064, 'max_drawdown_account': 0.002946048109208262, 'max_drawdown_abs': 2.9470905099999998}, {'key': 'TOTAL', 'trades': 43, 'profit_mean': -0.0032763346310423157, 'profit_mean_pct': -0.33, 'profit_total_abs': -7.05467964, 'profit_total': -0.0070546796399999995, 'profit_total_pct': -0.71, 'duration_avg': '0:36:00', 'wins': 14, 'draws': 0, 'losses': 29, 'winrate': 0.32558139534883723, 'cagr': -0.0852517029443316, 'expectancy': -0.1640623172093023, 'expectancy_ratio': -0.42108683108409806, 'sortino': -93.46148265233768, 'sharpe': -13.748172063045605, 'calmar': -64.46976057313123, 'sqn': -3.1452, 'profit_factor': 0.3756298711511648, 'max_drawdown_account': 0.007208911165269664, 'max_drawdown_abs': 7.21003108}], 'entry_tags': [{'key': 'bb_breakout_volume_surge', 'trades': 21, 'profit_mean': -0.002835714949742153, 'profit_mean_pct': -0.28, 'profit_total_abs': -2.98196388, 'profit_total': -0.0029819638799999998, 'profit_total_pct': -0.3, 'duration_avg': '0:38:00', 'wins': 8, 'draws': 0, 'losses': 13, 'winrate': 0.38095238095238093, 'cagr': -0.03689003609150365, 'expectancy': -0.14199828000000003, 'expectancy_ratio': -0.361005623029897, 'sortino': -98.59289356866253, 'sharpe': -5.884404432501299, 'calmar': -56.898805373482475, 'sqn': -1.9022, 'profit_factor': 0.41683707049016633, 'max_drawdown_account': 0.003452611118313228, 'max_drawdown_abs': 3.45424171}, {'key': 'atr_momentum_breakout', 'trades': 22, 'profit_mean': -0.003696926145010652, 'profit_mean_pct': -0.37, 'profit_total_abs': -4.07271576, 'profit_total': -0.004072715760000001, 'profit_total_pct': -0.41, 'duration_avg': '0:35:00', 'wins': 6, 'draws': 0, 'losses': 16, 'winrate': 0.2727272727272727, 'cagr': -0.0500678436106059, 'expectancy': -0.18512344363636368, 'expectancy_ratio': -0.4788622768746662, 'sortino': -41.705115702478174, 'sharpe': -7.875582359764333, 'calmar': -63.46848287949744, 'sqn': -2.4901, 'profit_factor': 0.341564369297334, 'max_drawdown_account': 0.004227410465696738, 'max_drawdown_abs': 4.2280672}, {'key': 'TOTAL', 'trades': 43, 'profit_mean': -0.0032763346310423157, 'profit_mean_pct': -0.33, 'profit_total_abs': -7.05467964, 'profit_total': -0.0070546796399999995, 'profit_total_pct': -0.71, 'duration_avg': '0:36:00', 'wins': 14, 'draws': 0, 'losses': 29, 'winrate': 0.32558139534883723, 'cagr': -0.0852517029443316, 'expectancy': -0.1640623172093023, 'expectancy_ratio': -0.42108683108409806, 'sortino': -93.46148265233768, 'sharpe': -13.748172063045605, 'calmar': -64.46976057313123, 'sqn': -3.1452, 'profit_factor': 0.3756298711511648, 'max_drawdown_account': 0.007208911165269664, 'max_drawdown_abs': 7.21003108}]}}, {'period': 'valid_mar_2026', 'timerange': '20260301-20260331', 'metrics': {'total_trades': 97, 'profit_total_abs': -25.016242499999997, 'profit_total': -0.025016242499999997, 'profit_total_pct': -2.50162425, 'profit_factor': 0.20477160955102575, 'max_drawdown': 0.025865758170000048, 'max_drawdown_pct': 2.586575817000005, 'winrate': 0.18556701030927836, 'parsed': True, 'roi_count': 13, 'roi_profit_abs': 5.65549243, 'stop_loss_count': 0, 'stop_loss_profit_abs': 0.0, 'trailing_stop_loss_count': 84, 'trailing_stop_loss_profit_abs': -30.67173493, 'force_exit_count': 0, 'force_exit_profit_abs': 0.0, 'exit_signal_count': 0, 'exit_signal_profit_abs': 0.0, 'pairs': [{'key': 'BNB/USDT', 'trades': 2, 'profit_mean': -0.007935395304054578, 'profit_mean_pct': -0.79, 'profit_total_abs': -0.79472399, 'profit_total': -0.0007947239900000001, 'profit_total_pct': -0.08, 'duration_avg': '0:10:00', 'wins': 0, 'draws': 0, 'losses': 2, 'winrate': 0.0, 'cagr': -0.009626353215738237, 'expectancy': -0.397361995, 'expectancy_ratio': -1.0, 'sortino': -263.47269490362856, 'sharpe': -263.47269490362856, 'calmar': -63.6832439151454, 'sqn': -206.8619, 'profit_factor': 0.0, 'max_drawdown_account': 0.0007947239899999659, 'max_drawdown_abs': 0.79472399}, {'key': 'BTC/USDT', 'trades': 12, 'profit_mean': -0.0051178911961267505, 'profit_mean_pct': -0.51, 'profit_total_abs': -3.0753259799999997, 'profit_total': -0.00307532598, 'profit_total_pct': -0.31, 'duration_avg': '0:40:00', 'wins': 2, 'draws': 0, 'losses': 10, 'winrate': 0.16666666666666666, 'cagr': -0.036780652784896506, 'expectancy': -0.256277165, 'expectancy_ratio': -0.6364404275881967, 'sortino': -307.9107270955441, 'sharpe': -5.9102714102736185, 'calmar': -63.683243915142604, 'sqn': -2.5651, 'profit_factor': 0.2362714868941639, 'max_drawdown_account': 0.0030753259800000024, 'max_drawdown_abs': 3.07532598}, {'key': 'OP/USDT', 'trades': 23, 'profit_mean': -0.004221704452084034, 'profit_mean_pct': -0.42, 'profit_total_abs': -4.86224193, 'profit_total': -0.0048622419299999995, 'profit_total_pct': -0.49, 'duration_avg': '0:31:00', 'wins': 5, 'draws': 0, 'losses': 18, 'winrate': 0.21739130434782608, 'cagr': -0.05757747365632504, 'expectancy': -0.21140182304347827, 'expectancy_ratio': -0.5567520600354692, 'sortino': -52.61016205700066, 'sharpe': -9.3736728224719, 'calmar': -52.36585220948504, 'sqn': -3.0017, 'profit_factor': 0.2885945899546782, 'max_drawdown_account': 0.005913077430000044, 'max_drawdown_abs': 5.9130774299999995}, {'key': 'DOGE/USDT', 'trades': 29, 'profit_mean': -0.005382730778745623, 'profit_mean_pct': -0.54, 'profit_total_abs': -7.816667059999999, 'profit_total': -0.007816667059999999, 'profit_total_pct': -0.78, 'duration_avg': '0:19:00', 'wins': 5, 'draws': 0, 'losses': 24, 'winrate': 0.1724137931034483, 'cagr': -0.0910602097249773, 'expectancy': -0.2695402434482759, 'expectancy_ratio': -0.6516688838913687, 'sortino': -120.63594394467674, 'sharpe': -15.054318149636236, 'calmar': -63.68324391514276, 'sqn': -4.3134, 'profit_factor': 0.21256676529792945, 'max_drawdown_account': 0.007816667059999987, 'max_drawdown_abs': 7.81666706}, {'key': 'SOL/USDT', 'trades': 31, 'profit_mean': -0.005454586350268407, 'profit_mean_pct': -0.55, 'profit_total_abs': -8.46728354, 'profit_total': -0.00846728354, 'profit_total_pct': -0.85, 'duration_avg': '0:58:00', 'wins': 6, 'draws': 0, 'losses': 25, 'winrate': 0.1935483870967742, 'cagr': -0.09828541899215126, 'expectancy': -0.27313817870967744, 'expectancy_ratio': -0.691487902526269, 'sortino': -69.96462499609841, 'sharpe': -20.67545479090548, 'calmar': -63.68324391514237, 'sqn': -5.7363, 'profit_factor': 0.1425550008674264, 'max_drawdown_account': 0.00846728354000004, 'max_drawdown_abs': 8.46728354}, {'key': 'TOTAL', 'trades': 97, 'profit_mean': -0.005150268626819656, 'profit_mean_pct': -0.52, 'profit_total_abs': -25.016242499999997, 'profit_total': -0.025016242499999997, 'profit_total_pct': -2.5, 'duration_avg': '0:37:00', 'wins': 18, 'draws': 0, 'losses': 79, 'winrate': 0.18556701030927836, 'cagr': -0.2652581135560479, 'expectancy': -0.2578994072164948, 'expectancy_ratio': -0.6476602355202986, 'sortino': -275.3548282741969, 'sharpe': -51.711115206082084, 'calmar': -61.59167894856473, 'sqn': -8.2021, 'profit_factor': 0.20477160955102575, 'max_drawdown_account': 0.025865758170000048, 'max_drawdown_abs': 25.865758169999996}], 'entry_tags': [{'key': 'atr_momentum_breakout', 'trades': 49, 'profit_mean': -0.004944832072098137, 'profit_mean_pct': -0.49, 'profit_total_abs': -12.13299762, 'profit_total': -0.012132997619999999, 'profit_total_pct': -1.21, 'duration_avg': '0:36:00', 'wins': 10, 'draws': 0, 'losses': 39, 'winrate': 0.20408163265306123, 'cagr': -0.1380180714797914, 'expectancy': -0.2476121963265306, 'expectancy_ratio': -0.6242140531440965, 'sortino': -94.87872014683724, 'sharpe': -24.511029318448085, 'calmar': -60.82575563574102, 'sqn': -5.442, 'profit_factor': 0.21573106143434026, 'max_drawdown_account': 0.01270298476000005, 'max_drawdown_abs': 12.702984760000001}, {'key': 'bb_breakout_volume_surge', 'trades': 48, 'profit_mean': -0.005359985109764538, 'profit_mean_pct': -0.54, 'profit_total_abs': -12.88324488, 'profit_total': -0.01288324488, 'profit_total_pct': -1.29, 'duration_avg': '0:38:00', 'wins': 8, 'draws': 0, 'losses': 40, 'winrate': 0.16666666666666666, 'cagr': -0.14594920012868606, 'expectancy': -0.268400935, 'expectancy_ratio': -0.6715278512732268, 'sortino': -692.4072994718099, 'sharpe': -27.326471885406217, 'calmar': -62.33084780508677, 'sqn': -6.1287, 'profit_factor': 0.19416657847212795, 'max_drawdown_account': 0.01316277341, 'max_drawdown_abs': 13.162773410000003}, {'key': 'TOTAL', 'trades': 97, 'profit_mean': -0.005150268626819656, 'profit_mean_pct': -0.52, 'profit_total_abs': -25.016242499999997, 'profit_total': -0.025016242499999997, 'profit_total_pct': -2.5, 'duration_avg': '0:37:00', 'wins': 18, 'draws': 0, 'losses': 79, 'winrate': 0.18556701030927836, 'cagr': -0.2652581135560479, 'expectancy': -0.2578994072164948, 'expectancy_ratio': -0.6476602355202986, 'sortino': -275.3548282741969, 'sharpe': -51.711115206082084, 'calmar': -61.59167894856473, 'sqn': -8.2021, 'profit_factor': 0.20477160955102575, 'max_drawdown_account': 0.025865758170000048, 'max_drawdown_abs': 25.865758169999996}]}}, {'period': 'valid_feb_2026', 'timerange': '20260201-20260228', 'metrics': {'total_trades': 116, 'profit_total_abs': -26.755886450000002, 'profit_total': -0.026755886450000002, 'profit_total_pct': -2.6755886450000004, 'profit_factor': 0.2592917042704054, 'max_drawdown': 0.027328641285279603, 'max_drawdown_pct': 2.7328641285279605, 'winrate': 0.25, 'parsed': True, 'roi_count': 17, 'roi_profit_abs': 7.4669013799999995, 'stop_loss_count': 0, 'stop_loss_profit_abs': 0.0, 'trailing_stop_loss_count': 99, 'trailing_stop_loss_profit_abs': -34.22278783, 'force_exit_count': 0, 'force_exit_profit_abs': 0.0, 'exit_signal_count': 0, 'exit_signal_profit_abs': 0.0, 'pairs': [{'key': 'BNB/USDT', 'trades': 12, 'profit_mean': -0.00570919382470213, 'profit_mean_pct': -0.57, 'profit_total_abs': -3.4306210300000006, 'profit_total': -0.0034306210300000008, 'profit_total_pct': -0.34, 'duration_avg': '1:10:00', 'wins': 2, 'draws': 0, 'losses': 10, 'winrate': 0.16666666666666666, 'cagr': -0.04539405542990116, 'expectancy': -0.2858850858333333, 'expectancy_ratio': -0.7178704652390653, 'sortino': -419.64996343619566, 'sharpe': -9.581999104397118, 'calmar': -66.92688624100158, 'sqn': -3.7427, 'profit_factor': 0.13855544171312165, 'max_drawdown_account': 0.0036270604486745173, 'max_drawdown_abs': 3.6277755400000005}, {'key': 'BTC/USDT', 'trades': 19, 'profit_mean': -0.0041177507970354965, 'profit_mean_pct': -0.41, 'profit_total_abs': -3.9177128199999998, 'profit_total': -0.00391771282, 'profit_total_pct': -0.39, 'duration_avg': '0:36:00', 'wins': 5, 'draws': 0, 'losses': 14, 'winrate': 0.2631578947368421, 'cagr': -0.05168228396621266, 'expectancy': -0.20619541157894739, 'expectancy_ratio': -0.5147712127278841, 'sortino': -114152.11019201568, 'sharpe': -8.268377638277958, 'calmar': -70.7591599057137, 'sqn': -2.6093, 'profit_factor': 0.301381925583586, 'max_drawdown_account': 0.0039177128200000196, 'max_drawdown_abs': 3.9177128199999998}, {'key': 'OP/USDT', 'trades': 31, 'profit_mean': -0.004086236392565386, 'profit_mean_pct': -0.41, 'profit_total_abs': -6.3431656300000006, 'profit_total': -0.0063431656300000005, 'profit_total_pct': -0.63, 'duration_avg': '0:24:00', 'wins': 8, 'draws': 0, 'losses': 23, 'winrate': 0.25806451612903225, 'cagr': -0.08242716818230333, 'expectancy': -0.20461824612903223, 'expectancy_ratio': -0.4765225716472662, 'sortino': -53.86538117762954, 'sharpe': -10.039229415879078, 'calmar': -59.29322562480284, 'sqn': -2.5068, 'profit_factor': 0.3577304469102064, 'max_drawdown_account': 0.007569786706524656, 'max_drawdown_abs': 7.579142789999999}, {'key': 'SOL/USDT', 'trades': 31, 'profit_mean': -0.004108178994471433, 'profit_mean_pct': -0.41, 'profit_total_abs': -6.37722401, 'profit_total': -0.00637722401, 'profit_total_pct': -0.64, 'duration_avg': '0:43:00', 'wins': 9, 'draws': 0, 'losses': 22, 'winrate': 0.2903225806451613, 'cagr': -0.08285224168869532, 'expectancy': -0.20571690354838712, 'expectancy_ratio': -0.5140493757982263, 'sortino': -86.04149224378865, 'sharpe': -14.141285813909109, 'calmar': -68.62007913605721, 'sqn': -3.5311, 'profit_factor': 0.275657697738863, 'max_drawdown_account': 0.006576020009878364, 'max_drawdown_abs': 6.57733595}, {'key': 'DOGE/USDT', 'trades': 23, 'profit_mean': -0.005806214996425909, 'profit_mean_pct': -0.58, 'profit_total_abs': -6.687162960000001, 'profit_total': -0.006687162960000001, 'profit_total_pct': -0.67, 'duration_avg': '0:22:00', 'wins': 5, 'draws': 0, 'losses': 18, 'winrate': 0.21739130434782608, 'cagr': -0.08671213484069007, 'expectancy': -0.2907462156521739, 'expectancy_ratio': -0.6665527861540753, 'sortino': -79.28388697216877, 'sharpe': -16.61761714966997, 'calmar': -70.75915990571384, 'sqn': -4.7893, 'profit_factor': 0.14829366213645934, 'max_drawdown_account': 0.006687162960000023, 'max_drawdown_abs': 6.687162960000001}, {'key': 'TOTAL', 'trades': 116, 'profit_mean': -0.004606184594448597, 'profit_mean_pct': -0.46, 'profit_total_abs': -26.755886450000002, 'profit_total': -0.026755886450000002, 'profit_total_pct': -2.68, 'duration_avg': '0:35:00', 'wins': 29, 'draws': 0, 'losses': 87, 'winrate': 0.25, 'cagr': -0.30693177000771676, 'expectancy': -0.23065419353448272, 'expectancy_ratio': -0.5555312217971959, 'sortino': -318.15981292700025, 'sharpe': -53.931137345987935, 'calmar': -69.27618639988705, 'sqn': -7.0461, 'profit_factor': 0.2592917042704054, 'max_drawdown_account': 0.027328641285279603, 'max_drawdown_abs': 27.34473368}], 'entry_tags': [{'key': 'atr_momentum_breakout', 'trades': 44, 'profit_mean': -0.0047802960837863215, 'profit_mean_pct': -0.48, 'profit_total_abs': -10.532401179999999, 'profit_total': -0.01053240118, 'profit_total_pct': -1.05, 'duration_avg': '0:30:00', 'wins': 10, 'draws': 0, 'losses': 34, 'winrate': 0.22727272727272727, 'cagr': -0.13336517251650215, 'expectancy': -0.23937275409090908, 'expectancy_ratio': -0.5872961105592085, 'sortino': -197.71627662615882, 'sharpe': -20.442439477428106, 'calmar': -70.7591599057143, 'sqn': -4.3056, 'profit_factor': 0.2399697392763185, 'max_drawdown_account': 0.010532401179999965, 'max_drawdown_abs': 10.532401179999997}, {'key': 'bb_breakout_volume_surge', 'trades': 72, 'profit_mean': -0.00449978312874221, 'profit_mean_pct': -0.45, 'profit_total_abs': -16.22348527, 'profit_total': -0.01622348527, 'profit_total_pct': -1.62, 'duration_avg': '0:39:00', 'wins': 19, 'draws': 0, 'losses': 53, 'winrate': 0.2638888888888889, 'cagr': -0.1983763229307801, 'expectancy': -0.22532618430555562, 'expectancy_ratio': -0.5363907200701195, 'sortino': -165.00851576775298, 'sharpe': -33.52598115603236, 'calmar': -68.32104716136462, 'sqn': -5.5449, 'profit_factor': 0.2713182670745548, 'max_drawdown_account': 0.016802438430672875, 'max_drawdown_abs': 16.8123325}, {'key': 'TOTAL', 'trades': 116, 'profit_mean': -0.004606184594448597, 'profit_mean_pct': -0.46, 'profit_total_abs': -26.755886450000002, 'profit_total': -0.026755886450000002, 'profit_total_pct': -2.68, 'duration_avg': '0:35:00', 'wins': 29, 'draws': 0, 'losses': 87, 'winrate': 0.25, 'cagr': -0.30693177000771676, 'expectancy': -0.23065419353448272, 'expectancy_ratio': -0.5555312217971959, 'sortino': -318.15981292700025, 'sharpe': -53.931137345987935, 'calmar': -69.27618639988705, 'sqn': -7.0461, 'profit_factor': 0.2592917042704054, 'max_drawdown_account': 0.027328641285279603, 'max_drawdown_abs': 27.34473368}]}}], 'path': 'logs/2026-05-31/run_20260531_085339', 'near_miss': False, 'risk_control_near_miss': False, '_candidate_score': -4.332104412}, {'run_id': '20260531_091409', 'created_at': '2026-05-31T09:28:35.564199', 'version': 'v003', 'strategy_name': 'MultiCoin_AI_Strategy_20260531_091409_v003', 'strategy_family': 'breakout_momentum', 'mutation_type': 'remove_bad_entry_condition', 'parent_source': 'baseline', 'total_trades': 195, 'profit_total_pct': -2.9222947099999996, 'profit_total_abs': -29.222947099999995, 'profit_factor': 0.5023707495133826, 'max_drawdown_pct': 3.3830903526309157, 'roi_profit_abs': 29.50138848, 'stoploss_profit_abs': -58.368238260000005, 'moving_stop_profit_abs': 0, 'final_score': 0.0, 'valid': False, 'new_best': False, 'failure_reason': '训练区间亏损超过 baseline；Profit factor 低于 baseline；最大回撤超过目标；交易数超过目标上限；固定止损亏损吞噬 ROI 收益。；高频风险：交易数超过目标上限 1.5 倍', 'validation_avg_profit_pct': None, 'validation_worst_profit_pct': None, 'validation_avg_pf': None, 'validation_total_trades': 0, 'validation_metrics': [], 'path': 'logs/2026-05-31/run_20260531_091409', 'near_miss': False, 'risk_control_near_miss': True, '_candidate_score': -16.238146252433086}, {'run_id': '20260531_101403', 'created_at': '2026-05-31T10:27:47.920613', 'version': 'v003', 'strategy_name': 'MultiCoin_AI_Strategy_20260531_101403_v003', 'strategy_family': 'breakout_momentum', 'mutation_type': 'adjust_roi', 'parent_source': 'baseline', 'total_trades': 148, 'profit_total_pct': -2.743181275, 'profit_total_abs': -27.43181275, 'profit_factor': 0.13363069803450986, 'max_drawdown_pct': 2.7431812749999946, 'roi_profit_abs': 3.4443317799999997, 'stoploss_profit_abs': 0, 'moving_stop_profit_abs': -30.435090549999998, 'final_score': 0.0, 'valid': False, 'new_best': False, 'failure_reason': '训练区间亏损超过 baseline；Profit factor 低于 baseline；交易数超过目标上限；移动止盈/止损结构造成大额亏损。；高频风险：交易数超过目标上限 1.5 倍', 'validation_avg_profit_pct': None, 'validation_worst_profit_pct': None, 'validation_avg_pf': None, 'validation_total_trades': 0, 'validation_metrics': [], 'path': 'logs/2026-05-31/run_20260531_101403', 'near_miss': False, 'risk_control_near_miss': False, '_candidate_score': -15.575}]；建议=breakout_momentum 高频/亏损后至少冷却 3 个 run

## 下一步优化建议
### A. 立即改配置
- strategy_family 权重：trend_following 最高；low_volatility_mean_reversion 小权重探索；pullback_reversal 低权重且放宽；breakout_momentum/strict_risk_filter 暂停或权重 0。
- 交易数目标保持 min 25 / ideal 35-60 / max 80，不建议直接放松 hard gate；新增 validation_max_trades 总量限制。
- pair 配置暂保留 5 币但对 DOGE/BNB 设置冷却观察；若 pair_attribution 缺失先重跑 pair-scan。
- advisor/codegen rules 增加 stoploss_to_roi_ratio、worst_month_penalty、validation_high_frequency_failure 明确硬约束。
- provider_pool 优先稳定代码生成模型；遇到 KeyError 类 runtime_error 的 provider/mutation 降权。

### B. 建议改代码逻辑
- 实现 pair_attribution.json：按 pair 记录 ROI/stoploss/trailing/validation month 贡献。
- 重写 strategy_family_leaderboard 统计源：直接从 round_history 聚合 high_frequency_failure、zero_trade、runtime_error、near_miss。
- 实现 near-miss follow-up：风控型 near-miss 自动生成收紧 stoploss/entry filter 的下一轮 mutation。
- parent selection 加校验：pre_run 推荐、nearest_candidate、actual_session_parent 三者写入并解释差异。
- runtime static check：检查 Bollinger/indicator 字段名、populate_* 使用列是否已创建。
- scoring formula 增加 worst-month 惩罚、stoploss_to_roi_ratio 惩罚、validation trade explosion 惩罚。

### C. 下一轮运行策略
- mode: explore-strategy-family for 1 diagnostic run, then ordinary optimize
- iterations: 3
- family_focus: ['trend_following']
- pause_families: ['breakout_momentum', 'strict_risk_filter']
- use_nearest_candidate: True
- pair_plan: 先 pair-scan 或保留 5 币但启用 pair attribution；不建议立刻切回 OP/SOL，除非 pair-scan 证明 BTC/BNB/DOGE 持续拖累
- goal: 找到训练 35-60 笔、PF>1、stoploss_to_roi<0.8、202603/202604 worst month 不亏或小亏的稳健 parent

## 给 Codex 的修改任务
详见 `codex_next_optimization_tasks.md`。

## 最终 10 问明确回答

1. **历史上最值得继续的父策略是哪一个？** nearest_candidate 中排名最高的风控型 near-miss；若必须落到已有 champion，则以 historical_best 作为安全基线但不要继续原样突变。
2. **当前应该继续使用 historical_best 还是 nearest_candidate？** 使用 nearest_candidate 作为 actual_session_parent，同时保留 historical_best 作 hard-gate 对照。
3. **当前最值得继续的 strategy_family 是哪个？** trend_following。
4. **哪些 family 应该暂停？** breakout_momentum、strict_risk_filter；pullback_reversal 降权重改造后再跑。
5. **哪些 mutation_type 应该加入黑名单？** 导致高频爆炸的 breakout_momentum 相关 mutation、重复 fingerprint 的 pair_specific_filter、造成 Bollinger/字段 KeyError 的 indicator mutation、无交易的过严 pullback mutation。
6. **当前 5 个币是否合理？** 暂时可保留但证据不足；需要 pair_attribution 后决定是否冷却 DOGE/BNB 或切回 OP/SOL。
7. **当前交易数目标是否合理？** min 25 / max 80 大体合理；应增加 ideal 35-60 和验证期交易数上限，不建议直接放松。
8. **当前 official best 硬门槛是否合理？** 方向合理但缺少 worst-month、stoploss_to_roi、validation_high_frequency 硬约束；不要放松，应补充。
9. **下一轮应该跑几轮、用什么模式？** 先 3 轮 explore-strategy-family 诊断（仅 trend_following + 小样本 low_volatility），随后普通 optimize。
10. **当前最优先让 Codex 修的 3 个问题是什么？** pair_attribution/stoploss归因、family_leaderboard 真实统计与降权、parent selection/near-miss follow-up 闭环。
