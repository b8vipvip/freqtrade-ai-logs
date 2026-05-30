# Combined Model Artifacts
Source directory: `model_artifacts/model_export_20260530_054626_19adfe7d`
Included models: `deepseek-v4-pro`, `glm-5.1`, `claude-opus-4-7`, `gpt-5.5`
Each section is annotated with the model name, an item sequence number, artifact type, and source path.

## Model: deepseek-v4-pro

### deepseek-v4-pro - 序号 01 - mutation_spec.json
- 模型名称: `deepseek-v4-pro`
- 序号: `01`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/mutation_spec/01_run_20260530_043127_v013_mutation_spec.json`

```json
{
  "mutation_type": "cooldown_or_protection",
  "reason": "当前最优策略 v001 的训练成绩显示，固定止损亏损（-7.58 USDT）严重侵蚀了 ROI 收益（6.32 USDT）。已失败的 mutation_type（如 adjust_stoploss, tighten_entry_trigger 等）均未能通过减少止损损失来改善整体收益。本轮未失败的 mutation_type 中，cooldown_or_protection 能够通过主动冷却机制，在发生止损后阻止同一交易对短期内再次入场，从而避免连续止损造成的资金大幅回撤。这同时符合跨 run 教训“减少 stoploss 损失，验证区间优先稳健”和本次 session 的硬约束（不增加交易频率、不单纯调整止损）。",
  "expected_effect": "预计将止损次数从 4 次减少至 2–3 次，使总止损亏损降至 5 USDT 以下，同时由于冷却期会过滤部分低质量信号，总交易数可能从 25 下降至 23–25，仍保持在 20 以上，不会触发验证跳过。这将直接提升净盈亏和 profit_factor，有望使 profit_total_pct 回正，并保持 max_drawdown_pct 在 3% 以内。",
  "changes": {
    "protection": {
      "add": [
        {
          "method": "StoplossGuard",
          "lookback_period_candles": 18,
          "trade_limit": 1,
          "stop_duration_candles": 12,
          "only_per_pair": true
        }
      ]
    }
  },
  "do_not_change": [
    "minimal_roi",
    "stoploss",
    "trailing_stop",
    "use_exit_signal",
    "timeframe",
    "entry_trigger_conditions",
    "entry_filters",
    "volume_filter",
    "pair_list",
    "indicator_set"
  ]
}
```

### deepseek-v4-pro - 序号 02 - strategy.py
- 模型名称: `deepseek-v4-pro`
- 序号: `02`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/strategy_py/01_run_20260530_043127_v013_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_043127_v013(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.02,
        "25": 0.015,
        "50": 0.01,
        "100": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 18,
                "trade_limit": 1,
                "stop_duration_candles": 12,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        atr_filter = (dataframe["atr_ratio"] < 1.3)

        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            long_condition = None
        elif pair == "ETH/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30)
            )
        elif pair == "XRP/USDT":
            long_condition = None
        elif pair == "DOGE/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### deepseek-v4-pro - 序号 03 - mutation_spec.json
- 模型名称: `deepseek-v4-pro`
- 序号: `03`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/mutation_spec/02_run_20260530_043127_v012_mutation_spec.json`

```json
{
  "strategy_name": "MultiCoin_AI_Strategy_20260530_043127_v012",
  "mutation_type": "reduce_trade_frequency",
  "reason": "本轮所有直接减损的 mutation_type（add_entry_filter、tighten_entry_trigger 等）已失败或重复，且严格禁止调整止损宽度。选择未尝试的 reduce_trade_frequency，通过提高入场标准过滤弱势信号，间接降低止损发生频率，从而减少固定止损对 ROI 的吞噬。",
  "expected_effect": "训练交易数预计从25降至20-24，仍满足 min_trades=25 的候选线；止损次数有望减少，profit_factor 可能回升至 >0.85，同时保持低回撤。",
  "changes": [
    "新增入场过滤：ADX > 25，仅在有趋势的市场入场",
    "新增入场过滤：收盘价位于 EMA50 上方，避免逆势",
    "延长 CooldownPeriod stop_duration_candles 从 3 至 5，降低连续入场"
  ],
  "do_not_change": [
    "不修改 stoploss",
    "不修改 ROI 表",
    "不修改 trailing_stop",
    "不启用 exit_signal"
  ]
}
```

