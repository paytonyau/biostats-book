# Chapter 8: Reading the Future
## *Correlation, Regression, and Survival Analysis*
---
> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Calculate and interpret **Pearson's r** and **Spearman's rho**
- Distinguish correlation from causation with specific examples
- Fit and interpret a **simple linear regression** model
- Read and construct a **Kaplan-Meier survival curve**
- Interpret and run the **log-rank test**
- Explain why OLS regression is not valid for censored survival data
```

## Before You Begin: From Groups to Relationships and Survival

Chapters 5–7 asked: *are these groups different?* This chapter asks two new questions:

1. **How are two continuous variables related?** — Correlation and regression
2. **How do we analyse time-to-event data?** — Survival analysis

Both questions are essential in cardiovascular epidemiology. Does age predict blood pressure? Does BMI predict cholesterol? And — the clinical question that defined Framingham — does smoking shorten the time to a cardiovascular event?

## Section 1: Correlation

### 1.1 Pearson's r

> 💡 **Plain English first:** Pearson's r is a single number from −1 to +1 that captures how strongly two variables move together in a straight-line pattern.

$$r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i-\bar{x})^2 \cdot \sum(y_i-\bar{y})^2}}$$

```{figure} ../images/ch08_scatterplots_correlation.png
:name: fig-scatter
:width: 85%
:align: center

**Figure 8.1** Scatterplots illustrating Pearson's r from −1.0 to +1.0. In the Framingham dataset, AGE and SYSBP would show a moderate positive correlation ($r \approx +0.3$ to $+0.4$); AGE and BMI a weak positive correlation; SYSBP and DIABP a strong positive correlation (both measure blood pressure in the same patient).
```

**Framingham examples:**
- AGE vs SYSBP: moderate positive r — older participants tend to have higher systolic BP.
- SYSBP vs DIABP: strong positive r — they are two measures from the same cardiovascular system.
- TOTCHOL vs BMI: weak to moderate positive r — higher body weight is associated with higher cholesterol, but the relationship is noisy.

**Assumptions of Pearson's r:**
- Both variables continuous (interval or ratio)
- Linear relationship — check with scatterplot
- Approximate bivariate Normality
- No severe outliers

### 1.2 Spearman's Rho

**Spearman's rho (ρ)** correlates the *ranks* of variables rather than raw values. Use when:
- Data is ordinal (e.g., `EDUC`)
- Data is continuous but skewed (e.g., `CIGPDAY`)
- Outliers are present

### 1.3 Correlation Is Not Causation

> A large, significant r does not prove causation. Three alternative explanations always exist.

| Explanation | Framingham example |
|---|---|
| **Confounding** | AGE correlates with both SYSBP and TOTCHOL — age confounds a BP–cholesterol correlation |
| **Reverse causation** | Does high BP cause high BMI, or high BMI cause high BP? |
| **Coincidence** | In large datasets, spurious correlations appear by chance |

The Framingham Study was longitudinal — participants were followed over time, and risk factors were measured *before* outcomes occurred. This temporal ordering (exposure before disease) is necessary but not sufficient for causal inference. Confounding by unmeasured variables remains possible.

## Section 2: Simple Linear Regression

### 2.1 The Regression Equation

$$\hat{y} = \beta_0 + \beta_1 x$$

**Framingham example:** Does age predict systolic blood pressure?
- $x$ = AGE (predictor)
- $\hat{y}$ = predicted SYSBP (outcome)
- $\beta_0$ = predicted SYSBP when AGE = 0 (mathematically necessary but not clinically interpretable here)
- $\beta_1$ = change in predicted SYSBP for each additional year of age

If $\beta_1 = 0.6$, this means: each additional year of age is associated with a predicted increase of 0.6 mmHg in systolic BP.

### 2.2 Why Use AGE → SYSBP for Regression?

Both AGE and SYSBP are continuous ratio variables. Neither is censored. OLS regression is fully valid.

> **Why NOT use time-to-event variables (TIMECHD, TIMEDTH) in OLS regression?**
> These are **censored survival data** — participants still alive at the end of follow-up have their time recorded but the true time-to-event is unknown. OLS regression ignores this censoring and produces biased, invalid estimates. The correct method for censored data is **survival analysis** (Section 3).

### 2.3 The Coefficient of Determination ($R^2$)

> 💡 **Plain English first:** $R^2$ tells you what proportion of the variation in SYSBP is explained by AGE. $R^2 = 0.15$ means age explains 15% of the variation — the other 85% is due to genetics, diet, stress, medication, and other unmeasured factors.
>
> ⚡ **Common mistake:** $r$ and $R^2$ are different. $r = 0.39$ describes *direction and strength*. $R^2 = 0.15$ describes the *proportion of variance explained*. Students frequently confuse the two.

$$R^2 = r^2 \text{ (for simple linear regression)}$$

### 2.4 Regression Assumptions and Diagnostic Plots

```{figure} ../images/ch08_regression_residuals.png
:name: fig-residuals
:width: 85%
:align: center

