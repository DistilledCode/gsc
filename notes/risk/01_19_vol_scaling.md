# Volatility Scaling
Volatility scaling is a technique in quantitative finance used to adjust the position size of an asset or portfolio based on its observed or estimated volatility. The goal is to normalize risk across assets or portfolios, ensuring consistent risk exposure over time or across investments:


### **Concept of Volatility Scaling**
1. **Volatility as a Measure of Risk**:
   - Volatility refers to the degree of variation in an asset's returns over a specific period. Higher volatility implies higher risk, while lower volatility implies lower risk.
   - It is often measured using historical price data (e.g., standard deviation of returns) or implied volatility from options markets.

2. **Purpose of Scaling**:
   - Volatility scaling adjusts position sizes to maintain a consistent risk level.
   - Without scaling, highly volatile assets could disproportionately influence portfolio risk, while low-volatility assets might contribute too little.

3. **Key Formula**:
   The position size of an asset is inversely proportional to its volatility:
   $$
   \text{Scaled Position} = \frac{\text{Target Risk}}{\text{Volatility}}
   $$
   - **Target Risk** is the desired risk level for the position or portfolio.
   - **Volatility** is the observed or forecasted volatility of the asset.

4. **Application**:
   - For a single asset, volatility scaling adjusts the position size based on its individual volatility.
   - For a portfolio, it helps allocate capital across assets to achieve balanced risk contributions.

### **Examples of Volatility Scaling**
1. **Risk Parity**:
   - The risk parity principle states that all assets in a portfolio should contribute equally to the total portfolio risk. This means the marginal risk contribution from each asset should be equal. The key insight is that risk parity doesn't focus on equal weights (like $\frac{1}{N}$ portfolios) or equal volatility contributions, but rather on equal risk contributions when considering correlations.
   - In risk parity, higher-volatility assets receive lower allocations, while lower-volatility assets receive higher allocations.


2. **Trend-Following Strategies**:
   - Many systematic trading strategies use volatility scaling to adjust bet sizes dynamically.
   - For example, a trend-following fund might increase exposure to assets with lower volatility to maintain consistent risk across trades.

3. **Leverage Adjustments**:
   - In leveraged portfolios, volatility scaling is used to ensure that leverage does not lead to excessive risk during periods of high market volatility.

# Volatility Scaling for Uncorrelated Assets

- You have three uncorrelated assets: **Asset A**, **Asset B**, and **Asset C**.
- You want the **total portfolio volatility to equal 10%** and to allocate capital based on each asset's volatility (i.e., normalize risk exposure across assets).
- Volatility estimates for the assets (annualized):
  - **Asset A**: 20%
  - **Asset B**: 10%
  - **Asset C**: 5%
- Initial capital: **$1,000,000**.

## **Calculation**

#### 1. **Determine Risk Contribution per Asset**
Let’s assume the portfolio is risk-targeted at 10%. The weight of each asset will be scaled inversely to its volatility. 

$$
\text{Weight of Asset}_i = \frac{\frac{1}{\text{Volatility of Asset}_i}}{\sum_{j=1}^n \frac{1}{\text{Volatility of Asset}_j}}
$$

Using the formula:

$$
\text{Weight of Asset A} = \frac{\frac{1}{0.20}}{\frac{1}{0.20} + \frac{1}{0.10} + \frac{1}{0.05}} = \frac{5}{5 + 10 + 20} = 0.1667
$$

$$
\text{Weight of Asset B} = \frac{\frac{1}{0.10}}{\frac{1}{0.20} + \frac{1}{0.10} + \frac{1}{0.05}} = \frac{10}{5 + 10 + 20} = 0.3333
$$

$$
\text{Weight of Asset C} = \frac{\frac{1}{0.05}}{\frac{1}{0.20} + \frac{1}{0.10} + \frac{1}{0.05}} = \frac{20}{5 + 10 + 20} = 0.5000
$$

#### 2. **Allocate Portfolio Capital**:
Using the weights, allocate the portfolio capital of $1,000,000:

$$
\text{Capital Allocated to Asset A} = 0.1667 \times 1,000,000 = 166,667
$$

$$
\text{Capital Allocated to Asset B} = 0.3333 \times 1,000,000 = 333,333
$$