### deepseek-v4-pro - 序号 04 - strategy.py
- 模型名称: `deepseek-v4-pro`
- 序号: `04`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/strategy_py/02_run_20260530_043127_v012_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_043127_v012(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    # Minimal ROI adjusted according to mutation spec
    minimal_roi = {
        "0": 0.02,
        "25": 0.015,
        "50": 0.01,
        "100": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 5,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        # Bollinger Bands for volatility contraction filter
        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        # MACD histogram rising (current > previous)
        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        # Bollinger %B
        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        # ATR-based volatility filter
        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        # ATR volatility filter: skip entries when volatility is spiking
        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum condition
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        # Additional mandatory filters from mutation spec:
        # Only enter in trending markets and when price is above EMA50
        mandatory_filter = (
            (dataframe["adx"] > 25) &
            (dataframe["close"] > dataframe["ema_50"])
        )

        if pair == "BTC/USDT":
            # Block BTC/USDT - too many stoplosses
            long_condition = None
        elif pair == "ETH/USDT":
            # Stricter ETH: require EMA20 > EMA50 and close > EMA20 for strong short-term trend
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30)
            )
        elif pair == "XRP/USDT":
            # Block XRP/USDT - too many stoplosses
            long_condition = None
        elif pair == "DOGE/USDT":
            # Keep DOGE unchanged
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: slightly relaxed RSI upper bound to capture a few more trend entries
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            # For others: use the base
            long_condition = macd_momentum

        # Apply the mandatory global filters to every active pair
        if long_condition is not None:
            long_condition = long_condition & mandatory_filter

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### deepseek-v4-pro - 序号 05 - mutation_spec.json
- 模型名称: `deepseek-v4-pro`
- 序号: `05`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/mutation_spec/03_run_20260530_043127_v011_mutation_spec.json`

```json
{
  "mutation_type": "cooldown_or_protection",
  "reason": "冠军策略v001的4次止损亏损-7.58吞噬了全部ROI利润(6.32)，训练交易数25已达下限，直接调整止损值曾导致重复策略，收紧入场过滤可能使交易数低于25。本轮通过强化StopLossGuard和MaxDrawdown保护，在不触动止损参数的前提下减少连续止损次数，从而降低止损净亏损，同时控制回撤。",
  "expected_effect": "减少连续止损触发次数，降低stop_loss总亏损，提升profit_factor，轻微减少总交易数但预计仍保留在25~80区间，max_drawdown可能下降。",
  "changes": "StoplossGuard.trade_limit从2降至1；StoplossGuard.stop_duration_candles从8增至10；MaxDrawdown.max_allowed_drawdown从0.02降至0.015；MaxDrawdown.stop_duration_candles从6增至8。",
  "do_not_change": "stoploss=-0.025；minimal_roi阶梯表；trailing_stop=True；use_exit_signal=False；入场条件及指标组合；pair_filters；timeframe=5m；均保持不变。"
}
```

### deepseek-v4-pro - 序号 06 - strategy.py
- 模型名称: `deepseek-v4-pro`
- 序号: `06`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/deepseek-v4-pro/strategy_py/03_run_20260530_043127_v011_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_043127_v011(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    # Minimal ROI adjusted according to mutation spec
    minimal_roi = {
        "0": 0.02,
        "25": 0.015,
        "50": 0.01,
        "100": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 8,          # 6 -> 8
                "max_allowed_drawdown": 0.015,        # 0.02 -> 0.015
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 1,                     # 2 -> 1
                "stop_duration_candles": 10,          # 8 -> 10
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        # Bollinger Bands for volatility contraction filter
        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        # MACD histogram rising (current > previous)
        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        # Bollinger %B
        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        # ATR-based volatility filter
        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        # ATR volatility filter: skip entries when volatility is spiking
        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum condition
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            # Block BTC/USDT - too many stoplosses
            long_condition = None
        elif pair == "ETH/USDT":
            # Stricter ETH: require EMA20 > EMA50 and close > EMA20 for strong short-term trend
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30)
            )
        elif pair == "XRP/USDT":
            # Block XRP/USDT - too many stoplosses
            long_condition = None
        elif pair == "DOGE/USDT":
            # Keep DOGE unchanged
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: slightly relaxed RSI upper bound to capture a few more trend entries
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            # For others: use the base
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

## Model: glm-5.1

### glm-5.1 - 序号 01 - mutation_spec.json
- 模型名称: `glm-5.1`
- 序号: `01`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/mutation_spec/01_run_20260530_052915_v005_mutation_spec.json`

