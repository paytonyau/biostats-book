# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 6: A Tale of Two Groups
## *Comparing Means with T-Tests*

> **Course:** HS 4510 Biostatistics | **Unit:** 6 | **Part:** II — Making Decisions Under Uncertainty
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Select the correct t-test design — independent or paired — from a study description
- Apply **Levene's Test** to check equal variances before an independent t-test
- Run and interpret both t-tests in PSPP and R, including the mean difference and 95% CI
- Explain why pairing increases statistical power and identify when pairing is appropriate
```


---

## Introduction: Comparing Two Groups

In Chapter 5, you learnt to compare a sample mean against a fixed, known population value — the one-sample t-test. You tested whether the average age of cholera victims differed from the baseline life expectancy of 40 years for a working-class Londoner.

That design — one sample, one reference value — answers an important class of questions. But the most common analytical problem in health sciences is different: you have **two groups**, both drawn from real patients, and you want to know whether they differ from each other.

Does a new oral rehydration therapy reduce seven-day cholera mortality more than the standard treatment? Do victims who drew water from the Broad Street pump show a different age profile than those who drew water from the Rupert Street pump? Do patients' blood pressure readings differ before and after a four-week course of antihypertensive medication?

Each of these questions compares two sets of measurements. The t-test remains the appropriate tool, but the design of the study — specifically, the relationship between the two groups — determines *which version* of the t-test to use. Choosing the wrong version produces invalid p-values and may lead to incorrect clinical conclusions.

---

## Section 1: Choosing the Right T-Test

### 1.1 The One Defining Question

Before selecting a statistical test, ask yourself one question about the two groups you wish to compare:

> **"Are the two groups made up of different people, or of the same people measured on two separate occasions?"**

The answer determines everything:

| The two groups are composed of... | Correct test |
|---|---|
| **Different people** — entirely separate, unrelated individuals | **Independent Samples t-test** |
| **The same people** — measured twice (before and after, or under two conditions) | **Paired Samples t-test** |

Using the Independent t-test when the data is paired ignores the within-subject correlation and artificially inflates the error term, reducing the power to detect a real effect. Using the Paired t-test when the data is independent violates the assumption of dependence, producing incorrect standard errors. Neither mistake is harmless.

---

### 1.2 Recognising Each Design in Practice

**Independent Samples designs:**
- A randomised controlled trial: 50 patients are randomly assigned to Drug A; a separate 50 patients are assigned to Drug B. The two groups contain different people.
- A case-control study: 40 cholera victims are compared to 40 healthy controls on a continuous risk factor. These are different individuals.
- A cohort comparison: patients from the Broad Street pump area are compared to patients from the Rupert Street pump area.

**Paired (dependent) Samples designs:**
- A pre-post intervention study: the same 30 patients have their blood pressure recorded before starting treatment and again after four weeks.
- A crossover trial: each patient receives Drug A for one month, then Drug B for one month. The same person generates both measurements.
- A matched-pairs design: each patient is individually matched to a control on age, sex, and disease severity. The match pair is analysed as a unit.
- A reliability study: the same biological sample is measured by two different laboratory technicians. Each sample generates one measurement per technician.

---

## Section 2: The Independent Samples T-Test

### 2.1 What It Measures

The **Independent Samples t-test** compares the means of two entirely separate, unrelated groups. It is the statistical backbone of the Randomised Controlled Trial — one group receives the intervention; a completely separate group receives the control or placebo. The two groups share no participants and no data.

**Our question for the Snow dataset:**

Were the cholera victims who drew water from the Broad Street pump significantly different in age from those who drew water from the Rupert Street pump?

- **H₀:** There is no true difference in mean age between the Broad Street group and the Rupert Street group. Any observed difference is due to random sampling variation.
- **Hₐ:** There is a statistically significant difference in mean age between the two pump groups.

---

### 2.2 The T-Statistic for Two Independent Groups

The t-statistic for the Independent Samples t-test is a ratio. The **numerator** captures the signal — how different the two group means are from each other. The **denominator** captures the noise — how much natural variability exists within each group.

$$t = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\dfrac{s_1^2}{n_1} + \dfrac{s_2^2}{n_2}}}$$

Where:
- **x̄₁** and **x̄₂** are the sample means of Group 1 and Group 2
- **s₁²** and **s₂²** are the sample variances of each group
- **n₁** and **n₂** are the sample sizes of each group
- The denominator is the **pooled standard error** of the difference between means

**What makes the t-statistic large?**

The t-statistic grows when:
1. The **difference between group means is large** (large numerator). If Broad Street victims are on average 15 years younger than Rupert Street victims, the numerator is large.
2. The **within-group variability is small** (small denominator). If every Broad Street victim is aged between 20 and 30, the variance for that group is low and the denominator shrinks.

A large t-statistic, whether positive or negative, yields a small p-value — and a small p-value gives us grounds to reject H₀.

**What makes the t-statistic small?**

The t-statistic shrinks when:
1. The group means are similar to one another (small numerator).
2. The within-group data is highly variable (large denominator) — individual ages scatter widely within each pump group, making it hard to distinguish signal from noise.

---

### 2.3 Degrees of Freedom for the Independent T-Test

For the Independent t-test, the degrees of freedom are calculated as:

$$df = n_1 + n_2 - 2$$

This formula applies when the two groups have equal variances. The software calculates this automatically, but understanding it reinforces the principle: each group contributes n − 1 degrees of freedom (one is spent estimating each group mean), giving a total of n₁ + n₂ − 2.

When variances are unequal, the Welch correction (Section 2.4) adjusts the degrees of freedom downward, which slightly increases the p-value — making the test more conservative.

---

### 2.4 The Critical Assumption — Equal Variances, and Levene's Test

The standard Independent t-test formula in Section 2.2 assumes that the two groups have roughly equal variance — that the spread of ages within the Broad Street group is similar to the spread within the Rupert Street group. If one group's ages span from 2 to 82, whilst the other's span from 25 to 35, the two groups are not equally "messy," and the standard pooled formula is mathematically unfair to the more variable group.

To check this assumption formally, PSPP and R automatically run **Levene's Test for Equality of Variances** alongside the t-test.

**Hypotheses for Levene's Test:**
- **H₀:** The two groups have equal variances (σ₁² = σ₂²)
- **Hₐ:** The two groups have unequal variances (σ₁² ≠ σ₂²)

**Reading the Levene's Test output:**

| Levene's p-value | Conclusion | Which t-test row to read |
|---|---|---|
| **p > 0.05** | Fail to reject H₀ — variances are approximately equal. The standard assumption holds. | **"Equal variances assumed"** — top row of the PSPP output |
| **p < 0.05** | Reject H₀ — variances are significantly unequal. The standard formula is not appropriate. | **"Equal variances not assumed"** — bottom row (Welch's correction applied) |

> **The Welch correction** adjusts both the denominator of the t-statistic and the degrees of freedom to account for unequal variances. It produces a slightly different (and more conservative) p-value. When in doubt, it is always safe to report the Welch-corrected result — it performs well under both equal and unequal variances, making it the more robust default choice.

> **Important:** Levene's Test is a *prerequisite check*, not the main test. You interpret Levene's p-value only to decide which row of the t-test output to read. You do not report Levene's result as your primary finding.

---

### 2.5 Assumptions of the Independent Samples T-Test

Before accepting the results of an Independent t-test, verify all four assumptions:

| Assumption | What it requires | How to check it |
|---|---|---|
| **Independence of observations** | Each participant contributes one data point. No individual appears in both groups. | Study design review — was randomisation or group assignment carried out correctly? |
| **Continuous outcome variable** | The variable being compared (e.g., Age) is interval or ratio level. | Level of measurement classification (Chapter 1). |
| **Approximate Normality within each group** | The outcome variable is approximately Normally distributed within each group. | Histogram and Shapiro-Wilk test for each group separately (Chapter 4). Robust to modest violations when n > 30 per group. |
| **Homogeneity of variance** | The two groups have roughly equal variance. | Levene's Test (Section 2.4). |

---

## Section 3: The Paired Samples T-Test

### 3.1 The Core Idea — Difference Scores

The **Paired Samples t-test** is used when the same individuals contribute measurements under both conditions — before and after an intervention, or under two different experimental conditions. The two measurement columns are not independent; they are linked by the person who generated them.

Rather than comparing the mean of Column 1 against the mean of Column 2 directly, the Paired t-test works as follows:

**Step 1:** For each participant, compute the **difference score**: d = measurement₂ − measurement₁ (e.g., post-treatment blood pressure minus pre-treatment blood pressure).

**Step 2:** Compute the mean of all difference scores: d̄.

**Step 3:** Test whether d̄ is significantly different from zero — that is, whether the average change is large enough to be unlikely under H₀ (which states that the true mean difference is zero: μ_d = 0).

The Paired t-test is therefore, in essence, a **one-sample t-test applied to the column of difference scores** — exactly the test you learnt in Chapter 5, but applied to within-person changes rather than raw measurements.

$$t = \frac{\bar{d}}{s_d / \sqrt{n}}$$

Where:
- **d̄** is the mean of the difference scores
- **s_d** is the standard deviation of the difference scores
- **n** is the number of *pairs* (not the total number of measurements)

---

### 3.2 A Clinical Example

A trial recruits 25 patients with Stage 1 hypertension. Systolic blood pressure is measured on Day 0 (before starting medication) and again on Day 28 (after four weeks of treatment).

| Patient | Day 0 (mmHg) | Day 28 (mmHg) | Difference (d = Day 28 − Day 0) |
|:---:|:---:|:---:|:---:|
| Patient 01 | 158 | 142 | −16 |
| Patient 02 | 145 | 138 | −7 |
| Patient 03 | 162 | 155 | −7 |
| Patient 04 | 149 | 151 | +2 |
| Patient 05 | 171 | 149 | −22 |
| ... | ... | ... | ... |

The Paired t-test analyses the **Difference** column. It asks: "Is the mean of these difference scores significantly different from zero?" A mean difference of, say, d̄ = −11.4 mmHg with a small standard deviation and p < 0.05 would indicate that the treatment produced a statistically significant reduction in systolic blood pressure.

Notice that Patient 04 shows a slight *increase* of +2 mmHg. The Paired t-test accommodates this — it tests the average direction of change across all patients, not whether every individual improved.

---

### 3.3 Why Pairing Increases Statistical Power

This is the most important conceptual point in Chapter 6, and it is frequently overlooked by students who encounter it for the first time.

**The problem with the Independent t-test in pre-post designs:**

Imagine two patients. Patient A has a resting systolic blood pressure of 165 mmHg; Patient B has a resting blood pressure of 148 mmHg. This 17 mmHg difference between patients is nothing to do with the treatment — it simply reflects the natural biological variability between two different human beings. In an Independent t-test, this between-person variability enters the denominator as noise, inflating the standard error and reducing the t-statistic.

**How pairing removes this noise:**

When the same patient provides both the pre-treatment and post-treatment measurement, the natural between-person variability cancels out. We no longer care that Patient A started at 165 mmHg and Patient B started at 148 mmHg. We care only about how many mmHg *each patient individually changed*. The between-person variability is subtracted away when we compute d = post − pre.

**The statistical consequence:**

The standard deviation of the difference scores (s_d) is typically much smaller than the standard deviation of the raw measurements in either group separately. A smaller denominator in the t-formula produces a larger t-statistic, which produces a smaller p-value — meaning the Paired t-test can detect the same real effect with a smaller sample size than the Independent t-test would require.

> **Practical implication:** If you have the option of using a paired design (e.g., measuring the same patients before and after an intervention) rather than an independent design (e.g., comparing two separate groups), the paired design will almost always be more statistically efficient. This means you can achieve the same power with fewer participants — reducing cost, time, and the number of patients exposed to experimental interventions.

---

### 3.4 Assumptions of the Paired Samples T-Test

| Assumption | What it requires | How to check it |
|---|---|---|
| **Paired observations** | Each pair must consist of measurements from the same individual (or from a matched pair). | Study design review. |
| **Continuous outcome** | The difference scores are continuous (interval or ratio). | Level of measurement classification. |
| **Approximate Normality of difference scores** | The *difference scores* (not the raw measurements) should be approximately Normally distributed. | Histogram and Shapiro-Wilk test on the difference column. Robust to moderate violations when n > 30. |

Note that the Paired t-test does **not** require equal variances between the two measurement occasions — because it analyses the single column of differences, the concept of "two group variances" does not apply.

---

### 3.5 Side-by-Side Comparison

| Feature | Independent Samples T-Test | Paired Samples T-Test |
|---|---|---|
| **Study design** | Two separate, unrelated groups | The same participants measured twice |
| **What is compared** | The two group means directly | The mean of within-participant difference scores |
| **Primary source of variation** | Between-person differences + random measurement error | Random measurement error only (between-person variation removed) |
| **Degrees of freedom** | n₁ + n₂ − 2 (approximately) | n − 1, where n = number of pairs |
| **Power relative to same n** | Lower — between-person noise inflates the denominator | Higher — between-person noise cancelled by differencing |
| **Requires Levene's Test** | Yes — to choose between standard and Welch versions | No — only one variance (that of the difference scores) |
| **Typical use cases** | RCTs, case-control studies, cross-sectional comparisons | Pre-post treatment trials, crossover trials, matched pairs |
| **R syntax** | `t.test(outcome ~ group, data = df)` | `t.test(before, after, paired = TRUE)` |

---

## 🔬 Lab Manual — Chapter 6

### Objective

Run an Independent Samples t-test to compare the mean age of cholera victims from the Broad Street pump group against those from the Rupert Street pump group. Interpret Levene's Test output to select the correct row of results. Then review the R syntax for a Paired t-test so that you are prepared to use it in a pre-post intervention design.

**Hypotheses:**
- H₀: There is no difference in mean age between the Broad Street and Rupert Street pump groups (μ₁ = μ₂).
- Hₐ: There is a statistically significant difference in mean age between the two pump groups (μ₁ ≠ μ₂).

---

### Option A — PSPP

**Step-by-step instructions:**

1. Open your `snow_1854_outbreak.sav` file.
2. Go to **Analyze → Compare Means → Independent-Samples T Test**.
3. Move **`Age`** into the **Test Variable(s)** box at the top.
4. Move **`Pump_Used`** into the **Grouping Variable** box at the bottom. Notice that two question marks appear: (`? ?`). PSPP needs to know the labels of the two groups you are comparing.
5. Click **Define Groups**. In **Group 1**, type exactly `Broad St` (matching the label in your data). In **Group 2**, type `Rupert St`. Click **Continue**.
6. Click **OK**.

**Reading the output — a two-step process:**

**Step 1 — Levene's Test:**

Find the table labelled *Independent Samples Test*. The leftmost columns show Levene's Test for Equality of Variances. Locate the **Sig.** column for Levene's Test.

| Levene's Sig. | Interpretation | Which row to read for the t-test |
|:---:|---|---|
| > 0.05 | Variances are equal | Top row: "Equal variances assumed" |
| < 0.05 | Variances are unequal | Bottom row: "Equal variances not assumed" |

**Step 2 — T-test results:**

From the correct row (determined above), record:
- **t** — the t-statistic
- **df** — degrees of freedom
- **Sig. (2-tailed)** — your p-value. Compare to α = 0.05.
- **Mean Difference** — x̄₁ − x̄₂. Note the sign: positive means Group 1 is older; negative means Group 1 is younger.
- **95% CI of the Difference** — if this interval does not include 0, the difference is statistically significant.

> **Checking means:** Before interpreting the p-value, find the *Group Statistics* table at the top of the output. It shows the mean age for each pump group separately. This tells you not only whether the difference is significant, but which group was older on average — information the p-value alone does not provide.

---

### Option B — R / RStudio

```r
# ---------------------------------------------------------
# Chapter 6 Lab: Independent and Paired Samples T-Tests
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded.
# library(HistData)
# cholera_data <- Snow.deaths

