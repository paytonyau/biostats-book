# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 7: Scaling Up
## *ANOVA, Comparison of Proportions, and Chi-Square*

> **Course:** HS 4510 Biostatistics | **Unit:** 7 | **Part:** III — Complex Connections
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Explain the **multiple comparisons problem** and calculate the family-wise error rate for k tests
- Run a **one-way ANOVA** and follow up a significant result with **Tukey's HSD**
- Compute **Risk Difference** and **Relative Risk** from a 2×2 table and run a **two-proportion z-test**
- Build a contingency table and run a **Chi-Square test**, checking expected-count assumptions
```


---

## Introduction: When Two Groups Are Not Enough

In Chapter 6, the t-test provided a clean answer to a binary question: do Broad Street pump victims differ in age from Rupert Street pump victims? One outcome variable, two groups, one comparison. The t-test was perfectly suited to that design.

But public health problems rarely present themselves so neatly. Dr Snow did not investigate just two pumps — he mapped thirteen pumps across Soho. A modern epidemiologist comparing treatment outcomes might have three arms, not two: a new drug, a standard-care comparison, and a placebo control. A public health surveillance team might want to compare disease incidence across five geographical zones simultaneously.

When the number of groups grows beyond two, a new problem emerges — one that invalidates the straightforward strategy of simply repeating the t-test for every pair of groups you wish to compare.

---

## Section 1: The Multiple Comparisons Problem

### 1.1 Why Repeated T-Tests Fail

Consider a scenario in which we wish to compare mean travel distances for victims of three different pumps: Broad Street, Rupert Street, and Marlborough Street. There are three possible pairwise comparisons: Broad vs. Rupert, Broad vs. Marlborough, and Rupert vs. Marlborough. It is tempting to run three separate t-tests and read the three resulting p-values.

This strategy is statistically invalid, and the reason is arithmetical.

Each individual t-test is run at α = 0.05 — meaning there is a 5% probability of a Type I error (a false positive) for that test alone, *if* H₀ is true for that pair. When we run multiple tests simultaneously, these error probabilities accumulate. The **family-wise error rate** — the probability that *at least one* of our comparisons will produce a false positive — grows with every additional test:

| Number of groups | Number of pairwise t-tests | Approximate family-wise Type I error rate |
|:---:|:---:|:---:|
| 2 | 1 | 5.0% |
| 3 | 3 | ~14% |
| 4 | 6 | ~26% |
| 5 | 10 | ~40% |
| 6 | 15 | ~54% |

With three groups and three t-tests, we accept approximately a 14% chance that at least one of our "significant" findings is a false alarm — nearly three times the agreed risk. With five groups and ten t-tests, we are more likely than not to generate at least one spurious significant result. Under these conditions, the p-value threshold of 0.05 is meaningless: we have abandoned the protection it was designed to provide.

> **⚠️ The inflation formula**
>
> The exact family-wise error rate for k independent tests, each at significance threshold α, is:
>
> $$\text{Family-wise error rate} = 1 - (1 - \alpha)^k$$
>
> For k = 3 tests at α = 0.05: 1 − (0.95)³ = 1 − 0.857 = **0.143** (14.3%)
>
> For k = 6 tests at α = 0.05: 1 − (0.95)⁶ = 1 − 0.735 = **0.265** (26.5%)
>
> This is not a theoretical concern — it is a mathematical certainty. Running multiple t-tests inflates the false positive rate in direct proportion to the number of tests conducted.

---

### 1.2 The Solution: One Protected Analysis

The solution is to perform a single omnibus test — one analysis that simultaneously evaluates all groups while maintaining the family-wise Type I error rate at exactly 5%. For continuous outcome variables, this test is the **Analysis of Variance (ANOVA)**. For categorical outcome variables, it is the **Chi-Square test**.

---

## Section 2: One-Way Analysis of Variance (ANOVA)

### 2.1 What ANOVA Asks

A **one-way ANOVA** compares the means of three or more independent groups on a single continuous outcome variable. The "one-way" refers to the fact that groups are defined by one categorical factor (in our case, the pump used).

**Our question for the Snow dataset:**

Does the average distance victims travelled to collect water differ across the three pump groups (Broad Street, Rupert Street, and Marlborough Street)?

| | Hypothesis | Notation |
|---|---|---|
| **H₀** | All three pump groups have the same population mean distance. Any observed differences between group averages are due to random sampling variation. | μ₁ = μ₂ = μ₃ |
| **Hₐ** | At least one pump group has a mean distance that is significantly different from the others. | At least one μᵢ ≠ the rest |

> **A crucial asymmetry:** The Alternative Hypothesis for ANOVA is deliberately vague — it states only that *at least one* group mean differs. It does not specify which group, or how many. This is intentional: ANOVA is an omnibus test designed to detect the presence of *any* difference whilst protecting the family-wise error rate. Identifying *which* groups differ requires a separate post-hoc test (Section 2.4).

---

### 2.2 The F-Statistic — Signal Over Noise

It may seem paradoxical that a test *for differences in means* is called Analysis of *Variance*. The logic becomes clear once you understand what the F-statistic measures.

ANOVA decomposes the total variability in the dataset into two components:

**Numerator — Between-Group Variance (the signal):**
How far apart are the three pump group averages from the overall (grand) mean? If Broad Street victims lived, on average, very close to the pump whilst Marlborough Street victims lived much further away, the between-group variance is large. This is the signal — the variation we are trying to explain by group membership.

**Denominator — Within-Group Variance (the noise):**
How scattered are individual observations around their own group's mean? Even within the Broad Street group, individual victims lived at different distances. This natural within-group scatter represents measurement noise and the inherent variability between individuals — variation that is unrelated to which pump they used.

$$F = \frac{\text{Between-Group Variance (signal)}}{\text{Within-Group Variance (noise)}}$$

**Interpreting the F-statistic:**

| F-statistic | What it tells you | Decision |
|:---:|---|---|
| **F ≈ 1** | The groups overlap heavily — the between-group spread is no larger than the within-group noise. | Fail to reject H₀ — all pump groups are similar. |
| **F > 1 (moderate)** | Some separation between group means relative to within-group scatter. | Check the p-value before deciding. |
| **F >> 1 (large)** | The group means are clearly separated relative to the noise within each group. Strong signal. | Reject H₀ — at least one pump group differs significantly. |

When F ≈ 1, the between-group variation is no greater than what we would expect from random noise alone — the groups look like they were drawn from the same population. When F is substantially greater than 1, the group means are further apart than the within-group noise can explain by chance — the group membership is accounting for real variation in the outcome.

---

### 2.3 The ANOVA Table — Reading the Output

Both PSPP and R produce a standard ANOVA summary table. The key elements are:

| Column | What it contains |
|---|---|
| **Sum of Squares (SS)** | The raw quantity of variance — Between-group SS and Within-group SS (also called Residual or Error SS). |
| **Degrees of Freedom (df)** | Between-group df = number of groups − 1. Within-group df = total observations − number of groups. |
| **Mean Square (MS)** | SS ÷ df for each row. The F-statistic is computed as MS_between ÷ MS_within. |
| **F** | The F-statistic. |
| **p-value (Pr > F in R; Sig. in PSPP)** | The probability of observing an F-statistic this large if H₀ is true. Compare to α = 0.05. |

> **Degrees of freedom in ANOVA**
>
> For a one-way ANOVA comparing k groups with N total observations:
> - Between-group df = k − 1. For three pumps: df = 3 − 1 = **2**.
> - Within-group df = N − k. For 190 observations across three groups: df = 190 − 3 = **187**.
> - Total df = N − 1 = 189.
>
> The F-statistic is always reported with both degrees of freedom: F(df_between, df_within). For example: F(2, 187) = 8.41, p = 0.0003.

---

### 2.4 Assumptions of One-Way ANOVA

Before running a one-way ANOVA, verify the following assumptions:

| Assumption | What it requires | How to check it |
|---|---|---|
| **Independence** | Observations are independent — each participant contributes one value. | Study design review. |
| **Continuous outcome** | The dependent variable is interval or ratio scale. | Level of measurement classification (Chapter 1). |
| **Approximate Normality within each group** | The outcome is approximately Normally distributed within each group. | Histogram or Shapiro-Wilk test per group (Chapter 4). Robust to moderate violations when group sizes exceed 30. |
| **Homogeneity of variance** | All groups have approximately equal variance. | Levene's Test (similar to Chapter 6). If violated with unequal group sizes, consider Welch's ANOVA. |

---

### 2.5 The Critical Limitation — ANOVA Finds "A" Difference, Not "The" Difference

A statistically significant ANOVA result (p < 0.05) carries one piece of information: somewhere among your groups, at least one mean is significantly different from at least one other. It does not specify which group or groups are responsible.

In our three-pump example, a significant ANOVA could mean any of the following:
- Broad Street differs from both Rupert Street and Marlborough Street, whilst the latter two are similar to each other.
- All three pumps are different from one another.
- Only one specific pair differs; the other pairs are essentially equivalent.

The ANOVA p-value cannot distinguish between these scenarios. A separate, **post-hoc test** is required.

---

### 2.6 Post-Hoc Tests — Tukey's HSD

**Tukey's Honestly Significant Difference (Tukey's HSD)** is the most widely used post-hoc test following a significant one-way ANOVA. It systematically tests every possible pairwise comparison of group means whilst applying a correction that keeps the family-wise Type I error rate at exactly α = 0.05 across all comparisons simultaneously.

For three pumps, Tukey's HSD produces three pairwise results:

| Comparison | Adjusted p-value | Interpretation |
|---|:---:|---|
| Broad Street vs. Rupert Street | p = ? | Significant if < 0.05 |
| Broad Street vs. Marlborough Street | p = ? | Significant if < 0.05 |
| Rupert Street vs. Marlborough Street | p = ? | Significant if < 0.05 |

Each adjusted p-value reflects the correction for multiple comparisons — it will be larger than the unadjusted t-test p-value would be for the same pair. This is the cost of protection: to keep the family-wise error rate at 5%, we require stronger evidence from each individual comparison.

> **When to run post-hoc tests:** Post-hoc tests are run *only* after a significant ANOVA result. If the omnibus ANOVA is not significant (p ≥ 0.05), there is no statistical warrant to search for pairwise differences. Running post-hoc tests after a non-significant ANOVA is a form of p-hacking.

> **Other post-hoc options:** Tukey's HSD assumes equal group sizes and equal variances. When group sizes are unequal, the Games-Howell test (which does not assume equal variances) is preferable. Both are available in R via the `agricolae` or `emmeans` packages.

---

---

## Section 3: Comparing Two Proportions

### 3.1 From Means to Proportions

ANOVA and t-tests compare **means** — they require a continuous outcome variable. But many of the most important questions in epidemiology and clinical research concern **proportions**: the fraction of each group that experienced a binary outcome.

- What proportion of people who drank contaminated pump water developed cholera, compared with those who did not?
- What proportion of vaccinated patients were hospitalised, compared with unvaccinated patients?
- What proportion of patients in the treatment arm recovered within 14 days, compared with the control arm?

Each of these questions involves two groups and a binary outcome (cholera / no cholera; hospitalised / not hospitalised; recovered / not recovered). The appropriate tools are the **two-proportion z-test** and two accompanying effect-size measures: **risk difference** and **relative risk**.

---

### 3.2 The 2 × 2 Table — Anatomy of a Proportion Comparison

All comparison-of-proportions analyses begin with a **2 × 2 contingency table** — two exposure groups crossed against a binary outcome:

|  | Outcome: Yes | Outcome: No | Row Total |
|---|:---:|:---:|:---:|
| **Exposed (Group 1)** | a | b | a + b |
| **Unexposed (Group 2)** | c | d | c + d |
| **Column Total** | a + c | b + d | n |

Where:
- **a** = exposed individuals who experienced the outcome
- **b** = exposed individuals who did not experience the outcome
- **c** = unexposed individuals who experienced the outcome
- **d** = unexposed individuals who did not experience the outcome

**Snow example:** Classify 150 households as "Near pump" (≤ 100m) versus "Far from pump" (> 100m), and record whether each household had at least one cholera death.

|  | Death: Yes | Death: No | Row Total |
|---|:---:|:---:|:---:|
| **Near pump (≤ 100m)** | 68 | 22 | 90 |
| **Far from pump (> 100m)** | 12 | 48 | 60 |
| **Column Total** | 80 | 70 | 150 |

---

### 3.3 Risk Difference and Relative Risk — The Epidemiologist's Effect Sizes

Before running any significance test, compute the two proportions and their effect-size measures. Effect sizes communicate the *magnitude* and *direction* of the difference between groups — something the p-value cannot do.

**Proportion (Risk) in each group:**

$$p_1 = \frac{a}{a+b} \quad \text{(proportion of exposed who experienced the outcome)}$$

$$p_2 = \frac{c}{c+d} \quad \text{(proportion of unexposed who experienced the outcome)}$$

Using the Snow example:

$$p_1 = \frac{68}{90} = 0.756 \quad (75.6\% \text{ of near-pump households had a death})$$

$$p_2 = \frac{12}{60} = 0.200 \quad (20.0\% \text{ of far-pump households had a death})$$

**Risk Difference (RD)** — the absolute difference in proportions:

$$RD = p_1 - p_2 = 0.756 - 0.200 = +0.556$$

*Interpretation:* Households near the pump had a 55.6 percentage point higher probability of suffering a cholera death than households far from the pump. This is the absolute excess risk attributable to proximity.

**Relative Risk (RR)** — the ratio of the two proportions:

$$RR = \frac{p_1}{p_2} = \frac{0.756}{0.200} = 3.78$$

*Interpretation:* Households near the pump were 3.78 times as likely to suffer a cholera death as households far from the pump. Relative risk is the most widely reported effect measure in cohort studies and outbreak investigations.

| RR value | Interpretation |
|:---:|---|
| **RR = 1** | The two groups have identical risk. No association between exposure and outcome. |
| **RR > 1** | The exposed group has higher risk. The exposure is positively associated with the outcome. |
| **RR < 1** | The exposed group has lower risk. The exposure is protective. |
| **RR = 3.78** | The exposed group is nearly four times as likely to experience the outcome. A clinically large effect. |

> **Risk difference vs. relative risk: which matters more?**
>
> Both measures are needed. Relative risk communicates the strength of association — an RR of 3.78 is striking regardless of context. Risk difference communicates the absolute burden — it tells you how many extra cases per 100 (or per 1,000) population are attributable to the exposure, which is what matters for resource allocation and policy. A vaccine that reduces risk from 0.02% to 0.01% has RR = 0.5 (a 50% relative reduction) but RD = −0.01% (a tiny absolute reduction). Whether that vaccine should be mandated depends on the absolute numbers, not the relative ratio alone.

---

### 3.4 The Two-Proportion Z-Test

Having computed the two proportions and their effect sizes, the next step is to test whether the observed difference could plausibly have arisen by chance. The **two-proportion z-test** (also called the test for equality of two proportions) tests:

- **H₀:** p₁ = p₂ (the two population proportions are equal)
- **Hₐ:** p₁ ≠ p₂ (the two population proportions differ) — two-tailed

The test statistic is:

$$z = \frac{p_1 - p_2}{\sqrt{\hat{p}(1-\hat{p})\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}}$$

Where **p̂** is the **pooled proportion** — the overall proportion across both groups combined, used to estimate the common population proportion under H₀:

$$\hat{p} = \frac{a + c}{n_1 + n_2} = \frac{a + c}{n}$$

Under H₀, the z-statistic follows a standard Normal distribution. The p-value is obtained from the Normal distribution tables (or software). For a two-tailed test, reject H₀ if |z| > 1.96 at α = 0.05.

**Worked example (Snow data):**

Pooled proportion: $\hat{p} = \frac{68 + 12}{90 + 60} = \frac{80}{150} = 0.533$

Standard error: $SE = \sqrt{0.533 \times 0.467 \times \left(\frac{1}{90} + \frac{1}{60}\right)} = \sqrt{0.249 \times 0.0278} = \sqrt{0.00692} = 0.0832$

Z-statistic: $z = \frac{0.756 - 0.200}{0.0832} = \frac{0.556}{0.0832} = 6.68$

With |z| = 6.68 >> 1.96, we reject H₀ with overwhelming confidence: p < 0.001. The difference in attack rates between near-pump and far-pump households is not plausibly a chance occurrence.

> **Assumptions of the two-proportion z-test:**
> - Each observation is independent.
> - Each cell in the 2 × 2 table contains at least 5 observations (for the Normal approximation to be valid). If any cell is below 5, use Fisher's Exact Test instead (covered in Section 4).
> - The data arise from two independent groups, not paired or matched observations.

---

### 3.5 Confidence Interval for the Risk Difference

A significance test alone is insufficient. A 95% confidence interval for the risk difference quantifies both the precision of the estimate and its practical range:

$$RD \pm 1.96 \times \sqrt{\frac{p_1(1-p_1)}{n_1} + \frac{p_2(1-p_2)}{n_2}}$$

$$= 0.556 \pm 1.96 \times \sqrt{\frac{0.756 \times 0.244}{90} + \frac{0.200 \times 0.800}{60}}$$

$$= 0.556 \pm 1.96 \times \sqrt{0.00205 + 0.00267} = 0.556 \pm 1.96 \times 0.0687$$

$$= 0.556 \pm 0.135 = [0.421, \; 0.691]$$

*Interpretation:* We are 95% confident that the true difference in attack rates between near-pump and far-pump households lies between 42.1 and 69.1 percentage points. Because this interval does not contain zero, the result is statistically significant — consistent with the p-value from the z-test.

---

## Section 4: The Chi-Square Test for Independence

### 4.1 When Means Are Not the Point

ANOVA and t-tests require a continuous outcome variable — they compare means. But many of the most important questions in public health involve *categorical* outcomes where no mean can be computed:

- Is vaccination status (vaccinated / unvaccinated) associated with disease outcome (infected / not infected)?
- Is the severity of cholera symptoms (Mild / Moderate / Severe) linked to the pump from which a victim drew water?
- Is the distribution of ABO blood types different across three ethnic groups?

For these questions — where both the exposure and the outcome are categorical — the appropriate test is the **Chi-Square Test for Independence** (χ², pronounced "ky-squared").

---

### 4.2 The Contingency Table

The starting point for a Chi-Square test is a **contingency table** (also called a cross-tabulation or crosstab): a grid in which each row represents one category of Variable 1, each column represents one category of Variable 2, and each cell contains the *count* of observations that fall into that combination of categories.

**Our question:** Is cholera symptom severity (Mild / Moderate / Severe) associated with the pump the victim used (Broad Street / Rupert Street / Marlborough Street)?

**Observed contingency table** (actual counts from the records):

| Pump Used | Mild | Moderate | Severe | Row Total |
|---|:---:|:---:|:---:|:---:|
| **Broad Street** | 12 | 28 | 40 | 80 |
| **Rupert Street** | 25 | 20 | 15 | 60 |
| **Marlborough St** | 18 | 16 | 16 | 50 |
| **Column Total** | 55 | 64 | 71 | **190** |

Notice the pattern: Broad Street victims appear to cluster towards Severe outcomes (40 severe cases out of 80 total = 50%), whilst Rupert Street victims cluster towards Mild outcomes (25 mild out of 60 total = 42%). The Chi-Square test asks whether this apparent pattern is large enough to be unlikely under the Null Hypothesis of independence.

---

### 4.3 Expected Counts — The Null Hypothesis Made Concrete

The **Null Hypothesis** for the Chi-Square test states that pump choice and symptom severity are completely independent — knowing which pump a victim used tells you nothing about the severity of their illness.

Under this null hypothesis, the count in each cell would be determined entirely by the marginal totals (the row and column totals), in proportion to the overall sample:

$$E_{ij} = \frac{\text{Row}_i \text{ Total} \times \text{Column}_j \text{ Total}}{\text{Grand Total}}$$

**Example calculation for the Broad Street / Mild cell:**

$$E = \frac{80 \times 55}{190} = \frac{4{,}400}{190} \approx 23.2$$

The observed count in this cell is 12. If pump choice and severity were truly independent, we would expect approximately 23 mild cases among Broad Street victims — but we observe only 12. This cell contributes a large term to the χ² statistic.

**Full expected counts table** (observed counts with expected counts in parentheses):

| Pump Used | Mild | Moderate | Severe |
|---|:---:|:---:|:---:|
| **Broad Street** | 12 (E: 23.2) | 28 (E: 27.0) | 40 (E: 29.9) |
| **Rupert Street** | 25 (E: 17.4) | 20 (E: 20.2) | 15 (E: 22.4) |
| **Marlborough St** | 18 (E: 14.5) | 16 (E: 16.8) | 16 (E: 18.7) |

> **Reading the expected counts:** Large discrepancies between observed and expected counts indicate cells where the two variables are behaving in a way that deviates from independence. The Broad Street / Mild cell (observed 12, expected 23.2) and the Broad Street / Severe cell (observed 40, expected 29.9) are the largest contributors to the χ² statistic in this table — precisely where Snow's data most strongly suggests an association.

---

### 4.4 The Chi-Square Statistic

The χ² statistic aggregates all the cell-level discrepancies into a single number:

$$\chi^2 = \sum_{\text{all cells}} \frac{(O - E)^2}{E}$$

Where O is the observed count and E is the expected count for each cell.

**Why divide by E?**

Dividing by the expected count makes the formula scale-invariant and fair across cells of different sizes. A difference of 10 between observed and expected counts means something very different in a cell where the expected count is 12 (a relative deviation of 83%) versus a cell where the expected count is 500 (a relative deviation of 2%). Dividing by E standardises the contribution of each cell to the same scale before summing.

**Degrees of freedom for Chi-Square:**

$$df = (\text{number of rows} - 1) \times (\text{number of columns} - 1)$$

For our 3 × 3 table: df = (3 − 1) × (3 − 1) = **4**.

The computed χ² is compared against the Chi-Square distribution with the appropriate degrees of freedom to obtain the p-value.

---

### 4.5 Assumptions and When to Use Fisher's Exact Test

The Chi-Square test relies on a large-sample approximation. Two assumptions must hold for the approximation to be valid:

**Assumption 1 — Independence of observations:**
Each individual appears in exactly one cell of the contingency table. No participant can contribute to more than one row-column combination. (This is a study design requirement, not a statistical check.)

**Assumption 2 — Minimum expected frequency:**
Every cell in the table must have an **expected count of at least 5**. When any expected count falls below 5, the χ² approximation becomes unreliable — the computed p-value is inaccurate.

> **What to do when expected counts are too low:**
>
> Use **Fisher's Exact Test** instead of the Chi-Square test. Fisher's Exact Test computes the exact probability of the observed table (and all tables more extreme) without relying on any large-sample approximation. It is valid for any sample size and any table dimensions, but is most commonly applied to 2 × 2 tables with small cell counts.
>
> In PSPP: tick the **Fisher** box in the Crosstabs → Statistics dialog.
> In R: use `fisher.test(your_table)`.

**Practical guidance on expected count violations:**
- If **one or two cells** in a larger table fall below 5, consider collapsing adjacent categories (e.g., merging "Moderate" and "Severe" into "Non-Mild") if this is scientifically defensible, then re-checking the expected counts.
- If **many cells** fall below 5, Fisher's Exact Test is the appropriate choice regardless of table size.
- Never report a Chi-Square p-value when expected counts are violated without acknowledging the violation and applying the correct remedy.

---

### 4.6 Interpreting the Chi-Square Result

A significant Chi-Square result (p < 0.05) means that the observed pattern of counts is significantly different from what we would expect if the two categorical variables were completely independent. It establishes **statistical association** — the two variables are linked in the population.

However, the Chi-Square test tells you nothing about:
- The **direction** of the association (which cell or cells are driving it)
- The **strength** of the association
- The **cause** of the association

To understand the direction and pattern of an association, always examine the **adjusted standardised residuals** in the PSPP output, or compute them in R. Cells with adjusted residuals greater than ±2.0 are the specific locations where observed counts deviate most significantly from expectation — the "hotspots" of the association.

To quantify the strength of a 2 × 2 association, compute **Cramér's V** or the **Phi coefficient**. For larger tables, Cramér's V is the standard measure. It ranges from 0 (no association) to 1 (perfect association):

| Cramér's V | Strength of association |
|:---:|---|
| 0.00 – 0.10 | Negligible |
| 0.10 – 0.30 | Small |
| 0.30 – 0.50 | Moderate |
| > 0.50 | Large |

> **Association ≠ Causation**
>
> A statistically significant Chi-Square result demonstrates that two categorical variables are associated in the data — it does not establish that one causes the other. Smokers have significantly higher rates of lung cancer (Chi-Square p < 0.001), but this association alone does not prove causation. Causation requires additional evidence: a plausible biological mechanism, a dose-response relationship, consistency across populations, and ideally, experimental evidence. The Chi-Square test is a tool for detecting associations; causal inference is a separate inferential step.

---

## 🔬 Lab Manual — Chapter 7

### Objective

**Part 1:** Run a one-way ANOVA to test whether mean travel distance to the pump differs across pump groups. Follow up a significant result with Tukey's HSD to identify which specific pairs differ.

**Part 2:** Compute a two-proportion z-test, risk difference, and relative risk comparing the attack rate between near-pump and far-pump households.

**Part 3:** Build a contingency table and run a Chi-Square test to assess whether symptom severity is associated with the pump used.

---

### Option A — PSPP

#### Part 1 — One-Way ANOVA

1. Open your `snow_1854_outbreak.sav` file.
2. Go to **Analyze → Compare Means → One-Way ANOVA**.
3. Move **`Distance_Metres`** into the **Dependent List**.
4. Move **`Pump_Used`** into the **Factor** box.
5. Click **Options**. Tick **Descriptive statistics** (to see means and SDs per group) and **Homogeneity of variance test** (Levene's Test for ANOVA). Click **Continue**.
6. Click **Post Hoc**. Select **Tukey** from the list. Click **Continue**.
7. Click **OK**.

**Reading the output:**

Find the **ANOVA** table. Locate the **Sig.** column in the row labelled *Between Groups*.
- If Sig. < 0.05: reject H₀ — at least one pump group has a significantly different mean distance. Proceed to the **Multiple Comparisons** table (Tukey's HSD output).
- If Sig. ≥ 0.05: fail to reject H₀ — no significant difference detected across pump groups. Do not interpret the post-hoc table.

In the **Multiple Comparisons** table, each row compares one pair of pumps. Read the **Sig.** column for each pair. A value below 0.05 indicates a statistically significant difference between that specific pair.

---

#### Part 2 — Two-Proportion Z-Test and Relative Risk

PSPP does not have a dedicated two-proportion z-test dialog. The calculation is best performed as follows:

1. Create a binary column `Near_Pump` in the Data Editor: enter 1 for households ≤ 100m from the Broad Street pump, 0 for households > 100m.
2. Create a binary column `Death_Any` (1 = at least one death in household, 0 = no death).
3. Go to **Analyze → Descriptive Statistics → Crosstabs**.
4. Move **`Near_Pump`** into **Row(s)** and **`Death_Any`** into **Column(s)**.
5. Click **Statistics**. Tick **Chi-square** and **Risk**. Click **Continue**.
6. Click **Cells**. Tick **Row percentages**. Click **Continue**. Click **OK**.

**Reading the output:**

- The *row percentages* in the crosstab give you p₁ (row 1 = near pump) and p₂ (row 2 = far pump).
- The **Risk Estimate** table produced by the **Risk** option gives the **Relative Risk** (labelled *Risk Ratio*) and its 95% CI directly.
- The **Chi-Square Tests** table gives the z-test equivalent (Pearson Chi-Square for a 2 × 2 table is mathematically equal to z²; take the square root to recover z).

---

#### Part 3 — Chi-Square Test

1. Go to **Analyze → Descriptive Statistics → Crosstabs**.
2. Move **`Pump_Used`** into the **Row(s)** box.
3. Move **`Severity`** into the **Column(s)** box.
4. Click **Statistics**. Tick **Chi-square** and — for effect size — **Phi and Cramér's V**. Click **Continue**.
5. Click **Cells**. Under *Expected*, tick **Expected** (to display expected counts in each cell). Click **Continue**.
6. Click **OK**.

**Reading the output:**

In the **Chi-Square Tests** table, read the row labelled *Pearson Chi-Square*. The **Asymp. Sig. (2-sided)** column gives the p-value.

> Check the footnote below the table. PSPP will flag any cells where the expected count is below 5 — for example, "X cells have expected count less than 5." If this is flagged, consider switching to Fisher's Exact Test (available in the Statistics dialog).

---

### Option B — R / RStudio

```r
# ---------------------------------------------------------
# Chapter 7 Lab: One-Way ANOVA and Chi-Square Test
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded.
# library(HistData)
# cholera_data <- Snow.deaths

