# Table of Content

- Coherent Risk Measure
- Value at Risk
  - VaR of Portfolio
  - Z-Score for Confidence Level in VaR
- Conditional Value at Risk
- R₂ ~ N(μ₂, σ₂²) for asset 2
  - CVaR of Portfolio
- Why VaR is incoherent & CVar is coherent

# [Coherent Risk Measure](https://people.math.ethz.ch/~delbaen/ftp/preprints/CoherentMF.pdf)

A **coherent risk measure** is a mathematical framework used in quantitative finance and risk management to quantify and assess risk in a consistent, rational, and mathematically robust way. It is based on a set of properties that a "good" risk measure should satisfy to ensure that it behaves predictably and aligns with intuitive risk assessment principles.

The concept of coherent risk measures was introduced in a seminal paper by Artzner et al. (1999). A risk measure $\rho$ is considered **coherent** if it satisfies the following four axioms:

### 1. **Monotonicity**

The **monotonicity** property ensures that if portfolio $X$ is "better" than portfolio $Y$ in all possible scenarios (i.e., it generates higher returns or has lower losses in every state of the world), then the risk measure $\rho$ should reflect that $X$ is less risky than $Y$. Mathematically:

$$
\text{If } X \geq Y \text{ in all states of the world, then } \rho(X) \leq \rho(Y).
$$

#### Why $X \geq Y$ Implies $\rho(X) \leq \rho(Y)$:

- A risk measure quantifies the "badness" or "danger" of a portfolio in terms of potential losses.
- If $X$ always performs better than $Y$, there is less uncertainty or downside associated with $X$, making it inherently less risky.

3. **Violation of Monotonicity**:
   If a risk measure were to assign $\rho(X) > \rho(Y)$, it would contradict the intuitive understanding of risk. A "better-performing" portfolio being considered riskier does not align with sound risk assessment principles.

### 2. **Sub-additivity**

- For any two portfolios $X$ and $Y$:
  $$
  \rho(X + Y) \leq \rho(X) + \rho(Y)
  $$
- This reflects the principle of diversification: the risk of a combined portfolio should not exceed the sum of individual risks.

### 3. **Homogeneity (Positive Homogeneity)**

- For any portfolio $X$ and scalar $\lambda \geq 0$:
  $$
  \rho(\lambda X) = \lambda \rho(X)
  $$
- This property ensures that scaling the size of a portfolio by a factor $\lambda$ scales its risk measure by the same factor.

### 4. **Translation Invariance**

- For any portfolio $X$ and risk-free asset $c$:
  $$
  \rho(X + c) = \rho(X) - c
  $$
- Adding a certain amount of risk-free cash $c$ reduces the risk measure by $c$, reflecting the reduction of uncertainty.

### Examples of Coherent Risk Measures

1. **Expected Shortfall (ES) or CVaR**: The average of the worst-case losses beyond a certain confidence level. ES is widely used in financial risk management due to its coherence and sensitivity to tail risk.
2. **Risk Measures Based on Utility Functions**: Under certain conditions, utility-based risk measures can also be coherent.

### Non-Coherent Risk Measures

Not all risk measures are coherent. For instance:

- **Value at Risk (VaR)** fails the sub-additivity property under certain conditions, which makes it not coherent despite its widespread use.

### Is Higher $\rho$ Desirable or Lower $\rho$?

- In the context of risk management, **lower $\rho$** is **more desirable** because it implies less risk.
- A **higher $\rho$** means the portfolio is considered riskier based on the specific risk measure.

---

---

# Value at Risk

Value at Risk (VaR) is a fundamental risk measure in quantitative finance, offering insights into potential portfolio losses over a given time period and confidence level. It is widely used for risk management, regulatory reporting, and capital allocation. Here, we delve deeper into the **calculation methods**, **limitations**, and related nuances.

## What is VaR?

VaR estimates the maximum potential loss of a portfolio over a specified time horizon with a given confidence level. For instance, a 1-day VaR of $1 million at the 95% confidence level implies there is a 5% chance that the portfolio will lose more than $1 million in one day.

## Calculation Methods for VaR

There are three primary methods for calculating VaR, each with its own strengths and weaknesses.

### **1. Historical Simulation**

Uses historical data of portfolio returns to estimate the empirical distribution of returns.

**Steps:**