# ── PART 1: INDEPENDENT SAMPLES T-TEST ───────────────────

# Step 2: Examine the two groups before testing.
# tapply() applies a function to a variable, split by a grouping factor.
# This shows us the mean age within each pump group.
tapply(cholera_data$Age, cholera_data$Pump_Used, mean)
tapply(cholera_data$Age, cholera_data$Pump_Used, sd)
tapply(cholera_data$Age, cholera_data$Pump_Used, length)  # group sizes

# Step 3: Check Normality within each group separately.
# The t-test assumes approximate Normality within each group.
by(cholera_data$Age, cholera_data$Pump_Used, shapiro.test)

# Step 4: Run the Independent Samples t-test.
# The formula syntax Age ~ Pump_Used means:
# "Analyse Age as a function of (i.e., separated by) Pump_Used."
# var.equal = FALSE applies Welch's correction (safer default).
result_ind <- t.test(Age ~ Pump_Used,
                     data      = cholera_data,
                     var.equal = FALSE)
result_ind

# Step 5: If you want to test with equal variances assumed
# (equivalent to reading the top row in PSPP), use var.equal = TRUE.
# Compare the p-values — they should be similar when variances are equal.
result_ind_eq <- t.test(Age ~ Pump_Used,
                        data      = cholera_data,
                        var.equal = TRUE)
