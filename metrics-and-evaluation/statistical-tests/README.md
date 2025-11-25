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

Because both models are evaluated on the **same 200 samples**, the data are **paired**.

### How It Works (Step by Step)

**Step 1: Compute differences** for each sample: `dᵢ = aᵢ - bᵢ`

- Sample 1: d₁ = 82.1 - 6.4 = 75.7 (positive, InvertiTune better)
- Sample 2: d₂ = 81.8 - 6.9 = 74.9 (positive, InvertiTune better)
- Sample 3: d₃ = 81.5 - 82.0 = -0.5 (negative, DeepEx better)
- ... continue for all 200 samples

**Intuition:** If the two models are truly equal, then on some samples InvertiTune will score higher (positive `dᵢ`), and on other samples DeepEx will score higher (negative `dᵢ`). These differences should be **balanced** in both direction and magnitude. The Wilcoxon test checks whether this balance holds or if one direction dominates.

**Step 2: Rank the absolute differences** `|dᵢ|` from smallest to largest

Ignore the sign, just look at magnitude:
- |d₃| = 0.5 gets rank 1 (smallest difference)
- |d₂| = 74.9 gets rank 2
- |d₁| = 75.7 gets rank 3 (largest difference)
- ... continue for all 200 samples

**Step 3: Sum ranks separately** for positive and negative differences

- R⁺ = sum of ranks where dᵢ > 0 (InvertiTune wins)
- R⁻ = sum of ranks where dᵢ < 0 (DeepEx wins)

**Step 4: Compare R⁺ and R⁻**

- If models are equal (H₀), then R⁺ and R⁻ should be similar
- If InvertiTune is truly better, R⁺ will be much larger than R⁻
- The test computes a **p-value** based on how different R⁺ and R⁻ are

### Python Implementation

```python
import numpy as np
from scipy import stats

# Per-sample scores for 200 test samples
scores_invertitune = [82.1, 81.9, 82.3, ...]  # 200 values
scores_deepex = [6.4, 6.2, 6.5, ...]          # 200 values

# Perform Wilcoxon signed-rank test
stat, p_value = stats.wilcoxon(scores_invertitune, scores_deepex)

print(f"InvertiTune mean: {np.mean(scores_invertitune):.2f}")
print(f"DeepEx mean: {np.mean(scores_deepex):.2f}")
print(f"p-value: {p_value:.4e}")

if p_value < 0.001:
    print("→ InvertiTune significantly outperforms DeepEx (p < 0.001)")
```

### What is a p-value?

The **p-value** answers: "If the models were actually equal, how likely is it to see a difference this large just by random chance?"

For our example:
- InvertiTune: 82.02
- DeepEx: 6.32
- Difference: 75.7 points

The p-value tells us: Could this huge 75.7-point difference happen by random luck if the models were actually the same?

- **p = 0.001** (very small): Only 0.1% chance this could happen by luck → The models are **truly different**
- **p = 0.4** (large): 40% chance this could happen by luck → We **cannot conclude** they're different

**Simple rule:**
- **p < 0.05**: The difference is real (statistically significant)
- **p < 0.01**: Very strong evidence
- **p < 0.001**: Extremely strong evidence

## What to Report in a Paper

When presenting results:

1. **Point estimates with confidence intervals:**
   - InvertiTune: 82.02 (81.01, 83.00)
   - DeepEx: 6.32 (6.08, 6.57)

2. **Statistical test results:**
   - "InvertiTune significantly outperforms all baselines (p < 0.001, Wilcoxon signed-rank test)"

---

**Key Takeaway:** Use bootstrap confidence intervals to quantify uncertainty, and use statistical tests to establish that observed differences are significant rather than due to chance.