# ── PART 1: ONE-WAY ANOVA ─────────────────────────────────

# Step 2: Summarise distance by pump group before testing.
tapply(cholera_data$Distance_Metres, cholera_data$Pump_Used, mean)
tapply(cholera_data$Distance_Metres, cholera_data$Pump_Used, sd)
tapply(cholera_data$Distance_Metres, cholera_data$Pump_Used, length)

# Step 3: Check the equal-variance assumption with Levene's Test.
# The 'car' package provides leveneTest().
# install.packages("car")  # run once if not already installed
library(car)
leveneTest(Distance_Metres ~ Pump_Used, data = cholera_data)
# p > 0.05: proceed with standard ANOVA.
# p < 0.05: consider Welch's ANOVA (oneway.test with var.equal = FALSE).

# Step 4: Fit the one-way ANOVA model.
# aov() fits the model; summary() displays the ANOVA table.
anova_model <- aov(Distance_Metres ~ Pump_Used, data = cholera_data)
summary(anova_model)

# Step 5: Read the output.
# Pr(>F) is the p-value. An asterisk (*) or (**) indicates significance.
# If p < 0.05, proceed to Step 6.

# Step 6: Run Tukey's HSD post-hoc test.
# TukeyHSD() compares every possible pair of groups
# whilst controlling the family-wise error rate at 5%.
TukeyHSD(anova_model)

