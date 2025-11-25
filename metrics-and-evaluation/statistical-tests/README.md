# Statistical Tests for Model Comparison

## What is a Statistical Test?

A statistical test answers the question: **"Is the difference between two models real, or could it have happened by chance?"**

Suppose we have a test set of **200 samples** and we compare two models:
- **InvertiTune**: average score = 82.02  
- **DeepEx**: average score = 6.32  

We use statistical tests to determine whether InvertiTune is truly better, rather than simply appearing better due to random variation.

## The Wilcoxon Signed-Rank Test

For each sample `i` (where `i = 1, 2, ..., 200`):
- Let `aᵢ` = InvertiTune's score on sample `i`  
- Let `bᵢ` = DeepEx's score on sample `i`  

### How It Works (Step by Step)

**Step 1: Compute differences** for each sample: `dᵢ = aᵢ - bᵢ`

- Sample 1: d₁ = 82.1 - 6.4 = 75.7 (positive, InvertiTune better)
- Sample 2: d₂ = 81.8 - 6.9 = 74.9 (positive, InvertiTune better)
- Sample 3: d₃ = 81.5 - 82.0 = -0.5 (negative, DeepEx better)
- ... continue for all 200 samples

**Step 2: Rank the absolute differences** `|dᵢ|` from smallest to largest

Ignore the sign, just look at magnitude:
- |d₃| = 0.5 gets rank 1 (smallest difference)
- |d₂| = 74.9 gets rank 2
- |d₁| = 75.7 gets rank 3 (largest difference)
- ... continue for all 200 samples

**Why rank?** Because we want to give more importance to **larger differences**. A big difference is more convincing evidence than a small difference.

**Step 3: Assign signs back to the ranks**

Now we look at whether each difference was positive or negative:
- d₃ = -0.5 (negative) → rank 1 goes to the "negative" group
- d₂ = 74.9 (positive) → rank 2 goes to the "positive" group  
- d₁ = 75.7 (positive) → rank 3 goes to the "positive" group

**Step 4: Sum the ranks separately**

- **R⁺** = sum of ranks for positive differences (where InvertiTune wins)
  - In our example: R⁺ = 2 + 3 = 5
- **R⁻** = sum of ranks for negative differences (where DeepEx wins)
  - In our example: R⁻ = 1

**Step 5: Compare R⁺ and R⁻**

**Null hypothesis (H₀):** The two models have equal performance.

This is what we're testing against. If H₀ is true, then R⁺ and R⁻ should be similar.

- In our example: R⁺ = 5 is much larger than R⁻ = 1, suggesting InvertiTune is better
- The test computes a **p-value** based on how different R⁺ and R⁻ are
- If the p-value is very small, we reject H₀ and conclude the models are truly different

### What is a p-value?

The **p-value** answers: "If the models were actually equal (H₀ is true), how likely is it to see a result as extreme as what we observed, just by random chance?"

In our example, we observed:
- R⁺ = 5 (InvertiTune wins on the larger differences)
- R⁻ = 1 (DeepEx wins on the smaller difference)
- Overall: InvertiTune scores 82.02, DeepEx scores 6.32

The p-value tells us: If the models were truly equal, what's the probability of seeing R⁺ so much larger than R⁻ just by luck?

- **p = 0.001** (very small): Only 0.1% chance this could happen by luck → The models are **truly different**
- **p = 0.4** (large): 40% chance this could happen by luck → We **cannot conclude** they're different

**Simple rule:**
- **p < 0.05**: The difference is real (statistically significant)
- **p < 0.01**: Very strong evidence
- **p < 0.001**: Extremely strong evidence

### Python Implementation

```python
from scipy import stats

# Small example lists (replace with your values)
scores_invertitune = [82.1, 81.8, 81.5, 82.3, 82.0]
scores_deepex      = [6.4,  6.9,  8.2,  6.1,  6.3]

# Wilcoxon signed-rank test (InvertiTune > DeepEx)
stat, p_value = stats.wilcoxon(scores_invertitune, scores_deepex, alternative="two-sided")

print("Statistic:", stat)
print("p-value:", p_value)
```