$$
\text{Capital Allocated to Asset C} = 0.5000 \times 1,000,000 = 500,000
$$

#### 3. **Verify Risk Contribution**:
Each asset’s risk contribution is proportional to its weight and volatility:

$$
\text{Risk Contribution}_i = \text{Weight}_i \times \text{Volatility}_i
$$

- For **Asset A**:
  $
  \text{Risk Contribution}_A = 0.1667 \times 20\% = 3.33\%
  $
- For **Asset B**:
  $
  \text{Risk Contribution}_B = 0.3333 \times 10\% = 3.33\%
  $
- For **Asset C**:
  $
  \text{Risk Contribution}_C = 0.5000 \times 5\% = 3.33\%
  $


The total portfolio volatility is the sum of individual contributions:

$$
\text{Total Portfolio Volatility} = 3.33\% + 3.33\% + 3.33\% = 10\%
$$

**3. Results**

**Scaled Weights**:
  - Asset A: $16.67\%$
  - Asset B: $33.33\%$
  - Asset C: $50.00\%$
  
**Capital Allocations**:
  - Asset A: $\$166,667$
  - Asset B: $\$333,333$
  - Asset C: $\$500,000$

**Portfolio Risk**: 10%, with equal contribution from all three assets.

## **Key Insights**
- **Higher-volatility assets (Asset A)** receive smaller allocations, while **lower-volatility assets (Asset C)** receive larger allocations.
- This ensures equalized risk contribution and maintains consistent portfolio risk.


## **Assumptions**

In the example of volatility scaling provided, we make several **assumptions** about the volatility of the assets. Some of these relate to how we treat **volatility** itself and the **correlation between assets**, which are critical factors in determining the actual portfolio risk. Let's examine these assumptions in detail:


### **1. Assumptions About Volatility**
- **Constant Volatility**:
We assume that the estimated volatilities for each asset (e.g., 20%, 10%, and 5%) remain stable over the holding period. In reality, asset volatility often fluctuates due to market conditions.

- **Reliable Volatility Estimates**:
We assume that the historical or model-based volatility we use as inputs accurately reflects future behavior. However, past volatility may not always predict future risk accurately.

- **Independence from Position Size**:
Volatility is assumed to be independent of how much capital we allocate to an asset. This holds in most cases but could break down if our positions are large enough to move the market or impact liquidity.

### **2. Assumptions About Correlation**
- **No Correlation Between Assets**:
  - In the example, we calculate risk contributions and portfolio volatility assuming that the assets are uncorrelated. This simplifies the math but is unrealistic in real-world portfolios, where assets often exhibit correlations.
  - If assets are correlated, their combined risk is lower or higher than the sum of their individual risks, depending on whether the correlations are negative or positive.

- **Independent Risk Contributions**:
The calculation assumes each asset contributes independently to portfolio risk. When assets are correlated, their risk contributions overlap, so equalizing individual risks may not equalize overall risk.

# Volatility Scaling for Correlated Assets

## **Impact of Correlation**
If we account for correlation, the **portfolio volatility** would no longer be the simple sum of weighted volatilities. Instead, it would be calculated using the covariance matrix:

$$
\text{Portfolio Volatility} = \sqrt{\mathbf{w}^\top \Sigma \mathbf{w}}
$$

Where:
- $\mathbf{w}$: Weight vector of the assets.
- $\Sigma$: Covariance matrix of asset returns.

- **Covariance matrix**:
  The covariance matrix incorporates both volatilities and correlations between assets:
  $$
  \Sigma_{ij} = \rho_{ij} \cdot \sigma_i \cdot \sigma_j
  $$
  Where $\rho_{ij}$ is the correlation between asset $i$ and $j$, and $\sigma_i$ and $\sigma_j$ are their volatilities.

- **Example with Correlation**:
  If Asset A and Asset B are positively correlated (e.g., $\rho_{AB} = 0.8$), allocating to both would result in higher portfolio risk than if they were uncorrelated. Conversely, if they are negatively correlated (e.g., $\rho_{AB} = -0.5$), the portfolio risk would be lower due to diversification effects.

## **Adjustments for Correlation**
To account for correlations:
1. **Calculate Portfolio Risk Dynamically**:
   Use the covariance matrix to calculate total portfolio risk. Adjust asset weights iteratively to equalize their risk contributions within the correlated framework.