# Each row shows one pairwise comparison:
# diff  = mean difference between the two groups
# lwr   = lower bound of the 95% CI for the difference
# upr   = upper bound of the 95% CI for the difference
# p adj = Tukey-adjusted p-value (compare to 0.05)
# A CI that does not include 0 indicates a significant difference.

# Step 7: Visualise the group means and variability.
boxplot(Distance_Metres ~ Pump_Used,
        data  = cholera_data,
        main  = "Distance to Pump by Pump Group",
        xlab  = "Pump Used",
        ylab  = "Distance (Metres)",
        col   = c("lightblue", "lightgreen", "lightyellow"),
        notch = FALSE)

# ── PART 2: TWO-PROPORTION Z-TEST AND RELATIVE RISK ──────

# Step 8: Create the binary exposure and outcome columns.
cholera_data$Near_Pump  <- ifelse(
  cholera_data$Distance_Metres <= 100, 1, 0)

cholera_data$Death_Any  <- ifelse(
  cholera_data$Household_Deaths > 0, 1, 0)

# Step 9: Build the 2x2 table.
prop_table <- table(Near_Pump = cholera_data$Near_Pump,
                    Death_Any  = cholera_data$Death_Any)
prop_table

# Step 10: Extract cell counts for hand calculation.
a <- prop_table[2, 2]   # near pump,  death
b <- prop_table[2, 1]   # near pump,  no death
c <- prop_table[1, 2]   # far from pump, death
d <- prop_table[1, 1]   # far from pump, no death