result_ind_eq

# Step 6: Extract and display the key results clearly.
cat("=== Independent Samples T-Test Results ===\n")
cat("Group means:\n")
print(tapply(cholera_data$Age, cholera_data$Pump_Used, mean))
cat("\nt-statistic:", round(result_ind$statistic, 3), "\n")
cat("Degrees of freedom:", round(result_ind$parameter, 1), "\n")
cat("p-value:", round(result_ind$p.value, 4), "\n")
cat("95% CI for mean difference: [",
    round(result_ind$conf.int[1], 2), ",",
    round(result_ind$conf.int[2], 2), "]\n")

# ── PART 2: PAIRED SAMPLES T-TEST (DEMONSTRATION) ────────

# Step 7: The paired t-test is demonstrated here using simulated
# pre- and post-treatment blood pressure data.
# In a real study, these would be two columns in your dataset.

# Simulate pre-treatment and post-treatment systolic blood pressure
# for 25 hypothetical patients.
set.seed(42)  # ensures reproducibility
pre_treatment  <- round(rnorm(25, mean = 155, sd = 12))
post_treatment <- pre_treatment - round(rnorm(25, mean = 10, sd = 6))

# Step 8: Examine the difference scores.
difference_scores <- post_treatment - pre_treatment
cat("\n=== Paired T-Test: Blood Pressure Example ===\n")
cat("Mean pre-treatment BP:", mean(pre_treatment), "mmHg\n")
cat("Mean post-treatment BP:", mean(post_treatment), "mmHg\n")
cat("Mean difference (post - pre):", round(mean(difference_scores), 2), "mmHg\n")
cat("SD of differences:", round(sd(difference_scores), 2), "mmHg\n")

