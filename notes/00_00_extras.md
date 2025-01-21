# Notional Exposure

Notional exposure refers to the total market value of a position that you're exposed to, regardless of how much capital you've actually deployed. It's the full face value or total value of the underlying assets controlled by your position.

For example:
- If you buy $100,000 worth of stocks directly, your notional exposure is $100,000
- If you buy futures contracts controlling $100,000 worth of stock index but only put up $10,000 in margin, your notional exposure is still $100,000
- If you enter into a swap agreement referencing $1 million in bonds but only post $50,000 in collateral, your notional exposure is $1 million

In the context of volatility targeting, a strategy targeting constant volatility will adjust its notional exposure up or down based on market conditions. When market volatility is high, it reduces notional exposure to maintain the same risk level. When volatility is low, it increases notional exposure. This is different from just maintaining the same dollar amount of market exposure regardless of conditions.

# Unfunded Returns

An unfunded return represents the excess return earned from a position that is financed by borrowing at the risk-free rate. It isolates the risk premium by removing the time value of money component.

**Key Characteristics**

1. Financing Structure:
- Position is established by borrowing at the risk-free rate (typically T-bill rate)
- Only requires margin/collateral rather than full capital investment
- Return is calculated net of financing costs

1. Mathematical Expression:
$$\text{Unfunded Return} = \text{Asset Return} - \text{Risk-free Rate}$$

**Common Examples**

1. Long Equity Position:
Example:
- Borrow $100 at T-bill rate (5%)
- Invest in stocks returning 12%
- Unfunded return = 12% - 5% = 7%

2. Futures/Forwards:
- Naturally unfunded instruments
- Trade on margin (typically 5-15% of notional value)
- No need to deduct risk-free rate as returns are already excess returns


# Exponentially Weighted Volatility

**Half-Life - The Foundation:**

Half-life is the time required for a quantity to reduce to half of its initial value. In a general exponential decay process:
$$N(t) = N_0 \cdot e^{-λt}$$
where:
- $N(t)$ is the quantity at time $t$
- $N_0$ is the initial quantity
- $\lambda$ is the decay constant
- $t$ is time

The relationship between half-life ($t_{1/2}$) and decay constant ($\lambda$) is:
$$t_{1/2} = \frac{\ln(2)}{λ}$$

1. Exponentially Weighted Volatility:
Volatility ($\sigma$) with exponential weights is calculated as:

$$σ_t = \sqrt{\sum_{i=1}^{n} w_i r_{t-i}^2}$$

where:
- $r_t$ are the returns
- $w_i$ are the weights given by:
$$w_i = (1-α)α^{i-1}$$

$$\alpha = \exp \left({\frac{\ln 0.5}{t_{1/2}}}\right)$$

The weights sum to 1 and decay exponentially as we go further back in time. For example, with a 90-day half-life:
- Day 1 weight is highest
- Day 90 weight is half of Day 1
- Day 180 weight is quarter of Day 1

Mathematically, after n half-lives, the remaining weight is:

$$w_{remaining} = \left( \frac{1}{2}\right)^n$$

The cumulative weight distribution follows:

$$w_{cumul}(t) = 1 - e^{-λt}$$


After three half-lives:
- $\left(\frac{1}{2}\right)^3 = 0.125$
- About $87.5\%$ of the total weight is captured
- The remaining weights become very small ($< 12.5\%$)
- This provides sufficient historical data for stable estimation


This ensures that the volatility estimate has "warmed up" sufficiently and isn't overly sensitive to initial conditions or lacking sufficient historical data for reliable estimation.

This approach provides a robust way to estimate volatility that:
1. Gives more weight to recent observations
2. Avoids mean estimation errors
3. Ensures sufficient historical data for reliable estimates
4. Smoothly incorporates new information while gradually forgetting old data


# Zero-Mean Volatility

Traditional volatility calculation involves:
$$σ = \sqrt{\frac{\sum_{i=1}^{n} (r_i - \bar{r})^2}{n-1}}$$

However, when using "stated zero mean", we assume $\mu = 0$, simplifying to:
$$σ = \sqrt{\frac{\sum_{i=1}^{n} r_i^2}{n}}$$