n1 <- a + b             # near pump total
n2 <- c + d             # far pump total

p1 <- a / n1            # attack rate, near pump
p2 <- c / n2            # attack rate, far pump

cat("=== Two-Proportion Comparison ===\n")
cat("Attack rate — near pump:", round(p1, 3), "\n")
cat("Attack rate — far pump: ", round(p2, 3), "\n")

# Step 11: Compute Risk Difference and Relative Risk.
RD <- p1 - p2
RR <- p1 / p2
cat("Risk Difference (RD):", round(RD, 3), "\n")
cat("Relative Risk (RR):  ", round(RR, 3), "\n")

# Step 12: Run the formal two-proportion z-test.
# prop.test() uses the chi-square approximation (z^2 = chi-square).
# correct = FALSE disables the continuity correction
# for a result equivalent to the standard z-test.
prop_result <- prop.test(
  x       = c(a, c),    # successes in each group
  n       = c(n1, n2),  # totals in each group
  correct = FALSE)
prop_result

# p-value: compare to 0.05
# 95% CI for RD: the conf.int element

# Step 13: Compute 95% CI for Risk Difference manually.
SE_RD <- sqrt((p1 * (1 - p1) / n1) + (p2 * (1 - p2) / n2))
CI_lo  <- RD - 1.96 * SE_RD
CI_hi  <- RD + 1.96 * SE_RD
cat("95% CI for RD: [", round(CI_lo, 3), ",", round(CI_hi, 3), "]\n")