# Step 9: Check Normality of the difference scores.
# This is the key assumption for the Paired t-test.
shapiro.test(difference_scores)
hist(difference_scores,
     main = "Distribution of Difference Scores (Post - Pre)",
     xlab = "Change in Systolic BP (mmHg)",
     col  = "lightgreen",
     border = "white")
abline(v = 0, col = "red", lwd = 2, lty = 2)  # zero-change reference line

# Step 10: Run the Paired t-test.
# paired = TRUE tells R that the two vectors are matched by participant.
result_paired <- t.test(post_treatment, pre_treatment, paired = TRUE)
result_paired

# Step 11: Compare to an Independent t-test on the same data.
# This demonstrates the power advantage of the paired design.
result_independent_same_data <- t.test(post_treatment, pre_treatment,
                                       paired = FALSE)

cat("\n=== Comparing Paired vs. Independent on the Same Data ===\n")
cat("Paired t-test p-value:     ", round(result_paired$p.value, 4), "\n")
cat("Independent t-test p-value:", round(result_independent_same_data$p.value, 4), "\n")
cat("Observe: the paired p-value is smaller,",
    "demonstrating higher power through the removal of between-person variability.\n")
```

**What to record and interpret:**

For the **Independent t-test**:
- Which pump group had the higher mean age? Does this difference reach statistical significance at α = 0.05?
- Does the 95% CI for the mean difference include zero? Is this consistent with the p-value decision?

For the **Paired t-test demonstration**:
- What is the direction of the mean difference? Does it indicate that blood pressure fell after treatment?
- Compare the p-values from the paired and independent versions of the same test. The difference illustrates precisely why pairing matters when the design permits it.

---

---

### 🧪 Test Your Knowledge

A researcher wants to test whether the **mean age** of victims who used the **Broad Street pump** differs significantly from those who used **Rupert Street pump**. **Task:** (a) State which t-test design is appropriate and why. (b) Write the R code, including the Levene's Test check. (c) Write a complete two-sentence conclusion reporting t, df, p-value, mean difference, and 95% CI.

```{dropdown} Show Solution
```r
# (a) Design: Independent Samples t-test.
# Rationale: The two groups (Broad St victims, Rupert St victims) are
# separate, unrelated individuals — not the same people measured twice.

# (b) R code
library(car)
two_pumps <- subset(cholera_data, 
                    Pump_Used %in% c("Broad St", "Rupert St"))
two_pumps$Pump_Used <- droplevels(as.factor(two_pumps$Pump_Used))

# Step 1: Levene's Test for equal variances
leveneTest(Age ~ Pump_Used, data = two_pumps)
# p > 0.05 → var.equal = TRUE; p < 0.05 → var.equal = FALSE

# Step 2: t-test (using Welch's as the safe default)
result <- t.test(Age ~ Pump_Used, data = two_pumps, var.equal = FALSE)
result

# (c) Conclusion template:
# "An independent-samples t-test found [a significant / no significant]
#  difference in mean age between Broad Street victims (M = [x̄₁]) and
#  Rupert Street victims (M = [x̄₂]), t([df]) = [t], p = [p], 
#  mean difference = [diff] years (95% CI [lower, upper])." 
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Independent Samples t-test** | A hypothesis test comparing the means of two entirely separate, unrelated groups of participants. Requires independence of observations between groups. |
| **Paired Samples t-test** | A hypothesis test comparing two measurements from the same participants (or matched pairs). Analyses the mean of within-participant difference scores. Equivalent to a one-sample t-test on the difference column. |
| **t-statistic** | The ratio of the between-group mean difference to the within-group standard error. A larger absolute value corresponds to a smaller p-value. |
| **Levene's Test** | A preliminary test for equality of variances between two groups in an Independent t-test. A p-value above 0.05 supports the equal-variances assumption; below 0.05 triggers Welch's correction. |
| **Equal variances assumed** | The standard row of the Independent t-test output in PSPP, used when Levene's Test p > 0.05. Applies the pooled-variance formula. |
| **Equal variances not assumed (Welch's correction)** | The adjusted row of the Independent t-test output, used when Levene's Test p < 0.05. Applies a modified t-formula and reduced degrees of freedom to account for unequal variances. |
| **Difference score** | In a paired design, the arithmetic difference between a participant's two measurements (e.g., post-treatment value minus pre-treatment value). The Paired t-test analyses the distribution of these difference scores. |
| **Degrees of freedom (df)** | A parameter that determines the shape of the t-distribution. For an Independent t-test: approximately n₁ + n₂ − 2. For a Paired t-test: n − 1, where n is the number of pairs. |
| **Randomised Controlled Trial (RCT)** | A study design in which participants are randomly assigned to a treatment group or a control group. The gold standard for evaluating clinical interventions and the primary context for the Independent t-test. |
| **Between-person variability** | Natural differences in the outcome variable that exist between different individuals regardless of treatment. Removed by the Paired t-test; contributes noise to the Independent t-test. |

---

## Review Questions

1. A physiotherapist measures the grip strength of 30 patients before and after a six-week hand rehabilitation programme. She wishes to determine whether the programme improved grip strength significantly.
   - (a) Should she use an Independent Samples or Paired Samples t-test? Justify your answer.
   - (b) What is the null hypothesis for this test?
   - (c) What assumption about the data must she verify before proceeding, and how should she check it?

2. You run an Independent Samples t-test in PSPP comparing the mean age of Broad Street pump victims against Rupert Street pump victims. The Levene's Test output shows F = 5.21, Sig. = 0.011. Which row of the t-test output table do you read? Explain precisely why the other row would be inappropriate.

3. Two groups are recruited randomly from a hospital ward: 25 patients receive Drug A and 25 patients receive Drug B for two weeks. The Independent t-test returns t(48) = 2.41, p = 0.019, 95% CI for mean difference [0.8, 9.4] days. Write a plain-language conclusion suitable for a clinical team briefing. Your answer should state the direction of the difference, the statistical significance, and what the confidence interval adds to the interpretation.

4. A researcher conducts an Independent t-test comparing blood cholesterol between two dietary groups and obtains p = 0.06. A colleague argues: "The result is not significant, so there is no difference between the diets." Write a more careful and statistically precise interpretation of this finding, with reference to what a p-value of 0.06 actually tells us.

5. Explain, using the concept of between-person variability, why a Paired Samples t-test achieves greater statistical power than an Independent Samples t-test when both tests are applied to data from the same 30 participants. Your answer should refer to what happens to the denominator of the t-formula in each case.

6. You run the following R code:
   ```r
   t.test(Recovery_Days ~ Treatment, data = trial_data)
   ```
   The output shows p = 0.002. What additional piece of information in the R output tells you *which* treatment group had the shorter average recovery time? Write the specific part of the output you would examine, and explain how to interpret it.

---

```{admonition} Key Takeaways
:class: tip