**Figure 8.2** Regression diagnostic plots. Residuals vs Fitted: check for patterns (linearity, homoscedasticity). Q-Q plot of residuals: check Normality. Both are produced by `plot(model)` in R.
```

| Assumption | Diagnostic |
|---|---|
| **Linearity** | Scatterplot; Residuals vs Fitted (no pattern = good) |
| **Independence** | Design-level |
| **Homoscedasticity** | Residuals vs Fitted (uniform spread) |
| **Normal residuals** | Q-Q plot of residuals |

## Section 3: Survival Analysis

### 3.1 Why Survival Analysis?

The Framingham dataset contains `TIMEDTH` (days to death) and `DEATH` (0/1 indicator). Participants who were still alive at the end of follow-up have their time recorded, but their true time-to-death is unknown — they are **censored observations**.

OLS regression ignores censoring and underestimates true survival times. Survival analysis handles this correctly.

### 3.2 The Kaplan-Meier Curve

The **Kaplan-Meier (KM) estimator** calculates the probability of surviving beyond each time point, accounting for censoring.


At each event time $t$, the KM survival probability is updated:

$$S(t) = S(t-1) \times \left(1 - \frac{\text{events at } t}{\text{at risk at } t}\right)$$

The resulting KM curve:
- Starts at 1.0 (everyone alive at baseline)
- Steps downward at each death event
- Remains flat when only censored observations occur
- The **median survival time** is the point where the curve crosses 0.50

### 3.3 The Log-Rank Test

The **log-rank test** compares KM curves between two or more groups.

**Framingham research question:** Does survival time differ between smokers and non-smokers?

- H₀: The survival curves for smokers and non-smokers are identical
- H₁: The survival curves differ

A significant log-rank test (p < 0.05) means the two groups have statistically different survival experiences. Combined with the KM plot, this provides both the statistical test and the visual comparison.

## 🔬 Lab Manual — Chapter 8

### Objective
Part 1: Correlation and regression — does AGE predict SYSBP?
Part 2: Survival analysis — do smokers and non-smokers have different all-cause survival?

### Option A — PSPP

**Correlation/Regression:**
1. **Analyze → Correlate → Bivariate** → move `AGE`, `SYSBP`, `TOTCHOL`, `BMI` → tick Pearson + Spearman → **OK**.
2. **Analyze → Regression → Linear** → `SYSBP` as Dependent, `AGE` as Independent → Statistics → CI → Plots → `*ZRESID` vs `*ZPRED` + Normal probability plot → **OK**.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 8 Lab: Correlation, Regression, and Survival
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")
library(survival)

# ══ Part 1: Correlation ═══════════════════════════════

# Scatterplot: AGE vs SYSBP
plot(fram_data$AGE, fram_data$SYSBP,
     main = "Age vs Systolic Blood Pressure",
     xlab = "Age (years)", ylab = "SYSBP (mmHg)",
     pch = 16, col = "#2166AC", cex = 0.7, alpha = 0.6)

# Pearson's r
cor.test(fram_data$AGE, fram_data$SYSBP, method = "pearson")

# Spearman's rho (for CIGPDAY — skewed)
cor.test(fram_data$CIGPDAY, fram_data$TOTCHOL, method = "spearman")

# Correlation matrix
cor(fram_data[, c("AGE","SYSBP","DIABP","TOTCHOL","BMI","GLUCOSE")],
    use = "complete.obs")

# ══ Part 2: Simple linear regression ═══════════════════

model <- lm(SYSBP ~ AGE, data = fram_data)
summary(model)

# Coefficients and CI
cat("Intercept (β₀):", round(coef(model)[1], 3), "\n")
cat("Slope (β₁):    ", round(coef(model)[2], 3), "mmHg/year\n")
cat("R-squared (R²):", round(summary(model)$r.squared, 4), "\n")
confint(model)

# Prediction: expected SYSBP for a 55-year-old
predict(model, newdata = data.frame(AGE = 55), interval = "prediction")

# Diagnostic plots
par(mfrow = c(2,2)); plot(model); par(mfrow = c(1,1))

# Add regression line to scatterplot
abline(model, col = "#C00000", lwd = 2)

# ══ Part 3: Survival analysis ══════════════════════════

# Code smoking status
fram_data$SMOKE_LABEL <- factor(fram_data$CURSMOKE, levels=c(0,1),
  labels=c("Non-smoker","Smoker"))

# Create survival object
surv_obj <- Surv(time  = fram_data$TIMEDTH,
                 event = fram_data$DEATH)

# Kaplan-Meier curves by smoking status
km_fit <- survfit(surv_obj ~ SMOKE_LABEL, data = fram_data)
summary(km_fit)$table   # Median survival time per group

# Plot KM curves
plot(km_fit,
     main = "Kaplan-Meier Survival Curves by Smoking Status",
     xlab = "Time (days)", ylab = "Survival Probability",
     col  = c("#2166AC","#C00000"),
     lwd  = 2, conf.int = TRUE)
legend("bottomleft",
       legend = c("Non-smoker","Smoker"),
       col    = c("#2166AC","#C00000"), lwd = 2)

# Log-rank test
logrank_test <- survdiff(surv_obj ~ SMOKE_LABEL, data = fram_data)
print(logrank_test)
cat("Log-rank p-value:", round(1 - pchisq(logrank_test$chisq, df=1), 4), "\n")
```