# Step 14: Compute 95% CI for Relative Risk.
# Use log-transform method (standard in epidemiology).
log_RR <- log(RR)
SE_logRR <- sqrt((1 - p1) / (a) + (1 - p2) / (c))
RR_lo  <- exp(log_RR - 1.96 * SE_logRR)
RR_hi  <- exp(log_RR + 1.96 * SE_logRR)
cat("95% CI for RR: [", round(RR_lo, 3), ",", round(RR_hi, 3), "]\n")

# ── PART 3: CHI-SQUARE TEST ───────────────────────────────

# Step 15: Build the contingency table.
# table() cross-tabulates two categorical variables.
sev_table <- table(Pump    = cholera_data$Pump_Used,
                   Severity = cholera_data$Severity)
sev_table

# Step 16: Check expected counts before running the test.
# If any expected count is below 5, use Fisher's Exact Test instead.
chisq.test(sev_table)$expected
# Examine all values in the output. Every value should be >= 5.

# Step 17: Run the Chi-Square test.
chi_result <- chisq.test(sev_table)
chi_result

# Step 18: Read the output.
# X-squared: the chi-square statistic
# df: degrees of freedom = (rows - 1) * (columns - 1)
# p-value: compare to 0.05

# Step 19: Compute Cramér's V for effect size.
# install.packages("vcd")  # run once if not already installed
library(vcd)
assocstats(sev_table)
# Cramér's V: 0–0.10 negligible; 0.10–0.30 small;
#             0.30–0.50 moderate; >0.50 large

