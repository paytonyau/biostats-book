# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 9: Course Review and Final Examination Preparation
## *Consolidating the Compass — From Data Types to Prediction*

> **Course:** HS 4510 Biostatistics | **Unit:** 9 | **Part:** IV — The Evidence-Based Practitioner
>
> **Note:** This chapter contains no new statistical theory. Its purpose is consolidation, synthesis, and examination preparation. Work through every section before attempting the Final Exam.

---

## How to Use This Chapter

This chapter is organised in three parts:

**Part I — Course Summary** re-maps every chapter's core question and primary tools in a single view, so you can see the full logical progression from description to inference to prediction.

**Part II — Test Selection Decision Guide** provides a step-by-step framework for choosing the correct statistical test from first principles — the most common source of errors in examinations.

**Part III — Software Cheat Sheets** provides a complete, exam-ready reference for every analysis covered in this course, in both PSPP and R. Each entry states the task, the exact menu path or function, and the critical output to read.

---

## Part I — Course Summary

### The Journey from Data to Evidence

Every chapter in this course added one new capability to your analytical toolkit. The progression follows a deliberate logic:

| Chapter | Core question answered | Primary tools |
|---|---|---|
| **Ch. 1 — The Vitals of Data** | What type of data do I have? | Nominal, Ordinal, Interval, Ratio; categorical vs. continuous |
| **Ch. 2 — The Middle and the Mess** | How do I summarise a distribution? | Mean, Median, Mode; Range, Variance, Standard Deviation |
| **Ch. 3 — The Margin of Error** | How certain can I be in my estimate? | Standard Error (SE); 95% Confidence Interval |
| **Ch. 4 — The Laws of Chance** | What shape does this data follow? | Normal distribution; Z-score; Binomial; Poisson; Shapiro-Wilk |
| **Ch. 5 — The Hypothesis Gamble** | Was this result real or chance? How was the sample obtained? | Sampling methods; p-value; α; Type I/II errors; Power analysis; One-sample t-test |
| **Ch. 6 — A Tale of Two Groups** | Do these two groups differ significantly? | Independent Samples t-test; Paired Samples t-test; Levene's Test |
| **Ch. 7 — Scaling Up** | Do 3+ groups differ? How do two attack rates compare? Are two categorical variables associated? | One-Way ANOVA; Tukey's HSD; Two-proportion z-test; RD; RR; Chi-Square; Fisher's Exact |
| **Ch. 8 — Reading the Future** | How strong is the relationship between two continuous variables, and can I predict one from the other? | Pearson's r; Simple Linear Regression; β₀; β₁; R² |

---

### The Four Course Learning Outcomes — Where They Are Addressed

| Learning outcome | Primary chapters |
|---|---|
| **LO1:** Select, use, and interpret descriptive statistical methods | Ch. 1, Ch. 2, Ch. 3 |
| **LO2:** Select, use, and interpret inferential methods and study design | Ch. 3, Ch. 5, Ch. 6, Ch. 7, Ch. 8 |
| **LO3:** Communicate results accurately and effectively | Interpretation sections throughout Chs. 5–8 |
| **LO4:** Recognise data types and discuss ethical use of data | Ch. 1 (data types); Ch. 5 (sampling equity, study design); Ch. 7 (research integrity) |

---

### Key Formulas Reference

| Measure | Formula | Chapter |
|---|---|:---:|
| Sample mean | $$\bar{x} = \frac{\sum x_i}{n}$$ | 2 |
| Sample variance (Bessel's correction) | $$s^2 = \frac{\sum(x_i - \bar{x})^2}{n-1}$$ | 2 |
| Standard Error of the mean | $$SE = \frac{s}{\sqrt{n}}$$ | 3 |
| 95% Confidence Interval | $$\bar{x} \pm 1.96 \times SE$$ | 3 |
| Z-score | $$Z = \frac{x - \mu}{\sigma}$$ | 4 |
| One-sample t-statistic | $$t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}}$$ | 5 |
| Family-wise error rate (k tests) | $$1 - (1-\alpha)^k$$ | 7 |
| F-statistic (ANOVA) | $$F = \frac{\text{Between-group variance (MS}_B\text{)}}{\text{Within-group variance (MS}_W\text{)}}$$ | 7 |
| Risk Difference | $$RD = p_1 - p_2$$ | 7 |
| Relative Risk | $$RR = \frac{p_1}{p_2}$$ | 7 |
| Two-proportion z-statistic | $$z = \frac{p_1 - p_2}{\sqrt{\hat{p}(1-\hat{p})(1/n_1 + 1/n_2)}}$$ | 7 |
| Chi-Square statistic | $$\chi^2 = \sum \frac{(O - E)^2}{E}$$ | 7 |
| Expected cell count (Chi-Square) | $$E = \frac{\text{Row total} \times \text{Column total}}{n}$$ | 7 |
| Pearson's r | $$r = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i-\bar{x})^2 \cdot \sum(y_i-\bar{y})^2}}$$ | 8 |
| Regression equation | $$\hat{y} = \beta_0 + \beta_1 x$$ | 8 |
| Coefficient of Determination | $$R^2 = r^2$$ | 8 |
| Bonferroni correction | $$\alpha_{\text{corrected}} = \frac{\alpha}{k}$$ | 7 |