**What to examine:**
- **Regression:** What is β₁? For each year of age, how much does predicted SYSBP increase? Is R² close to 0.15–0.20 (age explains ~15–20% of BP variation)?
- **Diagnostic plots:** Do residuals show a pattern? Is the Q-Q plot approximately diagonal?
- **KM curves:** Do smokers' survival curves separate from non-smokers' over time? Which group has lower median survival?
- **Log-rank test:** Is p < 0.05? Does smoking significantly reduce survival?

### 🧪 Test Your Knowledge

**(a)** Run a regression with `TOTCHOL` as outcome and `AGE` as predictor. Write the regression equation and predict TOTCHOL for a 60-year-old.

**(b)** Run KM analysis comparing survival between those with CHD at baseline (`PREVCHD=1`) and those without. State H₀ and H₁ for the log-rank test, run it, and interpret.

````{dropdown} Show Solution
```r
# (a) Regression: TOTCHOL ~ AGE
model_chol <- lm(TOTCHOL ~ AGE, data = fram_data)
summary(model_chol)
b0 <- coef(model_chol)[1]
b1 <- coef(model_chol)[2]
cat("Equation: TOTCHOL =", round(b0,1), "+", round(b1,3), "* AGE\n")
pred_60 <- b0 + b1 * 60
cat("Predicted TOTCHOL at age 60:", round(pred_60,1), "mg/dL\n")

# (b) KM with PREVCHD
fram_data$PREVCHD_LABEL <- factor(fram_data$PREVCHD, levels=c(0,1),
  labels=c("No prior CHD","Prior CHD"))

surv2 <- Surv(fram_data$TIMEDTH, fram_data$DEATH)
km2   <- survfit(surv2 ~ PREVCHD_LABEL, data = fram_data)
plot(km2,
     main = "Survival by Prevalent CHD at Baseline",
     col = c("#2166AC","#C00000"), lwd=2)
legend("bottomleft", legend=c("No prior CHD","Prior CHD"),
       col=c("#2166AC","#C00000"), lwd=2)

# H₀: survival curves are identical for those with and without prior CHD
# H₁: survival curves differ
lr2 <- survdiff(surv2 ~ PREVCHD_LABEL, data = fram_data)
print(lr2)
# Expected: patients with prior CHD have significantly worse survival (p < 0.05)
```
````

## Key Terms

| Term | Definition |
|---|---|
| **Pearson's r** | Strength and direction of linear association (−1 to +1). Sensitive to outliers. |
| **Spearman's rho** | Rank-based correlation. Use for skewed or ordinal data. |
| **Simple linear regression** | Models ŷ = β₀ + β₁x. β₁ = change in y per unit increase in x. |
| **R² (R-squared)** | Proportion of variance in y explained by x. R² = r² for simple linear regression with one predictor. |
| **Censored observation** | Participant whose event has not yet occurred by end of follow-up. Time recorded but outcome unknown. |
| **Kaplan-Meier estimator** | Non-parametric estimate of survival probability over time, correctly handling censoring. |
| **Log-rank test** | Compares KM survival curves between groups. H₀ = curves are identical. |

## Review Questions

1. Run `cor.test(fram_data$AGE, fram_data$SYSBP)` in R. Report r, p-value, and 95% CI. Write a one-paragraph interpretation including correlation vs causation.

2. Fit `lm(SYSBP ~ AGE)`. Write the regression equation. Predict SYSBP for participants aged 40 and 65. Is it valid to predict for a participant aged 25? Explain.

3. Explain why OLS regression (`lm(TIMEDTH ~ CURSMOKE)`) would give a biased, invalid answer for the question "does smoking reduce survival time?" What is the correct method?

4. Run the KM analysis comparing survival by smoking status. Describe the curves: which group has worse survival? What is the approximate median survival time for each group?

5. The log-rank test returns $\chi^2(1) = 3.8$, $p = 0.051$. Interpret this result. At $\alpha = 0.05$, what do you conclude? What would a power analysis suggest about this result?

```{admonition} Key Takeaways
:class: tip

- **Pearson's r** (−1 to +1): strength and direction of linear association.
- **Spearman's rho**: use for skewed data (CIGPDAY) or ordinal variables (EDUC).
- **Regression** $\hat{y} = \beta_0 + \beta_1x$: $\beta_1$ = change in $y$ per unit $x$. $R^2$ = proportion of variance explained.
- **Do NOT use OLS on censored survival data** — use Kaplan-Meier + log-rank test.
- **KM curve**: steps down at each death; median survival = time where curve crosses 0.50.
- **Log-rank test**: tests whether two KM curves are statistically different.
```

*Next: **Chapter 9 — The Full Picture** consolidates all methods and provides examination preparation.*

---

> Part III — Comparing More Than Two Groups