```json
{
  "mutation_type": "remove_bad_entry_condition",
  "reason": "ETH/USDT 贡献 9 笔交易、PF 仅 0.44、亏损 -2.14 USDT，是拖累整体 PF 的核心来源。4 笔 stoploss 合计 -7.58 USDT 完全吞噬 ROI 收益。此前 add_entry_filter / tighten_entry_trigger / pair_specific_filter / adjust_roi 均失败，因为它们没有精准移除导致 stoploss 的劣质入场条件。本轮选择 remove_bad_entry_condition，直接剔除 MACD 柱状图递减时的入场（假突破）和 RSI>65 超买区入场（易反转触发止损），从源头减少 stoploss 命中率。",
  "expected_effect": "stoploss 笔数从 4 降至 1-2，profit_factor 从 0.85 提升至 >1.0，总交易数维持 25-40 区间，max_drawdown_pct 降至 <1.0%",
  "changes": {
    "remove_conditions": [
      "移除 MACD histogram 在入场前 2 根 K 线内递减时的入场许可（假金叉）",
      "移除 RSI > 65 时的做多入场（超买区易反转触发固定止损）",
      "移除价格低于 EMA200 时的 macd_cross_momentum 入场（逆势入场止损率高）"
    ],
    "add_guard_conditions": [
      "要求 MACD histogram 在当前 K 线 > 前一根 K 线（动量确认递增）",
      "要求 RSI 在 40-65 区间（健康多头区域）",
      "要求 close > EMA200（趋势过滤，避免逆势入场）"
    ],
    "target_pair_focus": "ETH/USDT 的 macd_cross_momentum 入口条件收紧，SOL/USDT 保持现有逻辑不变"
  },
  "do_not_change": [
    "minimal_roi",
    "stoploss 值",
    "trailing_stop 配置",
    "timeframe",
    "SOL/USDT 入场逻辑",
    "use_exit_signal"
  ]
}
```

### glm-5.1 - 序号 02 - strategy.py
- 模型名称: `glm-5.1`
- 序号: `02`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/strategy_py/01_run_20260530_052915_v005_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_052915_v005(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.02,
        "25": 0.015,
        "50": 0.01,
        "100": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum: tightened RSI<65 (avoid overbought reversal),
        # added macd_histogram rising guard (avoid fake golden cross)
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 65) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            long_condition = None
        elif pair == "ETH/USDT":
            # ETH tightened: add macd_hist_rising (2-bar momentum confirm),
            # RSI now 45-65 via base, close>ema200 already present
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "XRP/USDT":
            long_condition = None
        elif pair == "DOGE/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: unchanged per mutation spec
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### glm-5.1 - 序号 03 - mutation_spec.json
- 模型名称: `glm-5.1`
- 序号: `03`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/mutation_spec/02_run_20260530_052915_v004_mutation_spec.json`

```json
{
  "mutation_type": "adjust_roi",
  "reason": "当前champion的ROI收益6.32 USDT被止损亏损-7.58 USDT吞噬。ETH/USDT的9笔交易中2笔止损导致整体亏损-2.14 USDT。通过调整ROI更激进（更快锁定小利润），可以减少持仓时间，降低被止损的概率，从而减少止损亏损对整体收益的侵蚀。上一轮add_entry_filter/pair_specific_filter/tighten_entry_trigger均因高度重复失败，需要从退出机制入手改变策略指纹。",
  "expected_effect": "减少止损亏损占比，提高profit_factor从0.85到1.0以上，总收益转正。ROI退出比例增加，止损退出比例减少，整体收益改善。",
  "changes": {
    "minimal_roi": {
      "0": 0.012,
      "15": 0.008,
      "30": 0.005,
      "60": 0.003
    }
  },
  "do_not_change": [
    "stoploss",
    "trailing_stop",
    "trailing_stop_positive",
    "trailing_stop_positive_offset",
    "trailing_only_offset_is_reached",
    "use_exit_signal",
    "entry_conditions",
    "protection_cooldown",
    "timeframe",
    "indicators"
  ]
}
```

### glm-5.1 - 序号 04 - strategy.py
- 模型名称: `glm-5.1`
- 序号: `04`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/strategy_py/02_run_20260530_052915_v004_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_052915_v004(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    # More aggressive ROI to lock in profits faster and reduce stoploss exposure
    minimal_roi = {
        "0": 0.012,
        "15": 0.008,
        "30": 0.005,
        "60": 0.003,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        atr_filter = (dataframe["atr_ratio"] < 1.3)

        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            long_condition = None
        elif pair == "ETH/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30)
            )
        elif pair == "XRP/USDT":
            long_condition = None
        elif pair == "DOGE/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### glm-5.1 - 序号 05 - mutation_spec.json
- 模型名称: `glm-5.1`
- 序号: `05`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/mutation_spec/03_run_20260530_052915_v003_mutation_spec.json`