---

### Effect Size Reference

| Test | Effect size measure | Scale / interpretation |
|---|---|---|
| t-tests | Cohen's d = (mean difference) / SD | < 0.2 trivial; 0.2–0.5 small; 0.5–0.8 medium; > 0.8 large |
| ANOVA | η² = SS_Between / SS_Total | < 0.01 small; 0.06–0.14 medium; > 0.14 large |
| Two-proportion | Risk Difference (RD); Relative Risk (RR) | RD in percentage points; RR = 1 (no effect), > 1 (increased risk), < 1 (protective) |
| Chi-Square | Cramér's V | < 0.10 negligible; 0.10–0.30 small; 0.30–0.50 moderate; > 0.50 large |
| Correlation | r | ±0.1–0.3 weak; ±0.3–0.6 moderate; ±0.7–0.9 strong; ±1.0 perfect |
| Regression | R² (× 100 = % variance explained) | < 10% weak; 30–59% moderate; 60–79% strong; ≥ 80% very strong |

---

## Part II — Test Selection Decision Guide

The most common examination error is selecting the wrong statistical test. Work through the questions below in order for any question presented to you.

---

### Step 1 — What is the research question asking?

| The question asks… | Tool to use |
|---|---|
| Describe / summarise a single variable | **Descriptive statistics** — mean, SD, median (Ch. 2). No significance test. |
| Estimate a population value from a sample | **Confidence Interval** (Ch. 3) |
| Is this variable Normally distributed? | **Shapiro-Wilk test** (Ch. 4) |
| Is this sample mean different from a known value? | **One-sample t-test** (Ch. 5) |
| Do **two** groups differ on a **continuous** outcome? | → Go to Step 2 |
| Do **three or more** groups differ on a **continuous** outcome? | **One-Way ANOVA** + Tukey's HSD (Ch. 7) |
| Are **two proportions (attack rates)** different? | **Two-proportion z-test** + RD + RR (Ch. 7) |
| Are **two categorical** variables associated? | **Chi-Square test** or Fisher's Exact Test (Ch. 7) |
| How strongly are **two continuous** variables related? | **Pearson's r** (Ch. 8) |
| Can I **predict** one continuous variable from another? | **Simple linear regression** (Ch. 8) |

---

### Step 2 — Choosing between the two-group t-tests

**Were the same individuals measured twice, or were two separate independent groups measured once each?**

| Study design | Correct test |
|---|---|
| Two **independent** groups (e.g., Drug A patients vs. Drug B patients) | **Independent Samples t-test** (Ch. 6) |
| **Same individuals** measured twice (before/after), or cases matched to controls | **Paired Samples t-test** (Ch. 6) |

---

### Step 3 — Checking assumptions before running any test

| Test | Assumptions to verify before proceeding |
|---|---|
| Any t-test | Continuous outcome; approximate Normality within each group (Shapiro-Wilk p > 0.05); independent t-test → check Levene's Test for equal variances |
| One-Way ANOVA | Continuous outcome; Normality within each group; Levene's Test for equal variances; independent observations |
| Two-proportion z-test | Each cell of the 2×2 table has ≥ 5 observations; independent groups |
| Chi-Square | All **expected** cell counts ≥ 5; independent observations. If any expected count < 5 → **Fisher's Exact Test** instead |
| Pearson's r | Linear relationship (verify with scatterplot); approximate bivariate Normality; no severe outliers |
| Simple linear regression | Linearity (scatterplot); independence of residuals; homoscedasticity (residuals vs. fitted plot); Normality of residuals (Q-Q plot) |

