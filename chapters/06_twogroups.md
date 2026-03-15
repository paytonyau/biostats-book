# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 6: A Tale of Two Groups
## *Independent and Paired Samples T-Tests*

> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Determine whether a two-group comparison requires an **independent** or **paired** t-test
- State and check the assumptions of each test
- Apply Levene's test and Welch's correction
- Run and interpret both tests in PSPP and R
```

## Before You Begin: The Research Design Question

Chapter 5 compared one group to a known value. Most health research compares **two groups**. Before running any test, one design question determines everything: **are the two groups independent, or related (paired)?**

## Section 1: Independent vs Paired Designs

### 1.1 The Core Distinction

**Independent design:** Two completely different groups of unrelated individuals.

*Framingham example:* Do smokers and non-smokers have different mean systolic blood pressure? Each participant appears in exactly one group.

**Paired design:** The same individuals measured twice, or matched pairs.

*Clinical example:* 30 hypertensive patients have SYSBP measured before and after 8 weeks on a new antihypertensive. The same 30 patients contribute both measurements.

**The decision rule:** Same individual (or matched pair) contributes to both groups → **Paired**. Completely unrelated individuals → **Independent**.

> ⚡ **Common mistake:** Pre/post measurements on the same patients look like two groups but are paired. Running an independent t-test on pre/post data discards the pairing, loses power, and can produce the wrong answer.

```{figure} ../images/ch06_paired_vs_independent.png
:name: fig-paired
:width: 85%
:align: center

**Figure 6.1** Independent vs paired design. In a paired design, the unit of analysis is the *difference score* for each individual — this eliminates between-person variability and increases statistical power.
```

### 1.2 Why Pairing Increases Power

When each participant serves as their own control, all the natural between-person variation in BP (due to age, genetics, fitness, diet) is removed from the analysis. We are asking: did the *change* in BP differ from zero? Because between-person variability is usually much larger than the treatment effect, pairing can dramatically increase power.

## Section 2: The Independent Samples T-Test

### 2.1 Research Question

**Framingham:** Do smokers and non-smokers have different mean systolic blood pressure at baseline?

- $H_0: \mu_{smoker} = \mu_{non-smoker}$
- $H_1: \mu_{smoker} \neq \mu_{non-smoker}$

### 2.2 The Formula

$$t = \frac{\bar{x}_1 - \bar{x}_2}{SE_{diff}} \qquad SE_{diff} = \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$

### 2.3 Levene's Test and Welch's Correction

Before the independent t-test, check whether the two groups have similar variances.

- **Levene's p > 0.05:** Assume equal variances → standard pooled-variance t-test.
- **Levene's p ≤ 0.05:** Variances differ → use **Welch's t-test** (adjusts degrees of freedom).

R's `t.test()` applies Welch's correction by default — the safer option in all cases.

### 2.4 Assumptions

| Assumption | How to check |
|---|---|
| Independence of groups | Design-level — different, unrelated individuals |
| Continuous ratio/interval outcome | Chapter 1 variable classification |
| Approximately Normal within each group | Histogram + Shapiro-Wilk per group; robust when n ≥ 30 per group |
| Approximately equal variances | Levene's test; Welch's correction if violated |

## Section 3: The Paired Samples T-Test

> 💡 **Plain English first:** A paired t-test is a one-sample t-test on the *differences*. For each person, calculate (after − before). Then test whether those difference scores average to zero.

> ⚡ **Common mistake:** Pre/post measurements are paired — not independent. Using an independent t-test on pre/post data discards the pairing and can give the wrong answer.

### 3.1 Research Question

The Framingham dataset does not contain pre/post measurements. To demonstrate the paired t-test, we use a clinically realistic example:

30 Framingham participants identified as hypertensive are enrolled in a 12-week lifestyle intervention. SYSBP is measured before and after.

- $H_0: \mu_{diff} = 0$ (mean BP change = 0 — no effect of intervention)
- $H_1: \mu_{diff} \neq 0$

### 3.2 The Formula

For each participant, compute $d_i = \text{after}_i - \text{before}_i$.

$$t = \frac{\bar{d}}{s_d / \sqrt{n}}$$

Where $\bar{d}$ = mean of difference scores, $s_d$ = SD of difference scores, $n$ = number of pairs.

### 3.3 Side-by-Side Comparison

| Feature | Independent | Paired |
|---|---|---|
| Groups | Different individuals | Same individuals twice |
| Unit of analysis | Individual observations | Difference scores |
| Formula | $t = \frac{\bar{x}_1 - \bar{x}_2}{SE_{diff}}$ | $t = \frac{\bar{d}}{s_d / \sqrt{n}}$ |
| df | $\approx n_1+n_2-2$ | $n-1$ (pairs) |
| Power | Lower | Higher (removes between-person variability) |

## 🔬 Lab Manual — Chapter 6

### Objective
Part 1: Independent t-test — does mean SYSBP differ between smokers and non-smokers?
Part 2: Paired t-test — does a lifestyle intervention reduce SYSBP?

### Option A — PSPP

**Independent t-test:**
1. **Analyze → Compare Means → Independent-Samples T Test**.
2. Move `SYSBP` to Test Variables; `CURSMOKE` to Grouping Variable.
3. **Define Groups:** Group 1 = 0, Group 2 = 1. **Continue → OK**.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 6 Lab: Independent and Paired T-Tests
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")
fram_data$CURSMOKE <- factor(fram_data$CURSMOKE, levels=c(0,1),
  labels=c("Non-smoker","Smoker"))

# ══ Part 1: Independent t-test ═══════════════════════

# Descriptives by smoking group
tapply(fram_data$SYSBP, fram_data$CURSMOKE, mean)
tapply(fram_data$SYSBP, fram_data$CURSMOKE, sd)
tapply(fram_data$SYSBP, fram_data$CURSMOKE, length)

# Normality check per group
by(fram_data$SYSBP, fram_data$CURSMOKE, shapiro.test)

# Levene's test
library(car)
leveneTest(SYSBP ~ CURSMOKE, data = fram_data)

# Independent t-test (Welch by default)
t.test(SYSBP ~ CURSMOKE, data = fram_data)

# Effect size: Cohen's d
m1 <- mean(fram_data$SYSBP[fram_data$CURSMOKE=="Smoker"],   na.rm=TRUE)
m2 <- mean(fram_data$SYSBP[fram_data$CURSMOKE=="Non-smoker"],na.rm=TRUE)
s_pool <- sd(fram_data$SYSBP, na.rm=TRUE)
d <- (m1 - m2) / s_pool
cat("Cohen's d =", round(d, 3), "\n")

# Visualise
boxplot(SYSBP ~ CURSMOKE, data = fram_data,
        main = "Systolic BP by Smoking Status",
        xlab = "Smoking Status", ylab = "SYSBP (mmHg)",
        col = c("#BDD7EE","#FEE0D2"))

# ══ Part 2: Paired t-test ════════════════════════════

# Hypothetical pre/post BP data — 30 hypertensive participants
set.seed(42)
before <- c(158,162,155,170,148,165,172,160,153,168,
            157,163,151,175,161,159,167,154,171,156,
            164,169,152,173,158,161,166,155,170,163)
after  <- before - rnorm(30, mean=8, sd=5)   # intervention reduces BP by ~8 mmHg
after  <- round(after)

difference_scores <- after - before

# Check Normality of DIFFERENCES (not raw values)
shapiro.test(difference_scores)
hist(difference_scores,
     main = "BP Change (After − Before)",
     xlab = "Change in SYSBP (mmHg)",
     col = "#C7E9C0", border = "white")

# Paired t-test
t.test(after, before, paired = TRUE)
```

