# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 7: Scaling Up

## *ANOVA, Chi-Square, and Categorical Comparisons*

> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Explain the multiple comparisons problem and calculate the FWER
- Run and interpret a **one-way ANOVA** with Tukey's HSD post-hoc test
- Compare two proportions and calculate risk difference and relative risk
- Run and interpret a **Chi-Square test** and **Fisher's Exact Test**
- Calculate and interpret **Cramér's V** as an effect size
```

## Before You Begin: Beyond Two Groups

Chapters 5–6 handled one or two groups. Cardiovascular epidemiology routinely compares three or more, or examines whether risk factors are associated with disease outcomes. This chapter extends the toolkit in two directions:

1. **Three or more continuous groups** → One-Way ANOVA
2. **Two categorical variables** → Chi-Square / Fisher's Exact Test

## Section 1: The Multiple Comparisons Problem

> 💡 **Plain English first:** Run enough tests and you will eventually get a "significant" result by chance. With α = 0.05, every test has a 5% false-positive rate. Run 14 independent tests and you have ~50% chance of at least one spurious positive.

### 1.1 Why Repeated T-Tests Fail

### 1.1 Why Repeated T-Tests Fail

The Framingham dataset includes four education levels (`EDUC` = 1, 2, 3, 4). To compare mean cholesterol across all four groups using t-tests, you would need six pairwise comparisons. The **Family-Wise Error Rate (FWER)**:

$$FWER = 1 - (1 - \alpha)^k$$

With $k = 6$ comparisons, $\alpha = 0.05$: FWER = $1 - 0.95^6 \approx$ **26%** — more than five times the intended Type I error rate.

### 1.2 The Solution: One Protected Analysis

ANOVA performs a single omnibus test comparing all groups simultaneously, protecting the Type I error rate at exactly α = 0.05.

## Section 2: One-Way ANOVA

### 2.1 Research Question

**Framingham:** Does mean TOTCHOL differ across education levels (`EDUC` = 1–4)?

- $H_0: \mu_1 = \mu_2 = \mu_3 = \mu_4$ (all group means equal)
- $H_1:$ At least one group mean differs from the others

### 2.2 The F-Statistic

```{figure} ../images/ch07_anova_decomposition.png
:name: fig-anova
:width: 85%
:align: center