---

### Step 4 — Reporting standard

Every statistical result in this course requires four components:

1. **Test statistic with degrees of freedom** — e.g., t(28) = 3.14, F(2, 87) = 7.6, χ²(4) = 12.3, r(48) = −0.71
2. **P-value** — exact value if possible; "p < 0.001" when below that threshold
3. **Effect size** — Cohen's d, η², RD, RR, Cramér's V, r, or R² as appropriate for the test
4. **One plain-language sentence** — stating the direction and practical meaning for the specific question asked

**Reporting template:**

> "A [test name] indicated that [outcome] was [significantly / not significantly] [higher / lower / associated] [t/F/χ²/z/r details], p = [value], [effect size] = [value]. [Plain-language sentence stating the practical conclusion]."

---

## Part III — Software Cheat Sheets

---

## PSPP Cheat Sheet

**Starting every session:**

1. Open PSPP → **File → Open → Data** → select your `.sav` file.
2. Examine column definitions in the **Variable View** tab (name, type, labels, values).
3. Enter or view data in the **Data View** tab.
4. All output appears in the **Output Viewer** window after each analysis.

> **Saving your work:** File → Save Data (saves the `.sav` file). Use File → Export → CSV to export data.

---

### CH. 2 — Descriptive Statistics

| Task | Menu path | Key output |
|---|---|---|
| Frequencies for a categorical variable | Analyze → Descriptive Statistics → **Frequencies** | Counts and valid % per category |
| Mean, SD, Min, Max for a continuous variable | Analyze → Descriptive Statistics → **Descriptives** | Mean, Std. Deviation, Min, Max |
| Full summary including CI, skewness, Shapiro-Wilk | Analyze → Descriptive Statistics → **Explore** | Mean ± 95% CI, Shapiro-Wilk row |
| Histogram | Graphs → **Chart Builder** → Histogram | Visual check for bell shape |

---

### CH. 3 — Confidence Interval for a Mean

| Task | Menu path | Key output |
|---|---|---|
| 95% CI for the population mean | Analyze → Descriptive Statistics → **Explore** | "95% Confidence Interval for Mean" — Lower Bound and Upper Bound |

---

### CH. 4 — Normality Testing

| Task | Menu path | Key output |
|---|---|---|
| Shapiro-Wilk test | Analyze → Descriptive Statistics → Explore → **Plots** → tick "Normality plots with tests" | Shapiro-Wilk row: **Sig. > 0.05** = Normality not violated |

---

### CH. 5 — One-Sample T-Test

| Task | Menu path | Key output |
|---|---|---|
| Test whether sample mean equals a known value μ₀ | Analyze → Compare Means → **One-Sample T Test** | Enter μ₀ in *Test Value* box. Read: **t**, **df**, **Sig. (2-tailed)** (p-value), **95% CI of the Difference** |

---

### CH. 6 — Independent Samples T-Test

| Task | Menu path | Key output |
|---|---|---|
| Compare means of two separate groups | Analyze → Compare Means → **Independent-Samples T Test** | Define Groups (e.g., 0 and 1). **Levene's Test**: Sig. > 0.05 → read top row (equal variances assumed); Sig. < 0.05 → read bottom row (Welch's). Then: **t**, **df**, **Sig. (2-tailed)**, **Mean Difference**, **95% CI of the Difference** |

---

### CH. 6 — Paired Samples T-Test

| Task | Menu path | Key output |
|---|---|---|
| Compare two measurements from the same individuals | Analyze → Compare Means → **Paired-Samples T Test** | Move both columns into *Paired Variables*. Read: **t**, **df**, **Sig. (2-tailed)**, **Mean** (mean difference), **95% CI of the Difference** |

---

### CH. 7 — One-Way ANOVA with Tukey's HSD

| Task | Menu path | Key output |
|---|---|---|
| Compare means across 3+ groups | Analyze → Compare Means → **One-Way ANOVA** | Dependent = outcome; Factor = group variable. Options → tick *Descriptive* + *Homogeneity of variance test*. Post Hoc → tick *Tukey*. Read ANOVA table: **F**, **df** (Between Groups, Within Groups), **Sig.** Then Multiple Comparisons table: **Sig.** per pair (< 0.05 = that pair differs) |