1. Collect historical portfolio return data for a chosen time horizon (e.g., daily returns for the past year).
2. Sort the returns from worst to best.
3. Identify the percentile corresponding to the confidence level (e.g., the 5th percentile for a 95% confidence level).
4. The loss at this percentile is the VaR.

#### **Advantages**

- Does not assume a specific distribution for returns.
- Easy to implement with historical data.

#### **Disadvantages**

- Assumes the future will resemble the past, which may not hold during market shocks.
- Requires a large dataset for accuracy.

### **2. Variance-Covariance Method (Parametric VaR)**

Assumes portfolio returns follow a normal distribution (or a similar parametric distribution).

**Steps:**

1. Calculate the portfolio's mean return ($\mu$) and standard deviation ($\sigma$).
2. Determine the z-score ($Z_\alpha$) for the desired **confidence level** (e.g., -1.645 for 95% confidence).
3. Note the above confidence level is different from the common statistical confidence iterval in which we look at both left & right tail. In VaR, we only look at the left tail.
4. Compute VaR using the formula:
   $$
   \text{VaR} = \mu + Z_\alpha \cdot \sigma
   $$

#### Advantages of VaR

- Computationally efficient for large portfolios, especially when returns are approximately normal.
- Easy to extend to portfolios using correlations and covariance matrices.

#### Disadvantages

- Assumes normality, which may underestimate risk during extreme events (fat tails).
- Less accurate for portfolios with options or other non-linear instruments.

### **3. Monte Carlo Simulation**

Uses random sampling techniques to simulate possible portfolio returns based on assumed return distributions.

**Steps:**

1. Define a statistical model for asset returns (e.g., geometric Brownian motion).
2. Simulate thousands of hypothetical portfolio returns over the chosen time horizon.
3. Construct the distribution of simulated returns and find the percentile corresponding to the confidence level.

#### **Advantages**

- Flexible and can accommodate non-linear portfolios and complex distributions.
- Captures tail risks more effectively than parametric methods.

#### **Disadvantages**

- Computationally intensive, especially for large portfolios.
- Results depend heavily on the accuracy of the chosen model for return generation.

## Limitations of VaR

### **1. Ignores Tail Risk**

- VaR only specifies the loss threshold for a given confidence level but provides no information about the magnitude of losses beyond that threshold.
- This limitation can underestimate the true risk in extreme market conditions.

### **2. Non-Subadditivity**

- VaR may fail to satisfy the subadditivity property, meaning the VaR of a combined portfolio can exceed the sum of the VaRs of individual portfolios:
  $$
  \text{VaR}(A + B) > \text{VaR}(A) + \text{VaR}(B)
  $$
- This violates a key principle of risk diversification.

### **3. Assumption-Dependence**

- Parametric VaR relies on assumptions about return distributions (e.g., normality), which may not hold in practice. Financial returns often exhibit skewness and fat tails.

### **4. Horizon and Scaling Issues**

- VaR assumes returns are independent and identically distributed (i.i.d.) over time. However, in reality, returns may exhibit autocorrelation or volatility clustering, making long-term VaR calculations less reliable.

### **5. Backtesting Challenges**

- Backtesting VaR is difficult due to limited extreme events in historical data. The effectiveness of VaR as a risk measure can degrade if the historical period does not adequately represent future conditions.

## Applications Despite Limitations

Despite its shortcomings, VaR remains a useful tool in finance due to its simplicity and interpretability. It is most effective when:

- Used alongside complementary risk measures like Conditional VaR (CVaR) or stress tests.
- Applied within well-defined risk management frameworks that acknowledge its limitations.

## VaR of a portfolio

### **Inputs and Assumptions**

1. **Portfolio composition:**
   - Asset 1 weight: $w_1$
   - Asset 2 weight: $w_2$
   - $w_1 + w_2 = 1$
2. **Asset statistics:**

   - Expected return of Asset 1: $\mu_1$
   - Expected return of Asset 2: $\mu_2$
   - Standard deviation of Asset 1: $\sigma_1$
   - Standard deviation of Asset 2: $\sigma_2$
   - Correlation between Asset 1 and Asset 2: $\rho_{12}$

3. **Confidence level:** $\alpha$ (e.g., $95\%$ or $99\%$), corresponding to a critical value $Z_\alpha$ from the standard normal distribution.

4. **Time horizon:** $T$ (e.g., daily, weekly).

