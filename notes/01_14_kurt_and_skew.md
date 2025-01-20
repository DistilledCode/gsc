# Kurtosis & Skewness

## Kurtosis

Tell us the tailedness/thickness of the distribution or the tendency for the distribution to produce outliers

### Positive Kurtosis: Leptokurtic (K>3)

* Heavier tails than normal distribution
* Have more data in the tails and near the peak, with fewer in the shoulders compared to a normal distribution.
* Higher probability of extreme returns
* Observed in volatile markets

### Zero Kurtosis: Mesokurtic (K=3)

* Normal/Gaussian distribution

### Negative Kurtosis: Platykurtic (K<3)

* Lighter tails than normal distribution
* Datapoints are more concentrated about mean (when compared to Normal Distribution)
* Lower probability of extreme returns
* Observed in stable markets

### Additional Note:

* For financial analysis or real-world applications, practitioners often use "excess kurtosis" $(K−3)$ instead of raw kurtosis.
* Raw Kurtosis is always positive, as it's a fourth moment.

## Skewness

Tell us the asymmetry of the distribution about mean

### Positive Skewness

* Mean > Median
* Positive deviations from mean are larger than negative ones

### Zero Skewness

* Mean = Median
* Balanced extremes on both sides
* The positive and negative deviations from the mean are balanced
* Magnitude wise, both positive & negative deviation can be low, moderate or high. But if skewness is zero then deviation is balanced on both ends magnitude wise

### Negative Skewness

* Mean < Median
* Negative deviations from mean are larger than positive ones

## Summary:

* Kurtosis tells us the likelihood of extreme returns (both positive and negative)
* Skewness tells us which direction (positive or negative) these extreme returns tend to occur

## Important Note

Zero skewness doesn't mean symmetric distribution. There exist asymmetric continuous distribution with zero skewness.

Reference: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2531847

X = Normal(mu = -2, sigma = 1)
Y = Normal(mu = 1, sigma = sqrt(2))
Z = I * X + (1-I) * Y
Where I is indicator variable such that P(I=1) = 1/3
Then Skewness(Z) = 0, Mean(Z) = 0 while the distribution is asymmetric

Hence, Zero skewness only means that deviation from mean is balanced on both ends.