Why do this? 
- Mean returns are notoriously difficult to estimate accurately, especially over short periods
- The estimation error in mean can significantly impact volatility estimates
- For short-term volatility estimation, assuming zero mean often provides more stable estimates

# Mean Notional Exposure
Let's define our variables:
* $P_t$ = Daily closing price at time $t$
* $Q_t$ = Position size (quantity) at time $t$
* $n$ = Number of days in the observation period
* $\text{NE}_t$ = Notional Exposure at time $t$

The notional exposure for any given day is:
$$\text{NE}_t = P_t \times Q_t$$

The mean notional exposure over the period is:
$$\text{Mean Notional Exposure} = \frac{1}{n}\sum_{t=1}^{n} \text{NE}_t$$

Substituting the notional exposure formula:
$$\text{Mean Notional Exposure} = \frac{1}{n}\sum_{t=1}^{n} (P_t \times Q_t)$$

If the position size remains constant over the period (let's call it Q), the formula simplifies to:
$$\text{Mean Notional Exposure} = Q \times \frac{1}{n}\sum_{t=1}^{n} P_t$$

# Turnover Notional Exposure

"Turnover" in this context refers to how much your position's exposure (in notional terms) changes relative to its average size. It measures the rate at which you're changing your positions.

1) First, let's define our variables:
* $P_t$ = Daily closing price at time $t$
* $Q_t$ = Position size at time $t$
* $\text{NE}_t$ = Notional Exposure at time $t$ = $P_t \times Q_t$
* $n$ = Number of days in the observation period

2) The daily exposure change (absolute) is:
$$|\Delta \text{NE}_t| = |\text{NE}_t - \text{NE}_{t-1}|$$

3) The mean absolute daily exposure change is:
$$\text{Mean}|\Delta \text{NE}| = \frac{1}{n-1}\sum_{t=2}^{n} |\text{NE}_t - \text{NE}_{t-1}|$$

4) The mean exposure is:
$$\text{Mean NE} = \frac{1}{n}\sum_{t=1}^{n} \text{NE}_t$$

5) To annualize the mean absolute daily change, multiply by $252$ (assuming 252 trading days):
$$\text{Annualized Mean}|\Delta \text{NE}| = 252 \times \text{Mean}|\Delta \text{NE}|$$

6) Finally, the turnover notional exposure is:
$$\text{Turnover Notional Exposure} = \frac{252 \times \frac{1}{n-1}\sum_{t=2}^{n} |\text{NE}_t - \text{NE}_{t-1}|}{2 \times \frac{1}{n}\sum_{t=1}^{n} \text{NE}_t}$$

This can be simplified to:
$$\text{Turnover Notional Exposure} = \frac{252 \times n \times \sum_{t=2}^{n} |\text{NE}_t - \text{NE}_{t-1}|}{2(n-1) \times \sum_{t=1}^{n} \text{NE}_t}$$

**Why $\times 2$ in denominator**


1. The absolute daily exposure change captures both increases and decreases in position size. When you increase a position from 100 to 150, the absolute change is 50. When you decrease it from 150 back to 100, that's another absolute change of 50.

2. However, in terms of actual position turnover, this round-trip trade really only "turned over" a position size of 50, not 100 (which is what you'd get if you just summed the absolute changes).

3. By dividing by 2, we correct for this double-counting effect. It normalizes the metric to represent the actual amount of position change relative to the average position size.

Think of it like this: If you start with $100, buy another $50 (going to $150), then sell $50 (back to $100):
- Sum of absolute changes = $50 + $50 = $100
- Actual turnover = $50 (the amount that was actually "turned over")
- Using 2x mean exposure in denominator gives us a more accurate representation of how much of the position was actually traded relative to its size

Without the factor of 2, we would be overstating the turnover rate by counting each round-trip trade twice.


**Interpretation**

* How much of your position you're "turning over" or changing annually
* A turnover of 1 (or 100%) would mean you're effectively changing your entire position once per year
* Higher values indicate more active trading/position changes
Lower values indicate more static positions