### **Portfolio Return and Variance**

Let the portfolio return $R_p$ be a weighted sum of the asset returns:

$$
R_p = w_1 R_1 + w_2 R_2
$$

- **Expected portfolio return:**

  $$
  \mu_p = w_1 \mu_1 + w_2 \mu_2
  $$

- **Portfolio variance:**

  $$
  \sigma_p^2 = w_1^2 \sigma_1^2 + w_2^2 \sigma_2^2 + 2 w_1 w_2 \rho_{12} \sigma_1 \sigma_2
  $$

- **Portfolio standard deviation:**
  $$
  \sigma_p = \sqrt{\sigma_p^2}
  $$

### **Final Expression for Two-Asset Portfolio**

Substituting $\sigma_p$ into the VaR equation:

$$
\text{VaR} = \mu_p + Z_\alpha \cdot \sqrt{w_1^2 \sigma_1^2 + w_2^2 \sigma_2^2 + 2 w_1 w_2 \rho_{12} \sigma_1 \sigma_2} \cdot \sqrt{T}
$$

**Absolute VaR:** The monetary value of the potential loss is calculated by multiplying the portfolio value $V$ with the VaR:

$$
\text{Absolute VaR} = V \cdot Z_\alpha \cdot \sigma_p \cdot \sqrt{T}
$$

## Z-Score for Confidence Levels in Value-at-Risk (VaR)

When calculating Value-at-Risk (VaR) using the **Variance-Covariance Method**, the z-score represents the critical value from the standard normal distribution corresponding to a specific confidence level. This is used to identify the threshold below which a certain percentage of returns fall.

#### **Key Points:**

1. **One-Tailed vs. Two-Tailed Confidence Levels:**

   - VaR focuses only on the **lower tail** of the distribution, representing the worst-case losses within a given confidence level. This makes it a **one-tailed test**.
   - In contrast, the commonly cited z-score of $-1.96$ for 95% confidence applies to a **two-tailed confidence interval** (with 2.5% in each tail).

2. **Z-Scores for Common Confidence Levels:**

   - For **95% confidence**, the z-score is $-1.645$, corresponding to the point where the lower 5% of the distribution lies.
   - For **99% confidence**, the z-score is $-2.33$, corresponding to the lower 1% of the distribution.

3. **Why $-1.645$ for 95% Confidence in VaR?**

   - VaR calculations focus on the **worst-case losses** in a one-tailed scenario.
   - The z-score $-1.645$ is derived from the standard normal distribution such that the cumulative probability below this point is 5%.

4. **Formula for VaR:**
   $$
   \text{VaR} = \mu + Z_\alpha \cdot \sigma
   $$
   - $\mu$: Mean return of the portfolio
   - $\sigma$: Standard deviation of portfolio returns
   - $Z_\alpha$: Z-score for the chosen confidence level (e.g., $-1.645$ for 95%)

---

---

# Conditional Value at Risk (CVaR)

**Conditional Value at Risk (CVaR)**, also known as **Expected Shortfall (ES)**, is a risk measure that builds upon the concept of Value at Risk (VaR). Unlike VaR, which only identifies a threshold loss at a given confidence level, CVaR provides information about the **average loss beyond the VaR threshold**. This makes CVaR a more comprehensive measure of risk, especially for tail risk analysis.

CVaR answers the question: **"If losses exceed the VaR, what is the expected average loss?"**

For example: If a portfolio has a daily CVaR of $1.5 million at the 95% confidence level, it means that, in the worst 5% of cases, the portfolio’s average loss will be $1.5 million.

## **Calculation Methods for CVaR**

CVaR can be calculated using different approaches, depending on whether historical data, parametric assumptions, or simulations are used. Below are the main methods:

### **1. Historical Simulation Method**

Uses historical portfolio returns to compute both VaR and the expected losses beyond VaR.

**Steps:**

1. Collect historical portfolio returns for the chosen time horizon (e.g., daily returns for the past year).
2. Sort the returns in ascending order.
3. Identify the losses beyond the VaR threshold (e.g., the worst 5% of returns for a 95% confidence level).
4. Calculate the average of these losses.

#### **Advantages**

- Simple to implement with historical data.
- Does not rely on assumptions about the distribution of returns.

#### **Disadvantages**

- Results are highly dependent on the quality and representativeness of historical data.
- May not adequately capture rare or extreme events if they are absent in the dataset.