```json
{
  "mutation_type": "add_entry_filter",
  "reason": "冠军策略核心问题：4次止损亏损-7.58 USDT完全吞噬20次ROI盈利+6.32 USDT，尤其ETH/USDT的PF仅0.44拖累整体。上一轮pair_specific_filter和tighten_entry_trigger均因生成重复指纹而失败。本轮选择add_entry_filter，新增ADX趋势强度+EMA趋势方向+RSI区间三重过滤，精准筛除弱势趋势中的入场信号，从根源减少止损触发次数，而非调整止损/ROI参数。这与已失败的mutation_type方向不同，且能产生不同的策略指纹。",
  "expected_effect": "止损次数从4次降至1-2次，stop_loss_profit_abs从-7.58收窄至-2~-3 USDT；总交易数维持在25-40区间；profit_factor从0.85提升至1.0+；ETH/USDT的亏损交易被过滤掉大部分",
  "changes": {
    "add_entry_filters": [
      "ADX > 20：确保仅在趋势明确时入场，过滤震荡行情中的MACD假交叉",
      "close > ema（如EMA50）：确保多头入场时价格在均线上方，避免逆势入场",
      "RSI介于30-65之间：避免在超买区（RSI>65）追多，减少入场即回调触发止损的概率"
    ],
    "target_entry_tag": "macd_cross_momentum",
    "filter_logic": "所有新增过滤条件与现有MACD交叉条件取AND，不改变原有MACD逻辑，仅叠加过滤层"
  },
  "do_not_change": {
    "stoploss": "-0.035，不放宽也不收紧，避免与黑名单中stoploss=-0.035+trailing_stop=True组合重复",
    "minimal_roi": "保持当前{0: 0.02, 25: 0.015, 50: 0.01, 100: 0.005}结构",
    "trailing_stop": "保持True",
    "use_exit_signal": "保持False",
    "timeframe": "保持5m",
    "protection_cooldown": "保持当前CooldownPeriod+MaxDrawdown+StoplossGuard组合"
  }
}
```

### glm-5.1 - 序号 06 - strategy.py
- 模型名称: `glm-5.1`
- 序号: `06`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/glm-5.1/strategy_py/03_run_20260530_052915_v001_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_052915_v001(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.02,
        "25": 0.015,
        "50": 0.01,
        "100": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        atr_filter = (dataframe["atr_ratio"] < 1.3)

        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            long_condition = None
        elif pair == "ETH/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 25) &
                (dataframe["macd_histogram"] > 0)
            )
        elif pair == "XRP/USDT":
            long_condition = None
        elif pair == "DOGE/USDT":
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

## Model: claude-opus-4-7