- **Independent t-test:** Two separate, unrelated groups. Formula: t = (x̄₁ − x̄₂) / SE_diff.
- **Paired t-test:** Same individuals measured twice, or matched cases and controls. Analyses the *difference scores* d = x₁ − x₂.
- **Levene's Test:** p > 0.05 → equal variances assumed (use Student's t). p < 0.05 → use Welch's correction (safer default).
- **Welch's df** is non-integer (Satterthwaite approximation) — this is normal and expected in software output.
- **Why pair?** Removes between-person variability from the error term, increasing power.
- **In R:** `t.test(y ~ group, var.equal = FALSE)` for independent. `t.test(before, after, paired = TRUE)` for paired.
```

## Chapter Summary

Chapter 6 extends hypothesis testing from the comparison of a single sample against a fixed reference value to the comparison of two groups — the most common analytical structure in clinical and public health research.

The single most important decision is identifying the correct test for the study design. If two groups contain different people, the Independent Samples t-test is appropriate. If the same people are measured twice, the Paired Samples t-test is required. Applying the wrong test to the wrong design is not a minor stylistic error — it invalidates the p-value and can lead to false conclusions.

The Independent t-test requires a preliminary check via Levene's Test. When variances are unequal, Welch's correction adjusts the degrees of freedom and produces a more conservative, reliable p-value. The correction is applied automatically by both PSPP and R, but the analyst must know to look for Levene's result and select the appropriate output row.

The Paired t-test works by converting a two-column comparison into a one-sample problem — testing whether the mean of the within-person difference scores differs significantly from zero. By removing between-person variability from the error term, the Paired design achieves higher statistical power with the same number of participants — a meaningful practical advantage in studies where recruitment is difficult or expensive.

In Chapter 7, we extend the comparison of means beyond two groups to three or more, using Analysis of Variance (ANOVA) — and we introduce the Chi-Square test for categorical outcomes.

---

*Next: **Chapter 7 — Scaling Up:** ANOVA and Chi-Square Tests*

---

> Part II — Making Decisions Under Uncertainty