**What to examine:**
- **Independent t-test:** Is mean SYSBP higher in smokers than non-smokers? Is the difference statistically significant? Is Cohen's d > 0.2 (small), > 0.5 (medium)?
- **Levene's test:** If p < 0.05, Welch's correction (default in R) is appropriate.
- **Paired t-test:** Is the mean BP reduction after the lifestyle intervention significantly different from zero? The paired design eliminates between-person variability — the test focuses entirely on whether *individuals* changed.

### 🧪 Test Your Knowledge

Compare mean TOTCHOL between diabetic (`DIABETES=1`) and non-diabetic (`DIABETES=0`) participants. **(a)** Is this independent or paired? **(b)** State H₀ and H₁. **(c)** Run the test and interpret the output including Cohen's d.

````{dropdown} Show Solution
```r
# (a) Independent — diabetic and non-diabetic are two different groups
#     of different individuals.

# (b) H_0: \mu_{TOTCHOL(diabetes)} = \mu_{TOTCHOL(no diabetes)}
#     H_1: \mu_{TOTCHOL(diabetes)} \neq \mu_{TOTCHOL(no diabetes)}

# (c) R code:
fram_data$DIABETES <- factor(fram_data$DIABETES, levels=c(0,1),
  labels=c("No diabetes","Diabetes"))

tapply(fram_data$TOTCHOL, fram_data$DIABETES, mean)
t.test(TOTCHOL ~ DIABETES, data = fram_data)

# Effect size:
m_diab  <- mean(fram_data$TOTCHOL[fram_data$DIABETES=="Diabetes"], na.rm=TRUE)
m_nodiab<- mean(fram_data$TOTCHOL[fram_data$DIABETES=="No diabetes"], na.rm=TRUE)
d_chol  <- abs(m_diab - m_nodiab) / sd(fram_data$TOTCHOL, na.rm=TRUE)
cat("Cohen's d =", round(d_chol, 3))
```
````

## Key Terms

| Term | Definition |
|---|---|
| **Independent t-test** | Compares means of two unrelated groups. |
| **Paired t-test** | Compares two measurements from same individuals or matched pairs. Unit = difference scores. |
| **Levene's test** | Tests equality of variances. p > 0.05 → assume equal variances. |
| **Welch's correction** | Adjusts df when group variances are unequal. Default in R. |
| **Difference score (d)** | Per-person change: after − before. Paired t-test analyses these directly. |

## Review Questions

1. A Framingham researcher compares mean BMI between participants with prevalent hypertension and those without. Is this independent or paired? State H₀ and H₁, run the test in R, and interpret.

2. Explain why Welch's t-test is preferred over the standard equal-variance t-test as a default in modern practice.

3. In the paired t-test example, why is the Shapiro-Wilk test applied to the *difference scores* rather than to the before and after measurements separately?

4. Run `t.test(SYSBP ~ CURSMOKE, data=fram_data)` in R. Report t, df, p, and the 95% CI for the difference. Is the difference clinically meaningful given the SD of SYSBP (~22 mmHg)?

5. Explain, using the concept of between-person variability, why a paired t-test for a pre/post BP study would have higher power than an independent t-test on the same data.

```{admonition} Key Takeaways
:class: tip

- **Independent:** two unrelated groups; t = (x̄₁ − x̄₂)/SE_diff.
- **Paired:** same individuals twice; t = d̄/(s_d/√n). Higher power.
- **Levene's test** → check equal variances. **Welch's correction** → safer default.
- **Never** run independent t-test on pre/post data from the same individuals.
- Always report Cohen's d — statistical significance alone is not enough.
```

*Next: **Chapter 7 — Scaling Up** extends to three or more groups and categorical associations.*

---

> Part II — Testing What We Think We Know