### claude-opus-4-7 - 序号 01 - mutation_spec.json
- 模型名称: `claude-opus-4-7`
- 序号: `01`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/mutation_spec/01_run_20260530_033035_v020_mutation_spec.json`

```json
{
  "mutation_type": "pair_specific_filter",
  "reason": "本轮所有11种mutation_type都已尝试过且失败，但pair_specific_filter是唯一曾成功的类型（v015冠军即来自此方向）。上一轮v019的pair_specific_filter失败原因是交易数22略低于25下限——问题出在ETH过滤太严导致交易数不足。本轮采用不同策略：不再过滤ETH的入场数量，而是针对ETH添加更严格的趋势确认条件（要求EMA20>EMA50且close>EMA20），同时对SOL略微放宽RSI阈值从32→34以增加1-2笔交易，确保总交易数>=25。核心目标是减少ETH的止损亏损（当前ETH 9笔中2笔止损贡献了大部分亏损），而非减少ETH交易数。",
  "session_parent_reason": "v015冠军有25笔交易刚好达标，ETH贡献-1.84 USDT亏损（PF=0.51），4笔止损共亏-7.58。本轮通过对ETH增加趋势过滤条件来避免逆势入场被止损，同时对SOL微调RSI以补偿可能减少的ETH交易数。",
  "expected_effect": "ETH止损次数从2次降至0-1次，止损亏损减少约2-4 USDT，总交易数维持25-28笔，profit_factor提升至1.1-1.3，总收益转正。",
  "changes": {
    "entry_conditions": {
      "ETH/USDT": "增加趋势过滤：要求 dataframe['ema20'] > dataframe['ema50'] 且 dataframe['close'] > dataframe['ema20']，仅在上升趋势中入场，减少逆势止损",
      "SOL/USDT": "RSI入场阈值从32放宽至34，微增1-2笔交易以补偿ETH可能减少的交易"
    },
    "indicators_to_add": "ema20 = ta.EMA(dataframe, timeperiod=20)（如尚未存在）",
    "stoploss": "保持不变 -0.035",
    "minimal_roi": "保持不变",
    "trailing_stop": "保持不变"
  },
  "do_not_change": [
    "stoploss",
    "minimal_roi",
    "trailing_stop",
    "trailing_stop_positive",
    "trailing_stop_positive_offset",
    "use_exit_signal",
    "timeframe",
    "DOGE/USDT入场条件",
    "BTC/USDT入场条件",
    "XRP/USDT入场条件"
  ]
}
```

### claude-opus-4-7 - 序号 02 - strategy.py
- 模型名称: `claude-opus-4-7`
- 序号: `02`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/strategy_py/01_run_20260530_033035_v020_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_033035_v020(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.035,
        "25": 0.02,
        "50": 0.012,
        "100": 0.006,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        # Bollinger Bands for volatility contraction filter
        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        # MACD histogram rising (current > previous)
        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        # Bollinger %B
        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        # ATR-based volatility filter
        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        # ATR volatility filter: skip entries when volatility is spiking
        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum condition
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            # Block BTC/USDT - too many stoplosses
            long_condition = None
        elif pair == "ETH/USDT":
            # Stricter ETH: require EMA20 > EMA50 and close > EMA20 for strong short-term uptrend
            # This filters out counter-trend entries that get stopped out
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 22)
            )
        elif pair == "XRP/USDT":
            # Block XRP/USDT - too many stoplosses
            long_condition = None
        elif pair == "DOGE/USDT":
            # Keep DOGE unchanged
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: slightly relaxed RSI lower bound from 38->34 and volume from 0.9->0.8
            # to pick up a few more valid trend entries
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 34) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.8) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            # For others: use the base
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### claude-opus-4-7 - 序号 03 - mutation_spec.json
- 模型名称: `claude-opus-4-7`
- 序号: `03`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/mutation_spec/02_run_20260530_033035_v019_mutation_spec.json`

```json
{
  "mutation_type": "pair_specific_filter",
  "reason": "所有11种mutation_type都已尝试且失败，但pair_specific_filter是唯一曾成功的类型（v015冠军）。v015的核心问题是ETH/USDT贡献了-1.84 USDT亏损（9笔交易中2笔止损，PF仅0.51），而SOL表现良好。v017/v018尝试pair_specific_filter失败是因为放宽了ETH入场导致多了止损交易。本轮采用完全不同的方向：不是限制ETH入场条件，而是对ETH增加一个ATR波动率过滤器，仅在低波动环境下入场ETH，从根本上避免ETH在高波动时触发止损。同时微调ROI让ETH的盈利交易更快锁定利润。",
  "session_parent_reason": "基于v015冠军，ETH的4笔止损中有2笔造成-7.58 USDT总止损亏损的大部分。需要精准过滤ETH的高风险入场而非全面收紧。",
  "expected_effect": "ETH止损次数从2次降至0-1次，减少约3-4 USDT止损亏损，profit_factor从1.00提升至1.3+，总交易数维持25-30笔（可能ETH减少2-3笔但仍在范围内）。",
  "changes": {
    "pair_specific_filter": {
      "ETH/USDT": {
        "add_condition": "dataframe['atr_14'] < dataframe['atr_14'].rolling(50).mean() * 1.1",
        "description": "ETH仅在ATR低于50周期均值1.1倍时入场，过滤高波动环境下的假突破"
      }
    },
    "adjust_roi": {
      "description": "对ETH缩短ROI持仓时间，更快锁定利润避免回撤转止损",
      "minimal_roi": {
        "0": 0.032,
        "20": 0.018,
        "40": 0.01,
        "80": 0.005
      }
    },
    "indicators_to_add": [
      "atr_14_rolling_mean_50"
    ],
    "entry_logic_change": "在populate_entry_trend中，对pair=='ETH/USDT'的行增加ATR波动率过滤条件"
  },
  "do_not_change": [
    "stoploss保持-0.035不变",
    "trailing_stop保持True不变",
    "SOL/USDT和DOGE/USDT的入场逻辑不变",
    "MACD交叉动量核心入场信号不变",
    "use_exit_signal保持False",
    "timeframe保持5m"
  ]
}
```