**Figure 7.1** ANOVA decomposes total variance into between-group variance (signal) and within-group variance (noise). The F-ratio measures signal/noise. In the Framingham context, between-group variance captures genuine cholesterol differences by education level; within-group variance captures individual variation unrelated to education.
```

$$F = \frac{MS_{Between}}{MS_{Within}} = \frac{\text{Signal}}{\text{Noise}}$$

- **F ≈ 1:** Between-group differences are no larger than random within-group variation.
- **F >> 1:** Group means differ more than chance alone would predict.

**Effect size: η²**
$$\eta^2 = SS_{Between} / SS_{Total}$$
η² = 0.01 small, 0.06 medium, 0.14 large.

### 2.3 Post-Hoc Tests — Tukey's HSD

A significant F tells you *something* differs among the four education groups. **Tukey's HSD** identifies *which specific pairs* differ, whilst maintaining the family-wise error rate at α.

Only apply Tukey's HSD *after* a significant omnibus F — running post-hoc tests after a non-significant ANOVA is invalid.

### 2.4 Assumptions

| Assumption | Check |
|---|---|
| Independence | Design-level |
| Approximately Normal within groups | Histogram + Shapiro-Wilk per group; robust when n > 30 per group |
| Homogeneity of variances | Levene's test; if violated, use Welch ANOVA |

## Section 3: Comparing Two Proportions

**Research question:** Is the proportion with incident CHD (`ANYCHD`) different between smokers and non-smokers?

**Effect sizes:**
- **Risk Difference (RD):** $p_1 - p_2$ — absolute difference in proportions.
- **Relative Risk (RR):** $p_1 / p_2$ — how many times more likely the event is in group 1.

$$z = \frac{\hat{p}_1 - \hat{p}_2}{\sqrt{\hat{p}(1-\hat{p})(\frac{1}{n_1}+\frac{1}{n_2})}}$$

**Epidemiological interpretation:** Suppose smokers have 38% CHD incidence and non-smokers 25%:

| Measure | Formula | Result | Plain language |
|---|---|---|---|
| **Risk Difference (RD)** | p₁ − p₂ | 38% − 25% = **+13%** | 13 extra cases per 100 smokers |
| **Relative Risk (RR)** | p₁ / p₂ | 38/25 = **1.52** | Smokers are 52% more likely to develop CHD |
| **Attributable Risk (AR)** | RD × 100 | **13 cases per 100** | Number of CHD cases caused by smoking per 100 exposed |
| **Number Needed to Harm (NNH)** | 1/RD | 1/0.13 = **7.7** | On average, about 8 smokers develop an "extra" CHD event that would not have occurred without smoking |

**Person-time and incidence rates:** When follow-up varies across participants (some are followed 5 years, others 10), counting raw events is misleading. Person-time rates divide events by the total time at risk:

$$\text{Incidence rate} = \frac{\text{Number of events}}{\text{Total person-time at risk}}$$

In the Framingham cohort, if 155 CHD events occurred over approximately 4,000 person-years of follow-up: Incidence rate = 155/4000 = **38.8 events per 1,000 person-years**.

> ⚡ **Common mistake:** RR can only be calculated from cohort or experimental studies where the denominator (population at risk) is known. In a **case-control study**, you cannot calculate RR — you use the **Odds Ratio (OR)** instead.

## Section 4: Chi-Square Tests

### 4.1 Research Question

**Framingham:** Is there an association between smoking status (`CURSMOKE`) and CHD incidence (`ANYCHD`)?

- H₀: Smoking and CHD are independent (no association)
- H₁: Smoking and CHD are associated

### 4.2 The Contingency Table

| | ANYCHD = 0 (No event) | ANYCHD = 1 (Event) | Total |
|---|---|---|---|
| **Non-smoker** | a | b | a+b |
| **Smoker** | c | d | c+d |
| **Total** | a+c | b+d | n |

### 4.3 Expected Counts

Under $H_0$ (no association), expected count = (row total $\times$ column total) / $n$.

$$\chi^2 = \sum \frac{(O - E)^2}{E}$$

```{figure} ../images/ch07b_chisquare_cramers_v.png
:name: fig-chisq
:width: 85%
:align: center