---

### CH. 7 — Two-Proportion Z-Test and Relative Risk

| Task | Menu path | Key output |
|---|---|---|
| Compare attack rates of two groups; get Relative Risk | Analyze → Descriptive Statistics → **Crosstabs** | Row = exposure (0/1); Column = outcome (0/1). Statistics → tick **Chi-square** + **Risk**. Cells → tick **Row percentages**. Read: row % gives p₁ and p₂; **Risk Estimate** table → Risk Ratio (= RR) with 95% CI; Pearson Chi-Square = z² (take √ to recover z) |

---

### CH. 7 — Chi-Square Test

| Task | Menu path | Key output |
|---|---|---|
| Test association between two categorical variables | Analyze → Descriptive Statistics → **Crosstabs** | Statistics → tick **Chi-square** + **Phi and Cramér's V**. Cells → tick **Expected**. Read: Pearson Chi-Square row — **Value** (χ²), **df**, **Asymp. Sig.** (p-value). Check footnote: cells with expected count < 5 flagged → switch to Fisher's Exact Test |
| Fisher's Exact Test (small expected counts) | Same Crosstabs dialog — appears automatically in the Chi-Square Tests table for 2×2 tables | Read: Fisher's Exact Test row — **Exact Sig. (2-sided)** |

---

### CH. 8 — Pearson's Correlation

| Task | Menu path | Key output |
|---|---|---|
| Correlation between two continuous variables | Analyze → Correlate → **Bivariate** | Move both variables into Variables box; tick *Pearson*. Read matrix: **r** at intersection, **Sig. (2-tailed)** below it. ** = p < 0.01; * = p < 0.05 |

---

### CH. 8 — Simple Linear Regression

| Task | Menu path | Key output |
|---|---|---|
| Predict a continuous outcome from a continuous predictor | Analyze → Regression → **Linear** | Dependent = y; Independent(s) = x. Statistics → tick *Estimates* + *Model fit*. Read: **Model Summary** → R Square (= R²); **ANOVA** table → Sig. (overall model p-value); **Coefficients** → Constant row = β₀, predictor row = β₁, Sig. column = p-value for each coefficient |

---

### CH. 9 — Dummy Variable from Text

| Task | Menu path | Key output |
|---|---|---|
| Recode qualitative text column into 0/1 | Transform → **Recode into Different Variables** | Output Variable: type new name → Change. Old and New Values: map each text value to 0 or 1. Verify in Data View |

---

### Bonferroni Correction (No Dedicated Dialog)

Calculate manually: **α_corrected = 0.05 / k**, where k = number of simultaneous tests. Apply this threshold when reading individual p-values from multiple tests. Report the correction method and k in any write-up.

---

## R / RStudio Cheat Sheet

**Starting every session:**

```r
# ── Install packages once ────────────────────────────────
# install.packages(c("HistData", "car", "vcd", "pwr"))

# ── Load packages ────────────────────────────────────────
library(HistData)   # Snow.deaths dataset
library(car)        # leveneTest()
library(vcd)        # assocstats() for Cramér's V
# library(pwr)      # power analysis (optional)

# ── Load dataset ─────────────────────────────────────────
cholera_data <- Snow.deaths

# ── First look at the data ───────────────────────────────
head(cholera_data)      # first 6 rows
str(cholera_data)       # variable names, types, preview
summary(cholera_data)   # min, max, mean, quartiles per column
dim(cholera_data)       # rows × columns
names(cholera_data)     # column names only
```

---

### CH. 2 — Descriptive Statistics

```r
# ── Central tendency ─────────────────────────────────────
mean(cholera_data$Age,   na.rm = TRUE)
median(cholera_data$Age, na.rm = TRUE)

# Mode (base R has no mode() — use table)
sort(table(cholera_data$Age), decreasing = TRUE)[1]

# ── Variability ──────────────────────────────────────────
sd(cholera_data$Age,  na.rm = TRUE)   # standard deviation
var(cholera_data$Age, na.rm = TRUE)   # variance
range(cholera_data$Age, na.rm = TRUE) # min and max
IQR(cholera_data$Age,  na.rm = TRUE)  # interquartile range

# ── Full numeric summary ─────────────────────────────────
summary(cholera_data$Age)

# ── By group ─────────────────────────────────────────────
tapply(cholera_data$Age, cholera_data$Pump_Used, mean)
tapply(cholera_data$Age, cholera_data$Pump_Used, sd)
tapply(cholera_data$Age, cholera_data$Pump_Used, length)

# ── Frequencies (categorical) ────────────────────────────
table(cholera_data$Pump_Used)
prop.table(table(cholera_data$Pump_Used))  # as proportions
```

