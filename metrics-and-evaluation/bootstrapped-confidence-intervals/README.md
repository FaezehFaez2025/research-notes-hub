# Bootstrap Confidence Intervals

## What is Bootstrap Confidence Interval?

Bootstrap confidence intervals provide a way to quantify the uncertainty of evaluation metrics. This method allows us to estimate how much our metrics would vary if we were to evaluate on different test sets drawn from the same distribution.

## Example: Test Set with 200 Samples

Consider a test set containing 200 samples. For each sample `i` (where `i = 1, 2, ..., 200`), let `sᵢ` denote its score (e.g., G-BLEU). The collection of all per-sample scores is `{s₁, s₂, ..., s₂₀₀}`.

### Point Estimate

The average score provides a point estimate of model performance:

```
μ̂ = (1/200) × Σᵢ₌₁²⁰⁰ sᵢ
```

where `sᵢ` is the score for the i-th sample.

However, this point estimate does not capture the variability that would arise if the evaluation were repeated on different test sets drawn from the same distribution.

### Bootstrap Resampling

To estimate this variability, we apply **non-parametric bootstrap resampling**:

1. Generate **B = 10,000** bootstrap samples
2. Each bootstrap sample is constructed by randomly sampling **200 items with replacement** from the original test set (i.e., resampling the 200 samples with replacement)
3. For each bootstrap sample `b`, compute the corresponding metric value `μ̂⁽ᵇ⁾` by averaging the scores of the resampled items

The distribution of bootstrap metric values:
```
{μ̂⁽¹⁾, μ̂⁽²⁾, ..., μ̂⁽ᴮ⁾}
```
approximates how the metric would vary over repeated evaluations.

### 95% Confidence Interval

The **95% bootstrap confidence interval** is obtained using the percentile method:

```
CI₉₅% = (μ̂₂.₅%, μ̂₉₇.₅%)
```

where:
- `μ̂₂.₅%` denotes the 2.5th percentile of the bootstrap distribution
- `μ̂₉₇.₅%` denotes the 97.5th percentile of the bootstrap distribution

**What is a percentile?** A percentile is a value below which a given percentage of observations fall. For example:
- The **2.5th percentile** is the value such that 2.5% of all bootstrap metric values are below it
- The **97.5th percentile** is the value such that 97.5% of all bootstrap metric values are below it (or equivalently, 2.5% are above it)

To find these percentiles, we sort all 10,000 bootstrap metric values `{μ̂⁽¹⁾, μ̂⁽²⁾, ..., μ̂⁽¹⁰⁰⁰⁰⁾}` in ascending order, then:
- The 2.5th percentile is the value at position 250 (2.5% of 10,000)
- The 97.5th percentile is the value at position 9,750 (97.5% of 10,000)

### Example Result

A result reported as **82.02 (81.01, 83.00)** indicates that:
- The point estimate is **82.02**
- Across resampled datasets of size 200, the metric typically lies within the interval **(81.01, 83.00)** with **95% confidence**

This means that if we were to repeat the evaluation on many different test sets of the same size from the same distribution, we would expect the metric to fall within this interval 95% of the time.

