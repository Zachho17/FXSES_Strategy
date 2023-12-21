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

## Workflow
### The data
Intraday data of mid price her hour is extracted using the API from Refinitiv Reuters, which only has maximum of one year worth of intraday hourly data.
The data is indexed with timestamp, and generated as a dataframe. The data is a time series data.

### Set up trading rules
- Segregate Tokyo session, London session and New York session
- Determine the respective highs and lows of three sessions
- If a trading signal is confirmed (either 3 higher highs or 3 lower lows) and no open trade, open a long/short trade at 8am Asian time
- Stop Loss (SL) and Take Profit (TP) would be determined accordingly (below)

### Set up trading simulation and evaluation
- SL set using London's high/low, determined by the direction of the trade (if long, use London low as SL. If short, use London high as SL.)
- TP set with the risk reward ratio of 1:1, i.e. if SL is 50 pips below the long entry, the TP is 50 pips above the long entry
- Open trade will be evaluated against both SL and TP level every hour, to determine whether to close the trade
- Once the trade is closed, the respective Profit and Loss will be saved in a list
- Performance matrix would be evaluated on the list, and a final performance summary would be generated into a dataframe

***If no position and no trade signal, the program would go to the next iteration**

## Challenges
### Data indexed in timestamp (great deal of time spent on this)
As there is a need to segregate each trading session by hour, I found it necessary to locate using specific timestamps rather than the relative index location from current iteration. Relative index location might still be applicable, though it might lack the specificity in achieving a consistent result.

Since we are only concerned the three sessions of the previous day, the strategy will not be needed to run in every iteration. Therefore additional logic has been put inplace, that convert and check the timestamp if current iteration and the previous iteration timestamp is within the same day

### Unconvenient timestamp data across weekend
Due to the nature of the data, certain workaround have been put in place to cater for weekend/Friday

### Open-trade evaluation
A multiple nested ifs have been put inplace to evaluate any open trades. 

As there are three possible states (no position, long positon and short position), an evaluation logic was needed for each. 
This undoubtedly lengthened the entire set of code.

Some degree of optimisation might be possible, but I have yet to explore at this junction. 