---

### CH. 3 — Confidence Interval for a Mean

```r
# t.test() on a single variable returns the 95% CI
result <- t.test(cholera_data$Age)
result$conf.int          # [lower, upper]

# Manual 95% CI
n    <- sum(!is.na(cholera_data$Age))
xbar <- mean(cholera_data$Age, na.rm = TRUE)
se   <- sd(cholera_data$Age, na.rm = TRUE) / sqrt(n)
c(xbar - 1.96 * se, xbar + 1.96 * se)  # [lower, upper]
```

---

### CH. 4 — Normality Testing

```r
# ── Histogram with Normal curve overlay ──────────────────
hist(cholera_data$Age, freq = FALSE,
     main = "Distribution of Age", xlab = "Age",
     col = "lightblue", border = "white")
curve(dnorm(x, mean = mean(cholera_data$Age, na.rm=TRUE),
               sd   = sd(cholera_data$Age, na.rm=TRUE)),
      add = TRUE, col = "red", lwd = 2)

# ── Normal Q-Q plot ───────────────────────────────────────
qqnorm(cholera_data$Age, main = "Normal Q-Q: Age")
qqline(cholera_data$Age, col = "red", lwd = 2)
# Points tracking the line closely → approximate Normality

# ── Shapiro-Wilk test ────────────────────────────────────
shapiro.test(cholera_data$Age)
# p > 0.05 → do not reject Normality (proceed with t-tests/ANOVA)
# p < 0.05 → evidence of non-Normality (consider non-parametric test)
```

---

### CH. 5 — One-Sample T-Test

```r
# Test whether mean Age differs from a known value (e.g., μ₀ = 40)
result <- t.test(cholera_data$Age, mu = 40)
result

# Key output:
# t         → test statistic
# df        → degrees of freedom (n − 1)
# p-value   → compare to 0.05
# conf.int  → 95% CI of (x̄ − μ₀)
# estimate  → sample mean

# Extract individual components
result$statistic   # t
result$parameter   # df
result$p.value     # p-value
result$conf.int    # 95% CI
result$estimate    # sample mean
```

---

### CH. 6 — Independent Samples T-Test

```r
# Step 1: Check equal-variance assumption with Levene's Test
leveneTest(Age ~ as.factor(Pump_Used), data = cholera_data)
# p > 0.05 → use var.equal = TRUE  (Student's t-test)
# p < 0.05 → use var.equal = FALSE (Welch's t-test — safer default)

# Step 2: Run the t-test
result <- t.test(Age ~ Pump_Used,
                 data      = cholera_data,
                 var.equal = FALSE)   # Welch's correction
result

# Key output:
# t, df (Welch's df is non-integer), p-value
# conf.int  → 95% CI of the mean difference
# estimate  → mean of each group
```

---

### CH. 6 — Paired Samples T-Test

```r
# Two vectors of equal length (same individuals, two time points)
before <- c(120, 135, 128, 145, 118)
after  <- c(110, 120, 118, 130, 112)

result <- t.test(before, after, paired = TRUE)
result

# Key output:
# t, df (= n_pairs − 1), p-value
# conf.int → 95% CI of the mean difference (before − after)
# estimate → mean of the difference scores
```

---

### CH. 7 — One-Way ANOVA and Tukey's HSD

```r
# Step 1: Check equal-variance assumption
leveneTest(Distance_Metres ~ as.factor(Pump_Used),
           data = cholera_data)

# Step 2: Fit the ANOVA model
anova_model <- aov(Distance_Metres ~ Pump_Used,
                   data = cholera_data)
summary(anova_model)
# Key output: Pr(>F) — overall F-test p-value
# If p < 0.05 → at least one group differs → run Tukey's HSD

# Step 3: Tukey's HSD post-hoc test
TukeyHSD(anova_model)
# Key output per pair: diff, lwr, upr, p adj
# p adj < 0.05 → that specific pair differs significantly
# CI not containing 0 → confirms significant difference

# Step 4: Visualise
boxplot(Distance_Metres ~ Pump_Used, data = cholera_data,
        main = "Distance by Pump Group",
        xlab = "Pump", ylab = "Distance (m)",
        col  = c("lightblue", "lightgreen", "lightyellow"))
```

