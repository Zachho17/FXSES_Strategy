# FX Sessions (FXSES) price action pattern strategy

## Summary
The underlying idea of this trading strategy is built upon an observation of patterns on price data. 
The pattern is derived from comparing the respective trading range from Tokyo session, to London session then to New York session

If there are three consective higher highs (i.e. New York high was higher than London high, which was also higher than Tokyo high)
(vice versa for lower lows)

Illustration attached: (Coming soon)

_There seems to be some degree of price continuation/follow through if such patterns have been observed._
So I have decided to develop a backtesting logic using Reuters intraday data, which is available through the CodeBook function in Reuters.

**The goal is to explore if trading such price continuation could generate consistent profit.**
(buy when three consective higher highs observed, sell when three consective lower lows observed)
Backtesting is then applied to multiple different currencies, in a attempt to observe if the strategy works better for one than the others.

It has been concluded that such strategy goes not generate consistent profit in the currencies being tested within the backtest period.
However, one could argue that the backtesting period is just merely 12 months(constraints from data source), which might still be within one specific market regime.
**Namely, in certain market regime, currencies tend to move in range and lack of follow through in price action.**

More intraday data, ideally across multiple market regimes, could determine if such strategy works better in certain market regimes than the others.
Or we can declare the strategy to be completely useless.
However, this would lead to the need to identify market regime in the specific period. This can well be my next project. 

### test