### claude-opus-4-7 - 序号 04 - strategy.py
- 模型名称: `claude-opus-4-7`
- 序号: `04`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/strategy_py/02_run_20260530_033035_v019_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_033035_v019(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.032,
        "20": 0.018,
        "40": 0.01,
        "80": 0.005,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        # Bollinger Bands for volatility contraction filter
        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        # MACD histogram rising (current > previous)
        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        # Bollinger %B
        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        # ATR-based volatility filter
        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        # ATR 50-period mean for ETH volatility filter
        dataframe["atr_sma_50"] = dataframe["atr"].rolling(window=50).mean()

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        # ATR volatility filter: skip entries when volatility is spiking
        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum condition
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            # Block BTC/USDT - too many stoplosses
            long_condition = None
        elif pair == "ETH/USDT":
            # Stricter ETH: require low ATR environment to avoid false breakouts
            # Only enter when ATR is below 1.1x its 50-period mean (low volatility)
            eth_atr_filter = (dataframe["atr"] < dataframe["atr_sma_50"] * 1.1)
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 30) &
                eth_atr_filter
            )
        elif pair == "XRP/USDT":
            # Block XRP/USDT - too many stoplosses
            long_condition = None
        elif pair == "DOGE/USDT":
            # Keep DOGE unchanged
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: slightly relaxed RSI upper bound to capture a few more trend entries
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            # For others: use the base
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### claude-opus-4-7 - 序号 05 - mutation_spec.json
- 模型名称: `claude-opus-4-7`
- 序号: `05`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/mutation_spec/03_run_20260530_033035_v018_mutation_spec.json`

```json
{
  "mutation_type": "pair_specific_filter",
  "reason": "v015冠军中ETH/USDT贡献9笔交易但净亏损-1.84 USDT（PF=0.51），其中2笔止损亏损占总止损的大部分。上一轮pair_specific_filter成功过（产生了v015冠军），本轮采用更精准的ETH入场过滤：要求ETH入场时ADX>28且价格在EMA100之上，而非完全禁止ETH。这样可保留ETH的高质量信号（7笔盈利），同时过滤掉弱势环境下的2笔大额止损。与之前失败的pair_specific_filter不同，本次不是简单排除币种或放宽条件，而是针对ETH增加趋势强度门槛。",
  "session_parent_reason": "所有11种mutation_type都已尝试过，pair_specific_filter是唯一成功过的类型。本轮回到pair_specific_filter但采用完全不同的实现方式：不排除ETH，而是对ETH施加更严格的ADX+EMA100趋势确认，目标是将ETH的2笔止损亏损（约-3.8 USDT）过滤掉，同时保留其7笔盈利交易中的大部分。",
  "expected_effect": "ETH交易数从9降至约5-7笔（过滤弱势入场），止损次数从4降至2-3次，总止损亏损从-7.58降至约-5.5 USDT，profit_factor从1.00提升至1.05-1.15，总交易数维持在23-27范围内。",
  "changes": {
    "entry_conditions": {
      "ETH/USDT_additional_filter": "对ETH/USDT入场增加条件：adx > 28 且 close > ema100，确保只在强趋势且价格在长期均线上方时入场",
      "implementation": "在populate_entry_trend中，对metadata['pair']=='ETH/USDT'的情况额外要求 (dataframe['adx'] > 28) & (dataframe['close'] > dataframe['ema100'])"
    },
    "indicators": {
      "add": "确保populate_indicators中计算ema100（如已有则无需重复）"
    }
  },
  "do_not_change": [
    "stoploss值保持-0.035不变",
    "trailing_stop配置不变",
    "minimal_roi不变",
    "SOL/USDT和DOGE/USDT的入场条件不变",
    "基础macd_cross_momentum入场逻辑不变"
  ]
}
```

### claude-opus-4-7 - 序号 06 - strategy.py
- 模型名称: `claude-opus-4-7`
- 序号: `06`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/claude-opus-4-7/strategy_py/03_run_20260530_033035_v018_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260530_033035_v018(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.035,
        "25": 0.02,
        "50": 0.012,
        "100": 0.006,
    }

    stoploss = -0.035

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.014
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_20"] = ta.EMA(dataframe, timeperiod=20)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_100"] = ta.EMA(dataframe, timeperiod=100)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        # Bollinger Bands for volatility contraction filter
        bollinger = qtpylib.bollinger_bands(qtpylib.typical_price(dataframe), window=20, stds=2)
        dataframe["bb_upper"] = bollinger["upper"]
        dataframe["bb_lower"] = bollinger["lower"]
        dataframe["bb_width"] = (dataframe["bb_upper"] - dataframe["bb_lower"]) / dataframe["bb_lower"]
        dataframe["bb_width_mean_20"] = dataframe["bb_width"].rolling(window=20, min_periods=20).mean()

        # MACD histogram rising (current > previous)
        dataframe["macd_hist_rising"] = (
            (dataframe["macd_histogram"] > dataframe["macd_histogram"].shift(1)) &
            (dataframe["macd_histogram"].shift(1) > dataframe["macd_histogram"].shift(2))
        )

        # Bollinger %B
        dataframe["bb_pctb"] = (dataframe["close"] - dataframe["bb_lower"]) / (dataframe["bb_upper"] - dataframe["bb_lower"])

        # ATR-based volatility filter
        dataframe["atr"] = ta.ATR(dataframe, timeperiod=14)
        dataframe["atr_sma"] = dataframe["atr"].rolling(window=20).mean()
        dataframe["atr_ratio"] = dataframe["atr"] / dataframe["atr_sma"]

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        pair = metadata.get("pair", "")

        # ATR volatility filter: skip entries when volatility is spiking
        atr_filter = (dataframe["atr_ratio"] < 1.3)

        # Base MACD momentum condition
        macd_momentum = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 40) &
            (dataframe["rsi_14"] < 72) &
            (dataframe["adx"] > 18) &
            (dataframe["close"] > dataframe["ema_50"]) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 1.0) &
            (dataframe["volume"] > 0) &
            atr_filter
        )

        if pair == "BTC/USDT":
            # Block BTC/USDT - too many stoplosses
            long_condition = None
        elif pair == "ETH/USDT":
            # Stricter ETH: require ADX > 28 and close > EMA100 for strong trend confirmation
            # This filters out weak-trend entries that led to stoploss hits
            long_condition = (
                macd_momentum &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["close"] > dataframe["ema_100"]) &
                (dataframe["close"] > dataframe["ema_20"]) &
                (dataframe["ema_20"] > dataframe["ema_50"]) &
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] > 45) &
                (dataframe["adx"] > 28)
            )
        elif pair == "XRP/USDT":
            # Block XRP/USDT - too many stoplosses
            long_condition = None
        elif pair == "DOGE/USDT":
            # Keep DOGE unchanged
            long_condition = (
                macd_momentum &
                (dataframe["adx"] > 26) &
                (dataframe["rsi_14"] < 58) &
                (dataframe["close"] > dataframe["ema_200"]) &
                (dataframe["macd_hist_rising"])
            )
        elif pair == "SOL/USDT":
            # SOL: slightly relaxed RSI upper bound to capture a few more trend entries
            long_condition = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["ema_12"] > dataframe["ema_26"]) &
                (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
                (dataframe["rsi_14"] > 38) &
                (dataframe["rsi_14"] < 74) &
                (dataframe["adx"] > 17) &
                (dataframe["close"] > dataframe["ema_50"]) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 0.9) &
                (dataframe["volume"] > 0) &
                atr_filter
            )
        else:
            # For others: use the base
            long_condition = macd_momentum

        if long_condition is not None:
            dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
                1,
                "macd_cross_momentum",
            )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

## Model: gpt-5.5

### gpt-5.5 - 序号 01 - strategy.py
- 模型名称: `gpt-5.5`
- 序号: `01`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/gpt-5.5/strategy_py/01_run_20260529_061342_v005_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260529_061342_v005(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.012,
        "30": 0.008,
        "60": 0.005,
        "120": 0.002,
    }

    stoploss = -0.018

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.012
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        long_condition = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 45) &
            (dataframe["rsi_14"] < 70) &
            (dataframe["adx"] > 20) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 2.2) &
            (dataframe["volume"] > 0)
        )

        dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
            1,
            "macd_cross_momentum",
        )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### gpt-5.5 - 序号 02 - strategy.py
- 模型名称: `gpt-5.5`
- 序号: `02`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/gpt-5.5/strategy_py/02_run_20260529_061342_v004_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260529_061342_v004(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.015,
        "30": 0.008,
        "90": 0.004,
        "180": 0.002,
    }

    stoploss = -0.025

    trailing_stop = True
    trailing_stop_positive = 0.008
    trailing_stop_positive_offset = 0.012
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        long_condition = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 45) &
            (dataframe["rsi_14"] < 70) &
            (dataframe["adx"] > 20) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 2.2) &
            (dataframe["volume"] > 0)
        )

        pair = metadata.get("pair", "")
        if pair.startswith("BTC/"):
            btc_entry_filter = (
                (dataframe["ema_50"] > dataframe["ema_200"]) &
                (dataframe["rsi_14"] < 45) &
                (dataframe["volume"] > dataframe["volume_mean_20"] * 1.5)
            )
            long_condition = long_condition & btc_entry_filter

        dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
            1,
            "macd_cross_momentum",
        )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```