---

### CH. 7 — Two-Proportion Z-Test, Risk Difference, Relative Risk

```r
# Step 1: Create binary exposure and outcome columns
cholera_data$Near_Pump <- ifelse(
  cholera_data$Distance_Metres <= 100, 1, 0)
cholera_data$Death_Any <- ifelse(
  cholera_data$Household_Deaths > 0, 1, 0)

# Step 2: 2×2 table
tab <- table(Exposed  = cholera_data$Near_Pump,
             Outcome  = cholera_data$Death_Any)
tab
# Columns: 0 = no death, 1 = death
# Rows:    0 = far, 1 = near

# Step 3: Extract counts
a <- tab[2, 2]; b <- tab[2, 1]   # near: death / no death
c <- tab[1, 2]; d <- tab[1, 1]   # far:  death / no death
n1 <- a + b;    n2 <- c + d      # group totals
p1 <- a / n1;   p2 <- c / n2     # attack rates

# Step 4: Compute effect sizes
RD <- p1 - p2    # Risk Difference (percentage points)
RR <- p1 / p2    # Relative Risk (ratio)
cat("p1:", round(p1,3), " p2:", round(p2,3), "\n")
cat("RD:", round(RD,3), " RR:", round(RR,3), "\n")

# Step 5: Two-proportion z-test
prop.test(c(a, c), c(n1, n2), correct = FALSE)
# Key output: X-squared = z²; p-value; conf.int for RD

# Step 6: 95% CI for Risk Difference (manual)
SE_RD <- sqrt(p1*(1-p1)/n1 + p2*(1-p2)/n2)
cat("95% CI for RD: [",
    round(RD - 1.96*SE_RD, 3), ",",
    round(RD + 1.96*SE_RD, 3), "]\n")

# Step 7: 95% CI for Relative Risk (log-transform method)
SE_lnRR <- sqrt((1-p1)/a + (1-p2)/c)
cat("95% CI for RR: [",
    round(exp(log(RR) - 1.96*SE_lnRR), 3), ",",
    round(exp(log(RR) + 1.96*SE_lnRR), 3), "]\n")
```

---

### CH. 7 — Chi-Square Test and Fisher's Exact Test

```r
# Step 1: Build the contingency table
ct <- table(Pump     = cholera_data$Pump_Used,
            Severity = cholera_data$Severity)

# Step 2: Check expected counts BEFORE running the test
chisq.test(ct)$expected
# All expected counts must be ≥ 5
# If any < 5 → use Fisher's Exact Test

# Step 3: Chi-Square test
chi_result <- chisq.test(ct)
chi_result
# Key output: X-squared (χ²), df, p-value

# Step 4: Effect size — Cramér's V
library(vcd)
assocstats(ct)
# Cramér's V: < 0.10 negligible | 0.10–0.30 small
#              0.30–0.50 moderate | > 0.50 large

# Step 5: Adjusted standardised residuals (which cells drive it?)
chi_result$stdres
# > +2 or < −2 → that cell contributes significantly to χ²

# Step 6: Fisher's Exact Test (when expected counts < 5)
fisher.test(ct)                                      # 2×2 tables
fisher.test(ct, simulate.p.value = TRUE, B = 10000) # larger tables
```

---

### CH. 8 — Pearson's Correlation

```r
# Step 1: Always plot first — correlation assumes linearity
plot(cholera_data$Distance_Metres,
     cholera_data$Household_Deaths,
     main = "Scatterplot: Distance vs. Deaths",
     xlab = "Distance (m)", ylab = "Deaths",
     pch = 19, col = "steelblue")

# Step 2: Pearson's r with significance test
cor_result <- cor.test(cholera_data$Distance_Metres,
                       cholera_data$Household_Deaths,
                       method = "pearson")
cor_result
# Key output: cor (r), t, df, p-value, 95% CI for ρ

# Extract r and p-value
cor_result$estimate   # r
cor_result$p.value    # p-value
cor_result$conf.int   # 95% CI for rho

# Step 3: Spearman's r (when Normality is violated)
cor.test(cholera_data$Distance_Metres,
         cholera_data$Household_Deaths,
         method = "spearman")
```