# Step 20: Inspect adjusted standardised residuals
# to identify which cells drive the association.
chi_result$stdres
# Values > +2 or < -2 indicate cells with significantly
# more or fewer observations than expected under H0.

# Step 21: If any expected count is below 5, use Fisher's Exact Test.
fisher.test(sev_table)
# Fisher's provides an exact p-value without large-sample assumptions.
# Note: Fisher's exact test for tables larger than 2x2 may be slow
# and requires the simulate.p.value = TRUE argument for large tables.
fisher.test(sev_table, simulate.p.value = TRUE, B = 10000)
```

**What to record and report:**

For the **ANOVA**:
- Report as: F(df_between, df_within) = [value], p = [value].
- State whether H₀ is rejected and identify which specific pump pairs differ from the Tukey's HSD output.

For the **two-proportion z-test and effect sizes**:
- Report the attack rate in each group (p₁ and p₂).
- Report RD with 95% CI: "The risk difference was [value] (95% CI [lower, upper])."
- Report RR with 95% CI: "Households near the pump were [RR] times as likely to suffer a death (95% CI [lower, upper])."
- Report the z-test p-value and state whether the difference is statistically significant.

For the **Chi-Square test**:
- Report as: χ²(df) = [value], p = [value], Cramér's V = [value].
- Describe the direction of the association by examining the cells with the largest deviations from expected counts.
- State whether Fisher's Exact Test was applied, and if so, why.

---

---

### 🧪 Test Your Knowledge

In a hypothetical extension of the Snow dataset, 3 neighbourhoods are compared on mean distance to the nearest safe water source: Soho (n=45, M=185m), Mayfair (n=38, M=312m), Holborn (n=41, M=247m). **Task:** (a) State why a one-way ANOVA is the correct test (not three separate t-tests). (b) Write the R code to run the ANOVA and Tukey's HSD. (c) Separately: in a 2×2 table, 52 of 80 exposed individuals developed illness vs. 14 of 60 unexposed. Calculate RD, RR, and state whether a Chi-Square or Fisher's Exact test is appropriate.

```{dropdown} Show Solution
```r
# (a) ANOVA is correct because running three t-tests (Soho vs Mayfair,
#     Soho vs Holborn, Mayfair vs Holborn) would inflate the FWER from
#     5% to 1-(1-0.05)^3 = 14.3%. ANOVA performs one protected omnibus test.

# (b) ANOVA and Tukey's HSD
# Assuming neighbourhood data is in cholera_data:
anova_model <- aov(Distance_Metres ~ Neighbourhood, data = cholera_data)
summary(anova_model)     # Read Pr(>F)
TukeyHSD(anova_model)    # Pairwise adjusted p-values

# (c) Proportions
p1 <- 52/80   # 0.650  (exposed attack rate)
p2 <- 14/60   # 0.233  (unexposed attack rate)
RD <- p1 - p2           # 0.417 (41.7 percentage points excess risk)
RR <- p1 / p2           # 2.79  (exposed nearly 3x more likely to fall ill)

# Check expected counts for Chi-Square
tab <- matrix(c(52, 28, 14, 46), nrow=2)
chisq.test(tab)$expected
# All expected counts >> 5 → Chi-Square is valid (not Fisher's Exact)
chisq.test(tab, correct = FALSE)
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Risk (proportion)** | The probability that an individual in a defined group will experience the outcome of interest. Computed as the count of outcomes divided by the group total (p = a / n₁). |
| **Risk Difference (RD)** | The absolute difference between two proportions (p₁ − p₂). Expresses the excess risk attributable to the exposure in percentage-point terms. |
| **Relative Risk (RR)** | The ratio of the outcome proportion in the exposed group to the outcome proportion in the unexposed group (p₁ / p₂). The primary effect-size measure in cohort studies and outbreak investigations. RR = 1 indicates no association; RR > 1 indicates increased risk in the exposed group. |
| **Two-proportion z-test** | A significance test comparing the proportions from two independent groups, testing H₀: p₁ = p₂. Uses the pooled proportion under H₀ to compute the standard error. |
| **Pooled proportion (p̂)** | The overall proportion across both groups combined, used as the best estimate of the common population proportion under H₀ in a two-proportion z-test. |
| **One-Way ANOVA** | Analysis of Variance — a test comparing the means of three or more independent groups on a continuous outcome, by decomposing total variance into between-group and within-group components. |
| **F-statistic** | The test statistic produced by ANOVA: the ratio of between-group variance to within-group variance. Values substantially greater than 1 indicate group separation. Reported as F(df_between, df_within). |
| **Between-group variance** | The variability of group means around the grand (overall) mean — the signal in an ANOVA. Large between-group variance indicates the groups are genuinely different. |
| **Within-group variance** | The variability of individual observations around their own group mean — the noise in an ANOVA. Reflects natural individual differences unrelated to group membership. |
| **Post-hoc test** | A follow-up analysis run after a significant ANOVA to identify specifically which pairs of groups differ significantly. Applied only when the omnibus ANOVA is significant. |
| **Tukey's HSD** | Tukey's Honestly Significant Difference — the most widely used post-hoc test for one-way ANOVA. Compares all possible group pairs whilst controlling the family-wise Type I error rate at α. |
| **Chi-Square test (χ²)** | A test for statistical independence between two categorical variables, comparing observed cell frequencies against expected frequencies derived under the Null Hypothesis of no association. |
| **Contingency table** | A grid displaying the joint frequency distribution of two categorical variables. The input for a Chi-Square test. Also called a cross-tabulation or crosstab. |
| **Expected count** | The cell frequency predicted by the Null Hypothesis of independence, calculated as (row total × column total) / grand total. |
| **Fisher's Exact Test** | An exact alternative to the Chi-Square test used when any expected cell count falls below 5. Computes the exact probability of the observed table without large-sample assumptions. |
| **Multiple comparisons problem** | The accumulation of Type I error risk when running many simultaneous hypothesis tests, quantified as 1 − (1 − α)^k for k independent tests at significance level α. |

---

## Review Questions

1. A researcher wants to compare the mean recovery time of patients assigned to one of four cholera treatment regimens: standard care, oral rehydration therapy alone, oral rehydration therapy plus antibiotics, and intravenous fluid therapy. Explain precisely why running all six possible pairwise t-tests is statistically invalid. Calculate the family-wise Type I error rate for six simultaneous tests at α = 0.05. What test should be used instead?

2. In a foodborne illness outbreak at a conference, 42 of 80 attendees who ate the buffet became ill, compared with 6 of 40 attendees who did not eat the buffet. (a) Compute p₁ and p₂. (b) Compute the Risk Difference and Relative Risk. (c) Run the two-proportion z-test by hand, showing all working. (d) Compute the 95% CI for the Risk Difference. (e) Write a two-sentence plain-language conclusion for a public health report.

3. A one-way ANOVA comparing systolic blood pressure across three dietary groups (low-sodium, Mediterranean, and standard diet) returns F(2, 147) = 8.3, p = 0.001. Write a correct two-sentence conclusion. Then state exactly what additional analysis must be performed, why the ANOVA result alone is insufficient, and what the additional analysis will reveal that the ANOVA cannot.

3. In the Tukey's HSD post-hoc output following an ANOVA across three hospital wards (A, B, C), the results are: Ward A vs. Ward B: p = 0.82; Ward A vs. Ward C: p = 0.003; Ward B vs. Ward C: p = 0.009. Write a plain-language summary of these three findings for a hospital manager who has no statistical training. State which ward appears to be the outlier and what the results suggest about where resources should be directed.

4. A public health analyst builds a 2 × 2 contingency table comparing vaccination status (vaccinated / unvaccinated) and disease outcome (infected / not infected). The four expected cell counts are: 3, 4, 22, and 18. Can they validly use a standard Chi-Square test? Justify your answer with reference to the assumption being checked. What should they do instead, and why is that alternative valid regardless of the expected counts?

5. The Chi-Square formula divides the squared difference (O − E)² by the expected count E before summing across cells. Explain in plain language why this division by E is necessary to make the formula fair, using a concrete example of two cells — one with a low expected count and one with a high expected count — where the same raw difference (O − E) has very different meaning.

6. A Chi-Square test comparing smoking status (never / former / current) against lung disease diagnosis (yes / no) returns χ²(2) = 19.4, p < 0.001, Cramér's V = 0.28. Write a complete conclusion: state the test statistic, degrees of freedom, p-value, and effect size in a single paragraph. Then explain why this result, despite being highly statistically significant, does not prove that smoking *causes* lung disease, and state what additional types of evidence would be needed to support a causal claim.

---

```{admonition} Key Takeaways
:class: tip