### gpt-5.5 - 序号 03 - strategy.py
- 模型名称: `gpt-5.5`
- 序号: `03`
- 源文件: `model_artifacts/model_export_20260530_054626_19adfe7d/gpt-5.5/strategy_py/03_run_20260529_061342_v003_strategy.py`

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta
from freqtrade.vendor.qtpylib import indicators as qtpylib


class MultiCoin_AI_Strategy_20260529_061342_v003(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.012,
        "15": 0.008,
        "35": 0.005,
        "60": 0.003,
        "120": 0.001,
    }

    stoploss = -0.025

    trailing_stop = True
    trailing_stop_positive = 0.005
    trailing_stop_positive_offset = 0.01
    trailing_only_offset_is_reached = True

    use_exit_signal = False
    exit_profit_only = False
    ignore_roi_if_entry_signal = False

    process_only_new_candles = True
    startup_candle_count = 220

    @property
    def protections(self):
        return [
            {
                "method": "CooldownPeriod",
                "stop_duration_candles": 3,
            },
            {
                "method": "MaxDrawdown",
                "lookback_period_candles": 48,
                "trade_limit": 10,
                "stop_duration_candles": 6,
                "max_allowed_drawdown": 0.02,
            },
            {
                "method": "StoplossGuard",
                "lookback_period_candles": 24,
                "trade_limit": 2,
                "stop_duration_candles": 8,
                "only_per_pair": True,
            },
        ]

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_12"] = ta.EMA(dataframe, timeperiod=12)
        dataframe["ema_26"] = ta.EMA(dataframe, timeperiod=26)
        dataframe["ema_50"] = ta.EMA(dataframe, timeperiod=50)
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi_14"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["adx"] = ta.ADX(dataframe, timeperiod=14)

        macd = ta.MACD(dataframe, fastperiod=12, slowperiod=26, signalperiod=9)
        dataframe["macd"] = macd["macd"]
        dataframe["macd_signal"] = macd["macdsignal"]
        dataframe["macd_histogram"] = macd["macdhist"]

        dataframe["volume_mean_20"] = dataframe["volume"].rolling(window=20, min_periods=20).mean()

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["enter_long"] = 0
        dataframe["enter_tag"] = ""

        long_condition = (
            (dataframe["ema_50"] > dataframe["ema_200"]) &
            (dataframe["ema_12"] > dataframe["ema_26"]) &
            (qtpylib.crossed_above(dataframe["macd_histogram"], 0)) &
            (dataframe["rsi_14"] > 45) &
            (dataframe["rsi_14"] < 70) &
            (dataframe["adx"] > 20) &
            (dataframe["volume"] > dataframe["volume_mean_20"] * 2.2) &
            (dataframe["volume"] > 0)
        )

        dataframe.loc[long_condition, ["enter_long", "enter_tag"]] = (
            1,
            "macd_cross_momentum",
        )

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["exit_long"] = 0
        return dataframe
```