---

### CH. 8 — Simple Linear Regression

```r
# Step 1: Fit the model (OLS)
model <- lm(Household_Deaths ~ Distance_Metres,
            data = cholera_data)

# Step 2: Full output
summary(model)
# Key output:
# Coefficients table:
#   (Intercept) = β₀ (predicted y when x = 0)
#   Distance_Metres = β₁ (change in y per 1-unit increase in x)
#   Pr(>|t|) = p-value for each coefficient
# Multiple R-squared = R² (proportion of variance explained)
# F-statistic p-value = overall model significance

# Step 3: Extract specific values
coef(model)["(Intercept)"]        # β₀
coef(model)["Distance_Metres"]    # β₁
summary(model)$r.squared          # R²

# Step 4: Make predictions with 95% prediction interval
new_data <- data.frame(Distance_Metres = c(50, 100, 150, 200))
predict(model, newdata = new_data, interval = "prediction")
# fit = predicted ŷ; lwr and upr = 95% prediction interval

# Step 5: Plot with regression line
plot(cholera_data$Distance_Metres,
     cholera_data$Household_Deaths,
     main = "Regression: Deaths ~ Distance",
     xlab = "Distance (m)", ylab = "Deaths",
     pch = 19, col = "steelblue")
abline(model, col = "red", lwd = 2)

# Step 6: Diagnostic plots (check assumptions)
par(mfrow = c(2, 2))
plot(model)
par(mfrow = c(1, 1))
# Plot 1 (Residuals vs Fitted):  no curved pattern → linearity OK
#                                uniform spread → homoscedasticity OK
# Plot 2 (Normal Q-Q):          points near diagonal → residual Normality OK
# Plot 3 (Scale-Location):      flat line → homoscedasticity confirmed
# Plot 4 (Residuals vs Leverage): no points in top-right → no influential outliers
```

---

### Dummy Variable Creation

```r
# Recode qualitative text into a binary 0/1 variable
cholera_data$Boils_Water_Dummy <- ifelse(
  cholera_data$Interview_Notes == "Boils water", 1, 0)

# Verify the coding
table(Original = cholera_data$Interview_Notes,
      Dummy    = cholera_data$Boils_Water_Dummy)

# Use dummy as grouping variable in t-test
t.test(Household_Deaths ~ Boils_Water_Dummy,
       data = cholera_data)
```

---

### Multiple Comparison Correction (Bonferroni)

```r
# Adjust a vector of raw p-values for k simultaneous tests
raw_p <- c(0.031, 0.048, 0.002, 0.190, 0.067,
           0.044, 0.210, 0.003, 0.090, 0.041)

bonferroni_p <- p.adjust(raw_p, method = "bonferroni")

data.frame(Test    = paste0("Test_", seq_along(raw_p)),
           Raw_p   = raw_p,
           Adj_p   = round(bonferroni_p, 4),
           Sig_raw = raw_p < 0.05,
           Sig_adj = bonferroni_p < 0.05)

# Manual threshold
k <- length(raw_p)
cat("Bonferroni threshold: alpha =", 0.05 / k, "\n")
```

---

## A Final Word from Soho, 1854

Dr John Snow did not have PSPP or R. He had no access to a p-value threshold, no regression software, and no hypothesis test in the modern sense. What he had was the intellectual discipline to ask a precise question, the methodological rigour to collect systematic data, the analytical care to plot it accurately on a map, and — critically — the courage to close his ledger, walk into the streets of Soho, and ask the people living there what was actually happening in their lives.

The Biostatistics Compass is not a book about software. It is a book about decision-making under uncertainty — about measuring what can be measured, acknowledging what cannot, and taking responsibility for the gap between the two. Every tool in Chapters 1 through 8 is a lens. How you aim the lens, what you do with what you see through it, and how honestly you report it — that is the work of a public health practitioner.

When you open a dataset, remember: every row was once a person.

Use the Compass carefully.

---

> Part IV — Consolidation and Examination Preparation | *End of course.*