2. **Diversification Impact**:
   Correlated assets contribute less diversification benefit, so their scaled weights may need adjustment to maintain the desired risk target.

3. **Equal Risk Contribution (ERC)**:
   In correlated portfolios, the principle of equal risk contribution can be applied. This ensures that each asset's marginal contribution to total portfolio risk is equal, factoring in both volatility and correlations.

## Example

Let’s extend the volatility scaling example to account for **correlated assets**. We will calculate portfolio volatility and adjust allocations accordingly by incorporating the **covariance matrix**.

**Scenario**:
- We have three assets: **Asset A**, **Asset B**, and **Asset C**.
- **Target portfolio risk (volatility):** 10%.
- **Volatility estimates (annualized):**
  - **Asset A**: 20%
  - **Asset B**: 10%
  - **Asset C**: 5%
- **Correlation matrix**:
  $$
  \mathbf{\rho} =
  \begin{bmatrix}
  1 & 0.8 & 0.2 \\
  0.8 & 1 & 0.5 \\
  0.2 & 0.5 & 1 \\
  \end{bmatrix}
  $$
- **Initial capital**: $1,000,000.

### **Step-by-Step Calculation**:

#### 1. **Construct the Covariance Matrix**
The covariance matrix $\Sigma$ is derived from the volatilities and correlations:
$$
\Sigma_{ij} = \rho_{ij} \cdot \sigma_i \cdot \sigma_j
$$

$$
\Sigma =
\begin{bmatrix}
0.20^2 & 0.8 \cdot 0.20 \cdot 0.10 & 0.2 \cdot 0.20 \cdot 0.05 \\
0.8 \cdot 0.20 \cdot 0.10 & 0.10^2 & 0.5 \cdot 0.10 \cdot 0.05 \\
0.2 \cdot 0.20 \cdot 0.05 & 0.5 \cdot 0.10 \cdot 0.05 & 0.05^2 \\
\end{bmatrix}
=
\begin{bmatrix}
0.04 & 0.016 & 0.002 \\
0.016 & 0.01 & 0.0025 \\
0.002 & 0.0025 & 0.0025 \\
\end{bmatrix}
$$

#### 2. **Initial Equal Weights**
Assume equal weights for all three assets to calculate the initial portfolio volatility:

$$
\mathbf{w} = \begin{bmatrix} \frac{1}{3}, \frac{1}{3}, \frac{1}{3} \end{bmatrix}
$$

Portfolio variance is:
$$
\text{Portfolio Variance} = \mathbf{w}^\top \Sigma \mathbf{w}
$$

Substitute:
$$
\text{Portfolio Variance} =
\begin{bmatrix}
\frac{1}{3} & \frac{1}{3} & \frac{1}{3}
\end{bmatrix}
\cdot
\begin{bmatrix}
0.04 & 0.016 & 0.002 \\
0.016 & 0.01 & 0.0025 \\
0.002 & 0.0025 & 0.0025 \\
\end{bmatrix}
\cdot
\begin{bmatrix}
\frac{1}{3} \\
\frac{1}{3} \\
\frac{1}{3} \\
\end{bmatrix}
$$

Perform the calculations:
$$
\text{Portfolio Variance} = 0.010389 \quad \text{(Approx)}
$$

Portfolio volatility is:
$$
\text{Portfolio Volatility} = \sqrt{0.010389} \approx 10.19\%
$$


#### 3. **Rescale Weights for Target Volatility**
The portfolio is riskier than the target ($10.19\% > 10\%$), so we scale the weights. We need to **adjust the weights dynamically** such that:

1. The total capital is fully allocated (sum of weights = 100%).
2. The target portfolio volatility is achieved.

#### 4. **Optimization Approach**
We need to solve for the weights $\mathbf{w}$ that satisfy:
1. **Full allocation**:
   $$
   \sum_{i=1}^n w_i = 1
   $$

2. **Target portfolio volatility**:
   $$
   \sqrt{\mathbf{w}^\top \Sigma \mathbf{w}} = \text{Target Volatility}
   $$

3. **Non-negativity constraint** (if short-selling is not allowed):
   $$
   w_i \geq 0 \quad \forall i
   $$


#### 5. **Optimization Objective**

We use a numerical optimization method (e.g., quadratic programming) to solve the following problem:

Using risk-parity principle, we minimize deviations from the target volatility, subject to full allocation and non-negativity constraints

$$
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n \sum_{j=1}^n (\mathbf{w}_i(\Sigma\mathbf{w})_i - \mathbf{w}_j(\Sigma\mathbf{w})_j)^2 \\
\text{subject to} & \mathbf{1}^\top\mathbf{w} = 1 \\
& \mathbf{w} \geq \mathbf{0} \\
& \sqrt{\mathbf{w}^\top \Sigma \mathbf{w}} = \text{Target Volatility}
\end{array}
$$

## Why can't we simply minimize $\left( \sqrt{\mathbf{w}^\top \Sigma \mathbf{w}} - \text{Target Volatility} \right)^2$

#### Convexity Analysis
- The term $\mathbf{w}^\top \Sigma \mathbf{w}$ is convex (as Σ is positive semi-definite)
- However, $\sqrt{\mathbf{w}^\top \Sigma \mathbf{w}}$ is concave
- The objective function $(\sqrt{\mathbf{w}^\top \Sigma \mathbf{w}} - \text{Target})^2$ is neither convex nor concave

This is a significant issue! The non-convexity means:
- Multiple local minima may exist
- Global optimality is not guaranteed
- Standard convex optimization methods may fail


#### Risk Contribution Analysis

For any asset $i$, its risk contribution (RC) to the portfolio is:
$$RC_i = w_i \cdot (\Sigma\mathbf{w})_i$$

Where:
- $w_i$ is the weight of asset $i$
- $(\Sigma\mathbf{w})_i$ is the i-th element of the vector resulting from $\Sigma\mathbf{w}$

This represents the marginal contribution of asset $i$ to portfolio risk

**1. Objective Function Breakdown**

$$\sum_{i=1}^n \sum_{j=1}^n (\mathbf{w}_i(\Sigma\mathbf{w})_i - \mathbf{w}_j(\Sigma\mathbf{w})_j)^2$$

This objective function:
- Takes every possible pair of assets $(i,j)$
- Computes the difference in their risk contributions
- Squares these differences to ensure positivity and penalize larger deviations
- Sums all these squared differences

**2. Why This Helps**

1. Mathematical Benefits:
   - The objective function is now positive definite
   - It's differentiable everywhere (unlike the original formulation)
   - While still non-convex, it has better numerical properties

2. Portfolio Management Benefits:
   - Provides better diversification by focusing on risk contributions
   - More stable to estimation errors in volatilities and correlations
   - Typically results in more balanced portfolios


**3. Example**

Let's consider a simple case with two assets:

The term $(\mathbf{w}_1(\Sigma\mathbf{w})_1 - \mathbf{w}_2(\Sigma\mathbf{w})_2)^2$ would be minimized when
  $\mathbf{w}_1(\Sigma\mathbf{w})_1 = \mathbf{w}_2(\Sigma\mathbf{w})_2$
  meaning both assets contribute equally to portfolio risk


**4. Benefits over Original Formulation**

- Less sensitive to estimation errors in input parameters
- More robust to extreme market conditions
- Better numerical properties for optimization
- Clear economic meaning (equal risk contribution)

**5. Mathematical Properties**

The risk-parity objective function has several advantageous properties:
1. Smoothness: Continuously differentiable everywhere in feasible region
2. Bounded: Non-negative and well-behaved at extremes
3. Symmetric: Treats all assets equally
4. Scale-invariant: Results don't depend on arbitrary scaling of assets


**6. Constraints Interaction**

The combination of the risk-parity objective with the constraints creates a well-structured problem:
- Full investment constraint ($\mathbf{1}^\top\mathbf{w} = 1$) ensures proper portfolio scaling
- Non-negativity ($\mathbf{w} \geq \mathbf{0}$) prevents short-selling
- Target volatility ($\sqrt{\mathbf{w}^\top \Sigma \mathbf{w}} = \text{Target Volatility}$) maintains desired risk level

This formulation effectively balances:
- Risk diversification (through risk parity)
- Portfolio constraints (through explicit constraints)
- Target risk level (through volatility constraint)

The risk-parity principle essentially transforms the portfolio optimization from a weight-based problem to a risk-based problem, which often leads to more robust and practically implementable solutions.