### **2. Parametric Method**

- Assumes portfolio returns follow a parametric distribution (e.g., normal, Student's t-distribution).
- CVaR is derived analytically based on the assumed distribution.
- Under normal distribution:
  $$
  \text{CVaR}_\alpha = \mu + \frac{\phi(Z_\alpha)}{1-\alpha} \cdot \sigma
  $$
  where:
  - $\phi(Z_\alpha)$ is the probability density function of the standard normal distribution at the z-score $Z_\alpha$.
  - $\mu$ & $\sigma$ are the mean and standard deviation of portfolio returns.
  - $\sigma$ is the standard deviation of portfolio returns.

#### **Advantages**

- Computationally efficient for portfolios with large numbers of assets.
- Useful when the return distribution is well-understood.

#### **Disadvantages**

- Accuracy depends on the validity of the distributional assumptions.
- Normal distribution often underestimates tail risks (fat tails).

### **3. Monte Carlo Simulation**

#### **Approach**

Generates a large number of hypothetical portfolio return scenarios based on modeled assumptions (e.g., stochastic processes like geometric Brownian motion).

**Steps:**

1. Define a return distribution model or generate random samples of returns.
2. Simulate thousands of portfolio outcomes over the chosen time horizon.
3. Calculate the VaR and the average losses beyond VaR from the simulated data.

#### **Advantages**

- Highly flexible and can accommodate non-linear portfolios and complex risk factors.
- Captures fat tails and non-normal return distributions.

#### **Disadvantages**

- Computationally intensive.
- Results depend heavily on the quality and assumptions of the return generation model.

### **4. Optimization-Based Approach**

#### **Approach**

For portfolios with constraints or involving derivatives, CVaR can be calculated by solving an optimization problem.

In this framework, CVaR is defined as:

$$
  \text{CVaR}_\alpha = \min_{\text{VaR}_\alpha} \left\{ \text{VaR}_\alpha + \frac{1}{1-\alpha} \sum_{i=1}^{N} w_i \cdot \max(0, L_i - \text{VaR}_\alpha) \right\}
$$

where:

- $L_i$ are the portfolio losses.
- $w_i$ are weights for each loss scenario.

#### **Advantages**

- Handles portfolios with complex constraints.
- Ideal for optimization problems in portfolio management.

#### **Disadvantages**

- Requires advanced mathematical tools and computational resources.

## **Advantages of CVaR**

1. **Comprehensive Tail Risk Measure**:

   Unlike VaR, CVaR provides insight into the severity of losses in extreme scenarios.

2. **Coherence**:

   CVaR satisfies all the properties of coherent risk measures, including **subadditivity**. This ensures that diversification reduces risk:

$$
     \text{CVaR}(A + B) \leq \text{CVaR}(A) + \text{CVaR}(B)
$$

3. **Scenario Adaptability**:

   Can handle portfolios with non-linear instruments like options, as it focuses on the actual tail of the distribution.

## Expanded explanation of Optimization-Based Method

The **Optimization-Based Method** frames CVaR as the solution to a constrained optimization problem. This approach is particularly useful for portfolios with constraints or those involving derivatives, as it enables explicit modeling of decision variables.

#### **Mathematical Formulation**

1. **Definition of CVaR:**
   For a portfolio loss $L$ (a random variable) and a confidence level $\alpha$, the CVaR can be expressed as:

   $$
   \text{CVaR}_\alpha = \min_{\text{VaR}} \left\{ \text{VaR} + \frac{1}{1-\alpha} \mathbb{E}[\max(0, L - \text{VaR})] \right\}
   $$

   Here:

   - $\text{VaR}$ is the threshold loss at confidence level $\alpha$.
   - $\max(0, L - \text{VaR})$ captures losses exceeding $\text{VaR}$.

2. **Optimization Problem for CVaR:**
   Suppose the portfolio is subject to a set of losses $\{L_i\}_{i=1}^N$ (e.g., from historical scenarios or simulations). CVaR can be estimated by solving:

   $$
   \text{CVaR}_\alpha = \min_{\nu, \xi_i} \left\{ \nu + \frac{1}{N(1-\alpha)} \sum_{i=1}^N \xi_i \right\}
   $$

   Subject to:

   $$
   \xi_i \geq L_i - \nu, \quad \xi_i \geq 0, \quad \forall i
   $$

   where:

   - $\nu$ is the VaR.
   - $\xi_i$ are auxiliary variables representing the excess losses beyond $\nu$.

3. **Interpretation of Constraints:**

   - The constraint $\xi_i \geq L_i - \nu$ ensures that $\xi_i$ captures only the part of the loss exceeding $\nu$.
   - $\xi_i \geq 0$ prevents negative contributions to the excess losses.

4. **Lagrangian Formulation:**
   To solve the optimization problem, we can write the Lagrangian:

   $$
   \mathcal{L}(\nu, \xi, \lambda) = \nu + \frac{1}{N(1-\alpha)} \sum_{i=1}^N \xi_i + \sum_{i=1}^N \lambda_i \left( L_i - \nu - \xi_i \right)
   $$

   where $\lambda_i$ are Lagrange multipliers enforcing the constraints.

5. **Numerical Solution:**

   - This problem is convex and can be solved efficiently using optimization solvers (e.g., linear programming for discrete scenarios).
   - Modern solvers such as Gurobi or CPLEX can handle large-scale problems involving thousands of scenarios.

6. **Application to Portfolio Optimization:**
   When incorporating CVaR into portfolio optimization, the objective becomes:
   $$
   \min_{w} \text{CVaR}_\alpha(w)
   $$
   where $w$ are portfolio weights. Additional constraints (e.g., $\sum w_i = 1$, $w_i \geq 0$) ensure feasibility.

## CVaR of a portfolio

Let's derive the CVaR formula for a two-asset portfolio with weights $w_1$ and $w_2$ under the parametric method.

Basic assumptions:

1. Returns of both assets follow normal distributions
2. $w_1 + w_2 = 1$ (full investment constraint)
3. Portfolio return is a linear combination of asset returns

Let's derive step by step:

1. First, let's define our variables:

- $R_1 \sim N(\mu_1, \sigma_1^2)$ for asset 1
- $R_2 \sim N(\mu_2, \sigma_2^2)$ for asset 1
- $\rho_{1,2}$ is the correlation between assets 1 and 2

2. The portfolio return Rₚ will be:
   $$R_p = w_1R_1 + w_2R_2$$

3. Since both assets are normally distributed, the portfolio return is also normally distributed with:

Portfolio mean:
$$\mu_p = w_1\mu_1 + w_2\mu_2$$

Portfolio variance:
$$\sigma_p^2 = w_1^2\sigma_1^2 + w_2^2\sigma_2^2 + 2w_1w_2\sigma_1\sigma_2\rho_{12}$$

4. Now we can use our single-asset CVaR formula with these portfolio parameters.
   For confidence level α:

$$CVaR_{\alpha} = -(\mu_p + \sigma_p \frac{\phi(\Phi^{-1}(1-\alpha))}{1-\alpha})$$

5. Substituting the portfolio parameters:

$$CVaR_{\alpha} = -(w_1\mu_1 + w_2\mu_2 + \sqrt{w_1^2\sigma_1^2 + w_2^2\sigma_2^2 + 2w_1w_2\sigma_1\sigma_2\rho_{12}} \cdot \frac{\phi(\Phi^{-1}(1-\alpha))}{1-\alpha})$$


---

---

# Why VaR is incoherent & CVaR is coherent

### Example of VaR Being Incoherent

**Value at Risk (VaR)** at confidence level $\alpha$ is defined as the threshold loss level such that the probability of losses exceeding this threshold is $1 - \alpha$. Formally:

$$
\text{VaR}_\alpha(X) = \inf \{ L \in \mathbb{R} : P(X \leq L) \geq 1 - \alpha \},
$$

where $X$ is the loss distribution.

#### Consider Two Portfolios $X$ and $Y$:

1. Portfolio $X$ and $Y$ have losses that are independent, with the following distributions:

   - $X$: $P(\text{Loss} = 0) = 0.99$, $P(\text{Loss} = 100) = 0.01$.
   - $Y$: $P(\text{Loss} = 0) = 0.99$, $P(\text{Loss} = 100) = 0.01$.

2. For $X + Y$, the combined portfolio, losses are:

   - $P(\text{Loss} = 0) = 0.9801$ (both $X$ and $Y$ have no loss),
   - $P(\text{Loss} = 100) = 0.0198$ (either $X$ or $Y$ has a loss),
   - $P(\text{Loss} = 200) = 0.0001$ (both $X$ and $Y$ have a loss).

3. **Calculate VaR at $\alpha = 0.95$:**
   - For $X$, $\text{VaR}_{0.95}(X) = 0$, because the 95th percentile of $X$ is zero.
   - For $Y$, $\text{VaR}_{0.95}(Y) = 0$, because the 95th percentile of $Y$ is also zero.
   - For $X + Y$, $\text{VaR}_{0.95}(X + Y) = 100$, because the combined portfolio has a 98.01% chance of loss $\leq 100$.

#### Result:

$$
\text{VaR}_{0.95}(X + Y) = 100 \not\leq \text{VaR}_{0.95}(X) + \text{VaR}_{0.95}(Y) = 0 + 0 = 0.
$$

This violates the **sub-additivity property**, making VaR **incoherent**.

### Example of CVaR Being Coherent

**Conditional Value at Risk (CVaR)** at confidence level $\alpha$ is the expected loss, given that losses exceed the $\text{VaR}_\alpha$. Formally:

$$
\text{CVaR}_\alpha(X) = \mathbb{E}[X \mid X \geq \text{VaR}_\alpha(X)].
$$

#### For the Same Portfolios:

- $X$: $P(\text{Loss} = 0) = 0.99$, $P(\text{Loss} = 100) = 0.01$.
- $Y$: $P(\text{Loss} = 0) = 0.99$, $P(\text{Loss} = 100) = 0.01$.

When combined, the portfolio $X + Y$ will have the following distribution:

- $P(\text{Loss} = 0) = 0.9801$ (both $X$ and $Y$ have no loss),
- $P(\text{Loss} = 100) = 0.0198$ (either $X$ or $Y$ has a loss),
- $P(\text{Loss} = 200) = 0.0001$ (both $X$ and $Y$ have a loss).

#### Step-by-Step CVaR Calculations:

1. **For $X$:**

   - $\text{VaR}_{0.95}(X) = 0$, because the 95th percentile of $X$ is 0.
   - The tail (losses greater than or equal to 0) consists only of a 1% chance of losing 100. Hence:
     $$
     \text{CVaR}_{0.95}(X) = \mathbb{E}[X \mid X \geq 0] = \frac{100 \times 0.01 + 0 \times 0.99}{0.01} = 100.
     $$

2. **For $Y$:**

   - Similarly, $\text{VaR}_{0.95}(Y) = 0$, and the tail is identical to $X$.
   - Hence, the CVaR is the same as for $X$:
     $$
     \text{CVaR}_{0.95}(Y) = 100.
     $$

3. **For $X + Y$:**
   - The combined portfolio $X + Y$ has $P(\text{Loss} = 100) = 0.0198$ and $P(\text{Loss} = 200) = 0.0001$.
   - The **VaR** at the 95% confidence level is 100, because the 95th percentile of $X + Y$ is 100.
   - Now calculate the **CVaR**:
     $$
     \text{CVaR}_{0.95}(X + Y) = \mathbb{E}[X + Y \mid X + Y \geq 100].
     $$
     The expected loss given that we are in the tail is:
     $$
     \text{CVaR}_{0.95}(X + Y) = \frac{100 \times 0.0198 + 200 \times 0.0001}{0.0199} = \frac{1.98 + 0.02}{0.0199} \approx 100.
     $$

### Sub-additivity Check:

Now, for sub-additivity:

- We have $\text{CVaR}_{0.95}(X) = 100$, $\text{CVaR}_{0.95}(Y) = 100$, and $\text{CVaR}_{0.95}(X + Y) \approx 100$.
- The sum of the individual CVaRs is:
  $$
  \text{CVaR}_{0.95}(X) + \text{CVaR}_{0.95}(Y) = 100 + 100 = 200.
  $$
- The combined portfolio's CVaR is $100$, which satisfies the **sub-additivity** property because:
  $$
  \text{CVaR}_{0.95}(X + Y) \leq \text{CVaR}_{0.95}(X) + \text{CVaR}_{0.95}(Y),
  $$
  ensuring that diversification does not lead to an increase in risk.

Thus, **CVaR is coherent** because it satisfies **sub-additivity**, **monotonicity**, **homogeneity**, and **translation invariance**.

# References

- https://quantpedia.com/an-introduction-to-value-at-risk-methodologies/
-