- **FWER:** 1 − (1 − α)^k — inflates rapidly. At k = 14 tests, α = 0.05: FWER ≈ 51%.
- **ANOVA F-statistic:** F = MS_Between / MS_Within. Large F → groups differ more than chance explains.
- **Tukey's HSD:** Run only after a significant ANOVA. Controls FWER whilst testing all pairwise comparisons.
- **Risk Difference:** RD = p₁ − p₂ (absolute excess risk in percentage points).
- **Relative Risk:** RR = p₁ / p₂. RR > 1 = increased risk; RR < 1 = protective; RR = 1 = no effect.
- **Chi-Square:** χ² = Σ(O−E)²/E. Tests independence between two categorical variables. Requires all expected counts ≥ 5.
- **Fisher's Exact Test:** Use when any expected count < 5. Always valid regardless of table size.
```

## Chapter Summary

Chapter 7 has extended the toolkit for comparing groups beyond the two-group constraint of the t-test, addressing one of the most consequential sources of statistical error in health sciences research — the multiple comparisons problem.

The one-way ANOVA solves the multiple comparisons problem for continuous outcomes by performing a single omnibus test that maintains the family-wise Type I error rate at exactly 5%, regardless of how many groups are compared. It achieves this by reframing the question about differences in means as a question about the ratio of between-group variance (signal) to within-group variance (noise). A significant F-statistic licences further investigation via Tukey's HSD, which identifies the specific group pairs responsible for the overall effect.

The Chi-Square test extends hypothesis testing to categorical outcomes, comparing the pattern of observed counts against the pattern expected under the Null Hypothesis of statistical independence. Its assumptions — independence of observations and expected cell counts of at least 5 — must be verified before the p-value is interpreted. When expected counts are too low, Fisher's Exact Test provides a valid alternative.

Both tests share a fundamental limitation: they establish statistical association, not causation. A significant ANOVA tells you that group membership explains some of the variance in the outcome; a significant Chi-Square tells you that two categorical variables are non-independently distributed. In both cases, the interpretation of *why* requires subject-matter knowledge, study design considerations, and — in the case of causal claims — a body of convergent evidence that no single statistical test can provide.

In Chapter 8, we move from comparing groups to describing *relationships* between continuous variables, using correlation and simple linear regression.

---

*Next: **Chapter 8 — Reading the Future:** Correlation and Simple Linear Regression*

---

> Part III — Complex Connections