**Figure 7.2** Chi-Square test components: contingency table with observed and expected counts, grouped bar chart of proportions, and Cramér's V interpretation scale. In the Framingham context, the grouped bar shows the CHD event rate in smokers vs non-smokers.
```

> ⚡ **Common mistake:** Chi-Square requires **expected** cell counts ≥ 5 in all cells. When this fails, use Fisher's Exact Test. PSPP and R will warn you — but it is your responsibility to check.

**Effect size: Cramér's V**
$$V = \sqrt{\frac{\chi^2}{n \times (\min(r,c)-1)}}$$
V < 0.1 negligible; 0.1–0.29 small; 0.30–0.49 moderate; ≥ 0.50 large.

## 🔬 Lab Manual — Chapter 7

### Objective

Part 1: ANOVA — does mean TOTCHOL differ across education levels?
Part 2: Chi-Square — is smoking associated with CHD incidence?
Part 3: Proportions — calculate RR and RD for smoking → CHD.

### Option A — PSPP

**ANOVA:** Analyze → Compare Means → One-Way ANOVA. `TOTCHOL` as Dependent, `EDUC` as Factor. Post Hoc → Tukey. Options → Homogeneity of variance test.

**Chi-Square:** Analyze → Descriptive Statistics → Crosstabs. `CURSMOKE` as Row, `ANYCHD` as Column. Statistics → Chi-Square + Phi and Cramér's V. Cells → Observed, Expected, Row percentages.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 7 Lab: ANOVA and Chi-Square
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")
library(car)

fram_data$EDUC     <- factor(fram_data$EDUC, levels=1:4,
  labels=c("0-11yrs","HS_GED","Some_college","College_grad"), ordered=TRUE)
fram_data$CURSMOKE <- factor(fram_data$CURSMOKE, levels=c(0,1),
  labels=c("Non-smoker","Smoker"))
fram_data$ANYCHD   <- factor(fram_data$ANYCHD, levels=c(0,1),
  labels=c("No_CHD","CHD_event"))

# ══ Part 1: One-Way ANOVA ════════════════════════════

tapply(fram_data$TOTCHOL, fram_data$EDUC, mean, na.rm=TRUE)
tapply(fram_data$TOTCHOL, fram_data$EDUC, sd,   na.rm=TRUE)

leveneTest(TOTCHOL ~ EDUC, data = fram_data)

anova_result <- aov(TOTCHOL ~ EDUC, data = fram_data)
summary(anova_result)

# Effect size: eta-squared
SS <- summary(anova_result)[[1]]$"Sum Sq"
eta_sq <- SS[1] / sum(SS)
cat("eta-squared =", round(eta_sq, 3), "\n")

TukeyHSD(anova_result)

boxplot(TOTCHOL ~ EDUC, data = fram_data,
        main = "Total Cholesterol by Education Level",
        xlab = "Education", ylab = "TOTCHOL (mg/dL)",
        col = c("#FEE0D2","#FDD0A2","#C7E9C0","#BDD7EE"))

# ══ Part 2: Chi-Square ════════════════════════════════

tbl <- table(fram_data$CURSMOKE, fram_data$ANYCHD)
print(tbl)

# Check expected counts (all should be ≥ 5)
chi_result <- chisq.test(tbl)
print(chi_result$expected)
print(chi_result)

# Cramér's V
V <- sqrt(chi_result$statistic / (sum(tbl) * (min(dim(tbl))-1)))
cat("Cramér's V =", round(V, 3), "\n")

# ══ Part 3: Proportions and RR ═══════════════════════

prop.table(tbl, margin=1)    # Row proportions

# Relative Risk and Risk Difference
p_smoke    <- tbl["Smoker","CHD_event"] / sum(tbl["Smoker",])
p_nosmoke  <- tbl["Non-smoker","CHD_event"] / sum(tbl["Non-smoker",])
RD <- p_smoke - p_nosmoke
RR <- p_smoke / p_nosmoke
cat("CHD rate — smokers:    ", round(p_smoke*100,1), "%\n")
cat("CHD rate — non-smokers:", round(p_nosmoke*100,1), "%\n")
cat("Risk Difference:", round(RD*100,1), "percentage points\n")
cat("Relative Risk:  ", round(RR,2), "\n")

prop.test(tbl)    # Two-proportion z-test
```

**What to examine:**

- **ANOVA:** Does TOTCHOL significantly differ by education level? Which specific pairs differ in Tukey's HSD? Is η² small, medium, or large? Does higher education predict lower or higher cholesterol?
- **Chi-Square:** Is there a statistically significant association between smoking and CHD? Is Cramér's V large enough to be clinically meaningful?
- **RR:** How much more likely are smokers to have a CHD event compared to non-smokers?

### Worked Example — CHD Risk in Smokers vs Non-smokers

Using the Framingham teaching dataset, suppose our 2×2 table shows:

| | CHD event | No CHD event | Total |
|---|---|---|---|
| **Smoker** (n=255) | 98 | 157 | 255 |
| **Non-smoker** (n=245) | 57 | 188 | 245 |
| **Total** | 155 | 345 | 500 |

**Step 1 — Proportions:**

- p_smoker = 98/255 = 38.4%
- p_non_smoker = 57/245 = 23.3%

**Step 2 — Risk Difference:** 38.4% − 23.3% = **+15.1 percentage points**
→ Smoking is associated with 15 extra CHD events per 100 people over 10 years

**Step 3 — Relative Risk:** 38.4/23.3 = **RR = 1.65**
→ Smokers are 65% more likely to develop CHD

**Step 4 — Odds Ratio:** (98 × 188)/(157 × 57) = 18424/8949 = **OR = 2.06**
→ Note: OR (2.06) > RR (1.65) because CHD is common (31%) — they diverge substantially

**Step 5 — NNH:** 1/0.151 = **6.6**
→ On average, about 7 smokers will develop a CHD event that would not have occurred in a non-smoker

**Step 6 — Chi-Square test:**

- Expected counts all ≥ 5 ✓
- χ²(1) = 14.8, p < 0.001
- Cramér's V = 0.17 (small-to-moderate association)

### 🧪 Test Your Knowledge

Test whether **BMI** differs across the four education groups using ANOVA. **(a)** State H₀ and H₁. **(b)** Run ANOVA and Tukey's HSD. **(c)** Calculate η² and interpret. **(d)** Based on the pattern, does education appear to be a social determinant of BMI in this cohort?

````{dropdown} Show Solution
```r
# (a) H_0: \mu_{BMI(educ1)} = \mu_{BMI(educ2)} = \mu_{BMI(educ3)} = \mu_{BMI(educ4)}
#     H_1: At least one education group has a different mean BMI.

# (b) R code:
anova_bmi <- aov(BMI ~ EDUC, data = fram_data)
summary(anova_bmi)
TukeyHSD(anova_bmi)

# (c) eta-squared:
SS_bmi <- summary(anova_bmi)[[1]]$"Sum Sq"
eta_sq_bmi <- SS_bmi[1] / sum(SS_bmi)
cat("eta-squared =", round(eta_sq_bmi, 3))

# (d) If lower education groups have higher mean BMI, this is consistent
#     with the well-documented inverse relationship between socioeconomic
#     status and obesity — a pattern that the Framingham study helped
#     establish in the cardiovascular literature.
```
````

## Key Terms

| Term | Definition |
|---|---|
| **FWER** | Family-Wise Error Rate — probability of ≥1 false positive across k comparisons. |
| **One-Way ANOVA** | Tests whether ≥3 group means differ. F = between/within variance. |
| **Tukey's HSD** | Post-hoc test comparing all group pairs after significant ANOVA, controlling FWER. |
| **η² (eta-squared)** | ANOVA effect size. Proportion of total variance explained by group. |
| **Risk Difference** | p₁ − p₂. Absolute difference in proportions. |
| **Relative Risk (RR)** | p₁/p₂. Multiplicative comparison of event rates between two groups. |
| **Chi-Square test** | Tests independence of two categorical variables. Requires expected counts ≥ 5. |
| **Fisher's Exact Test** | Replaces Chi-Square when expected counts < 5. |
| **Cramér's V** | Chi-Square effect size. 0 = no association, 1 = perfect. |

## Review Questions

1. Calculate the FWER for 6 pairwise comparisons at α = 0.05. Why is ANOVA preferable to running all 6 t-tests?

2. Run `aov(SYSBP ~ EDUC, data=fram_data)` and `TukeyHSD()` in R. Which education groups differ in mean SYSBP? Interpret the pattern in terms of social determinants of cardiovascular health.

3. A Chi-Square test comparing smoking status and DEATH returns χ²(1) = 4.8, p = 0.028, Cramér's V = 0.10. Write a complete one-paragraph interpretation.

4. Calculate RR for the association between prevalent hypertension (`PREVHYP`) and any CHD event (`ANYCHD`). Interpret the result in plain clinical language.

5. Run `chisq.test(table(fram_data$BPMEDS, fram_data$ANYCHD))`. Check expected cell counts. Are they all ≥ 5? If not, which test should you use instead, and why?

```{admonition} Key Takeaways
:class: tip

- **ANOVA** protects Type I error when comparing ≥3 means simultaneously.
- **F = between/within variance.** Significant F → Tukey's HSD to find specific pairs.
- **η²:** < 0.06 small, 0.06–0.14 medium, > 0.14 large.
- **RR** and **RD** quantify the epidemiological burden of a risk factor.
- **Chi-Square** tests categorical associations. Expected cells < 5 → Fisher's Exact.
- **Cramér's V** measures association strength (0–1).
```

*Next: **Chapter 8 — Reading the Future** introduces correlation, regression, and survival analysis.*

---

> Part III — Comparing More Than Two Groups
