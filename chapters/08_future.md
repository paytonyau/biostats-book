# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 8: Reading the Future
## *Correlation and Simple Linear Regression*

> **Course:** HS 4510 Biostatistics | **Unit:** 8 | **Part:** III — Complex Connections
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Interpret **Pearson's r** using magnitude and direction, and test its statistical significance
- State the assumptions of simple linear regression and check them using diagnostic plots
- Interpret the **regression equation** ŷ = β₀ + β₁x, including slope, intercept, and R²
- Make a prediction with a 95% prediction interval and explain the danger of extrapolation
```


---

## Introduction: From Description to Prediction

Up to this point, the tools in your statistical toolkit have been built for categorising and comparing. You have described a distribution with means, medians, and standard deviations. You have tested whether an outbreak was real with probability distributions. You have tested whether specific groups differ from one another with t-tests, ANOVA, and the Chi-Square test.

All of these are powerful, necessary skills. But they share a common characteristic: they look at fixed categories and ask whether those categories differ.

Epidemiology has a higher calling: **prediction**.

If a cholera outbreak strikes London again and you know that a family lives exactly 150 metres from a contaminated well, can you make a specific numerical forecast of how many people in that household are likely to fall ill? If a new patient arrives at a hypertension clinic and you know their age, can you estimate what their systolic blood pressure is likely to be? If a public health authority wants to model the relationship between vaccination coverage in a district and the expected number of measles cases, what mathematical framework should they use?

To answer these questions, we must move beyond comparing discrete categories and begin describing the **continuous relationship between two moving variables** — how one changes as the other changes, and whether that relationship is strong enough to carry predictive weight.

This chapter introduces the two complementary tools for doing this: **Pearson's correlation coefficient**, which measures the strength and direction of the relationship, and **simple linear regression**, which quantifies that relationship as an equation and uses it to generate predictions.

---

## Section 1: Pearson's Correlation Coefficient (r)

### 1.1 What Correlation Measures

When two continuous variables tend to move together in a systematic way, we say they are **correlated**. The direction and consistency of that co-movement is what Pearson's r captures.

- If one variable increases and the other also tends to increase, the correlation is **positive**. Example: as patient age increases, systolic blood pressure tends to increase.
- If one variable increases and the other tends to decrease, the correlation is **negative**. Example: as distance from the Broad Street pump increases, the number of cholera deaths in a household tends to decrease.
- If the two variables show no systematic relationship — knowing one tells you nothing about the other — the correlation is approximately **zero**.

Pearson's r forces the result onto a fixed, bounded scale. This is one of its most valuable properties: regardless of the original units of measurement, r always falls between −1 and +1.

---

### 1.2 The Pearson Formula

$$r = \frac{\sum[(x_i - \bar{x})(y_i - \bar{y})]}{\sqrt{\sum(x_i - \bar{x})^2 \times \sum(y_i - \bar{y})^2}}$$

The formula computes, for every observation, the product of its deviation from the x-mean and its deviation from the y-mean. When both deviations are consistently in the same direction (both positive, or both negative), the numerator is large and positive — a positive r. When the deviations are consistently in opposite directions, the numerator is large and negative — a negative r. When the deviations are random with no pattern, the positive and negative products cancel out — r approaches zero.

In practice, both PSPP and R compute Pearson's r instantly. What matters for a health sciences practitioner is **reading the result correctly**.

---

### 1.3 The Correlation Scale

| r value | Strength and direction | What it looks like in Dr Snow's data |
|:---:|---|---|
| **r = −1.0** | Perfect negative | Every extra metre from the pump corresponds to exactly fewer deaths. A flawless straight line sloping downward with no scatter whatsoever. |
| **−0.7 to −0.9** | Strong negative | A clear protective effect of distance. Points are scattered but the downward trend is unmistakeable. |
| **−0.3 to −0.6** | Moderate negative | Distance matters, but other factors — immune status, household hygiene, the practice of boiling water — also play a measurable role. |
| **r ≈ 0** | No correlation | Distance and deaths are essentially unrelated — the scatterplot is a formless cloud with no discernible direction. |
| **+0.3 to +0.6** | Moderate positive | A weak upward trend — closer proximity to the pump associates with more cases, but inconsistently. |
| **+0.7 to +0.9** | Strong positive | Proximity strongly and consistently predicts illness burden. |
| **r = +1.0** | Perfect positive | A flawless straight line sloping upward. Biologically impossible — real data always contains some unexplained variation. |

> **Reading the Snow dataset:**
>
> If Dr Snow computed r = −0.75 between `Distance_Metres` and `Household_Deaths`, he would have strong negative correlation: every additional metre a household sits from the Broad Street pump acts as a measurable protective shield. The strength of that r-value — well into the "strong" range — would represent mathematically compelling evidence before a single formal hypothesis test was run. This is one reason correlation is often the first exploratory step in an epidemiological investigation: it immediately quantifies whether a suspected relationship is worth pursuing with more complex modelling.

---

### 1.4 Testing Whether r is Statistically Significant

Pearson's r is a sample statistic — an estimate of the true population correlation (ρ, "rho"). Just as a sample mean might differ from the population mean by chance, a sample r might differ from the true ρ = 0 (no correlation) purely due to sampling variation, especially in small samples.

To test whether an observed r could plausibly have arisen by chance, we apply a hypothesis test:

- **H₀:** ρ = 0 (there is no linear correlation in the population)
- **Hₐ:** ρ ≠ 0 (a linear correlation exists in the population)

The test statistic is:

$$t = \frac{r\sqrt{n - 2}}{\sqrt{1 - r^2}}$$

with df = n − 2. Both PSPP and R report the p-value for this test automatically alongside r. If p < 0.05, the correlation is statistically significant — unlikely to be a chance artefact of sampling.

> **⚠️ Statistical significance and practical significance are not the same**
>
> With a large enough sample, even a trivially weak correlation can achieve statistical significance. A correlation of r = +0.08 in a dataset of n = 1,000 may produce p = 0.01 — but r = 0.08 explains less than 1% of the variance in y. Always report both r (to convey practical strength) and the p-value (to convey statistical reliability). Neither alone tells the complete story.

---

### 1.5 Assumptions of Pearson's Correlation

Before computing Pearson's r, verify the following:

| Assumption | What it requires | How to check it |
|---|---|---|
| **Both variables are continuous** | Both x and y are interval or ratio level. | Level of measurement classification (Chapter 1). |
| **Linear relationship** | The relationship between x and y follows a straight-line pattern. | Scatterplot — a curved or non-monotonic pattern violates this assumption. |
| **Approximate bivariate Normality** | Both variables are approximately Normally distributed. | Histogram and Shapiro-Wilk test on each variable (Chapter 4). |
| **No severe outliers** | Extreme outliers disproportionately distort r. | Scatterplot — look for isolated points far from the main cluster. |

> **When assumptions are violated:** If the relationship is monotonic but not linear (e.g., a curved association), or if data are ordinal, use **Spearman's rank correlation** (rs) instead of Pearson's r. Spearman's ranks the data and computes correlation on the ranks, making it robust to non-linearity and non-Normality. In R: `cor.test(x, y, method = "spearman")`.

---

## Section 2: The Golden Rule — Correlation Does Not Imply Causation

### 2.1 The Miasma Warning

This is the single most important interpretive principle in epidemiology. A large r, even one that is highly statistically significant, does not establish that one variable causes the other. It establishes only that the two variables are associated — that they move together in the data.

The history of public health provides an instructive cautionary tale directly from the 1854 outbreak.

The miasma theorists had their own correlation. People living at lower elevations in London had higher cholera mortality. They computed the numbers, found a strong relationship, and concluded that heavy, toxic air settled in the lowlands and poisoned the residents living there. The mathematics was not wrong — the correlation was real. The causal interpretation was entirely wrong.

London's lowest-lying neighbourhoods happened to border the heavily polluted River Thames — which was simultaneously the city's primary source of drinking water and the primary receptor of its sewage. Elevation did not cause cholera. The River Thames was a **confounding variable**, silently associated with both the low altitude and the contaminated water supply, creating an apparent causal link between two variables that had no direct relationship with each other.

Had the miasma theorists understood the logic of confounding, they might have asked the right question much earlier. Dr Snow's genius was precisely that he did ask it — and then designed an investigation that separated the effect of water source from the effect of elevation.

---

### 2.2 The Confounding Variable

A **confounding variable** (or confounder) is a third variable that:
1. Is associated with the predictor (x)
2. Is associated with the outcome (y)
3. Is **not** on the causal pathway from x to y — it is an independent cause of y, not a mediator

When a confounder is present and uncontrolled, it creates the appearance of a direct relationship between x and y that may be entirely spurious — or it may mask a genuine relationship that would otherwise be visible.

**Diagram of confounding:**

```
       Elevation (x)  ——→  Cholera deaths (y)
              ↑                    ↑
              |                    |
         [Proximity to Thames — the confounder]
```

The arrow from x to y appears in the data, but it is not a direct causal arrow — it is an artefact of both variables being driven by the hidden third factor.

---

### 2.3 Contemporary Examples of Spurious Correlations

The principle extends well beyond historical examples. Spurious correlations appear regularly in health sciences research when confounders are not identified and controlled:

- **Ice cream sales and drowning rates** are positively correlated. The confounder is summer temperature — hot weather increases both ice cream consumption and swimming.
- **Shoe size and reading ability in children** are positively correlated. The confounder is age — older children have larger feet and read better.
- **Hospital admission rate and mortality rate** may be positively correlated across hospitals. The confounder is patient severity — hospitals that admit sicker patients have both higher admission rates and higher mortality, not because admission causes death.

> **How to control for confounding:**
> - **Randomisation** (in experimental studies) — if participants are randomly assigned, confounders are distributed equally across groups by chance.
> - **Restriction** — limit the study to participants with similar values on the confounder (e.g., only studying one age group).
> - **Matching** — pair cases and controls on the confounder's value.
> - **Statistical adjustment** — include the confounder as an additional predictor in a multiple regression model (introduced in more advanced courses).
>
> Identifying confounders requires both statistical literacy and qualitative knowledge of the biological and social mechanisms underlying the data. No statistical test automatically identifies all confounders — this is one of the reasons epidemiology requires scientific judgement, not merely algorithmic processing.

---

## Section 3: Simple Linear Regression

### 3.1 From Association to Prediction

Pearson's r tells you *whether* a linear relationship exists and *how strong* it is. It does not tell you the *precise mathematical form* of the relationship, and it cannot generate a numerical prediction.

Simple linear regression goes further. It fits a straight line through the cloud of data points — the **line of best fit** — and expresses that line as an equation. Once you have the equation, you can substitute any value of x and obtain a specific predicted value of y.

---

### 3.2 Correlation vs. Regression — Choosing the Right Tool

| | Pearson's Correlation (r) | Simple Linear Regression (β, R²) |
|---|---|---|
| **Primary question** | How strongly are x and y related? | Given a value of x, what does y equal? |
| **Output** | A single number between −1 and +1 | An equation: y = β₀ + β₁x, plus R² |
| **Symmetric?** | Yes — r(x, y) = r(y, x) | No — x predicts y; the roles are fixed |
| **Enables prediction?** | No | Yes |
| **Use when** | You want to quantify the strength and direction of an association | You want to describe the relationship precisely or forecast a specific value |

---

### 3.3 The Line of Best Fit — Ordinary Least Squares

When we plot a scatterplot of `Distance_Metres` (x-axis) against `Household_Deaths` (y-axis), the data points form a cloud with a downward trend — more deaths near the pump, fewer deaths far away. Simple linear regression calculates the single straight line through that cloud that **minimises the sum of squared residuals** — the total squared vertical distance from every data point to the line.

This fitting method is called **Ordinary Least Squares (OLS)**. It is the mathematical foundation of simple linear regression and guarantees, under its assumptions, that the resulting line is the best unbiased linear estimate of the true relationship.

A **residual** is the difference between the actual observed value of y for a given observation and the value the regression line predicts for that same x:

$$\text{residual}_i = y_i - \hat{y}_i = \text{observed} - \text{predicted}$$

A residual of zero means the data point falls exactly on the line. A large positive residual means the actual value was much higher than predicted; a large negative residual means the actual value was much lower than predicted. The pattern of residuals is an important diagnostic tool for assessing whether the regression model is appropriate — Section 3.7 addresses this.

---

### 3.4 The Regression Equation

Every straight line can be described by a single equation. In simple linear regression, this is:

$$\hat{y} = \beta_0 + \beta_1 x$$

(The hat symbol over y — "y-hat" — denotes a predicted value, distinguishing it from the observed value y.)

| Symbol | Name | What it means for our dataset |
|:---:|---|---|
| **ŷ** | Predicted outcome | The predicted number of `Household_Deaths` for a given distance. |
| **x** | Predictor (independent variable) | The known value of `Distance_Metres` for the household. |
| **β₁** | Slope | How much `Household_Deaths` changes for every one-metre increase in distance. A negative β₁ means deaths decrease as distance increases. |
| **β₀** | Intercept | The predicted number of deaths when distance = 0 — that is, for a household located directly at the pump. Often not biologically meaningful when x = 0 is outside the realistic range of the data. |
| **R²** | Coefficient of Determination | The proportion of variance in `Household_Deaths` explained by `Distance_Metres` alone. |

---

### 3.5 Interpreting the Slope (β₁)

The slope is the most important single number in a simple regression output. It carries three pieces of information simultaneously:

1. **Direction** — the sign tells you whether the relationship is positive (rising) or negative (falling).
2. **Magnitude** — the absolute value tells you how much y changes per unit of x.
3. **Units** — β₁ is always expressed in the units of y per unit of x (e.g., deaths per metre).

**Example:** If the regression of `Household_Deaths` on `Distance_Metres` yields β₁ = −0.018, the interpretation is:

> "For every additional metre a household is located from the Broad Street pump, the expected number of deaths in that household decreases by 0.018. Equivalently, for every 100 additional metres of distance, the expected household death toll decreases by approximately 1.8."

This is the operational translation of Snow's spatial map into a precise, quantified relationship. The number 0.018 is small per metre, but its accumulated effect over the range of distances in the data (from near the pump to several hundred metres away) accounts for a large and meaningful difference in predicted outcomes.

> **Is β₁ statistically significant?**
>
> Just as Pearson's r is tested against the hypothesis ρ = 0, the slope β₁ is tested against the hypothesis that the true population slope is zero (β₁ = 0). A slope of zero would mean x has no linear relationship with y whatsoever. The regression output from both PSPP and R provides a t-statistic and p-value for each coefficient. If p < 0.05 for β₁, the slope is statistically significant — the predictor x explains a significant portion of the variance in y.

---

### 3.6 The Coefficient of Determination (R²)

R² answers the most practically important question about a regression model: **how much of the story does this model actually tell?**

$$R^2 = r^2$$

For simple linear regression with one predictor, R² is literally the square of Pearson's r. It represents the **proportion of the total variance in y that is explained by the linear relationship with x**. The remainder (1 − R²) is the proportion left unexplained — attributable to other variables not in the model, to measurement error, or to random biological variation.

**Interpreting R² — multiply by 100 to express as a percentage:**

| R² value | Descriptive label | Interpretation in the Snow context |
|:---:|:---:|---|
| **< 0.10** | Very weak | Distance barely predicts household deaths — other factors dominate completely. |
| **0.10 – 0.29** | Weak | Distance explains some variation, but the model has limited predictive utility. |
| **0.30 – 0.59** | Moderate | Distance is a meaningful but imperfect predictor — a useful model for initial investigation. |
| **0.60 – 0.79** | Strong | Distance is a powerful predictor of household mortality — the model explains the majority of observed variation. |
| **≥ 0.80** | Very strong | Proximity to the pump explains the overwhelming majority of the variation in deaths. |

**Worked example:**

If r = −0.85 between `Distance_Metres` and `Household_Deaths`, then R² = (−0.85)² = **0.72**.

Interpretation: *"Distance from the Broad Street pump explains 72% of the variation in household death counts across the dataset. The remaining 28% of variation is attributable to other factors not captured by this model — individual immune status, whether residents boiled their water, nutritional health, and other unmeasured variables."*

> **The 28% is not a failure of the model — it is an accurate characterisation of how much the model does not explain.** A good scientist reports R² honestly, neither dismissing a useful 72% as "only" a partial explanation nor overstating a model's completeness. R² is a precision tool for communicating the limits of the model.

---

### 3.7 Assumptions of Simple Linear Regression

Simple linear regression carries stronger assumptions than Pearson's correlation, because it is making stronger claims — not just about association, but about the mathematical form and predictive structure of the relationship.

| Assumption | What it requires | How to check it |
|---|---|---|
| **Linearity** | The relationship between x and y is linear. | Scatterplot of x vs. y. Plot of residuals vs. fitted values — should show no pattern. |
| **Independence of residuals** | Each observation's residual is independent of all others. | Study design review. For time-series data, check for autocorrelation. |
| **Homoscedasticity** | The variance of the residuals is approximately constant across all values of x (equal spread). | Residuals vs. fitted values plot — should show approximately uniform vertical spread. A funnel pattern indicates heteroscedasticity (a violation). |
| **Normality of residuals** | The residuals are approximately Normally distributed. | Histogram of residuals; Q-Q plot of residuals. Note: it is the *residuals*, not the raw variables, that must be Normally distributed. |
| **No severe multicollinearity** | (Relevant for multiple regression, not simple regression with one predictor.) | N/A for simple regression. |

> **Checking residual plots in R:**
> After fitting a model with `lm()`, run `plot(model)`. R produces four diagnostic plots automatically. The most important are Plot 1 (Residuals vs. Fitted — checks linearity and homoscedasticity) and Plot 2 (Normal Q-Q — checks normality of residuals). When these plots look clean, the regression assumptions are broadly satisfied.

---

### 3.8 Making a Numerical Prediction

The practical payoff of regression is the ability to substitute a new x value into the equation and obtain a specific predicted y value.

**Our regression equation** (from fitting the model to the Snow dataset):

$$\hat{\text{Household\_Deaths}} = 4.2 - 0.018 \times \text{Distance\_Metres}$$

**Prediction for a household 150 metres from the pump:**

$$\hat{y} = 4.2 - (0.018 \times 150) = 4.2 - 2.7 = \mathbf{1.5 \text{ deaths}}$$

A household 150 metres from the Broad Street pump is predicted to suffer approximately 1 to 2 deaths — far fewer than the 4 deaths predicted for a household located directly at the pump (Distance = 0, where ŷ = β₀ = 4.2).

**Prediction for a household 200 metres from the pump:**

$$\hat{y} = 4.2 - (0.018 \times 200) = 4.2 - 3.6 = \mathbf{0.6 \text{ deaths}}$$

At 200 metres, the model predicts fewer than one death per household on average — consistent with Snow's empirical observation that mortality fell dramatically with distance from the pump.

> **⚠️ The limits of prediction — extrapolation**
>
> The regression equation should only be used to make predictions within the range of x values present in the original dataset. This is called **interpolation** and is reliable. Making predictions for x values *outside* this range — called **extrapolation** — assumes that the linear relationship continues indefinitely in both directions, which is rarely biologically plausible.
>
> For example, substituting x = 2,000 metres into our equation gives ŷ = 4.2 − 36 = −31.8 deaths — a biologically impossible negative value. The relationship between distance and deaths does not remain linear across arbitrarily large distances; at some point, distance becomes irrelevant because the person is no longer drinking from the pump at all. Always report the range of x values used to fit the model and caution against extrapolation beyond that range.

---

## 🔬 Lab Manual — Chapter 8

### Objective

**Part 1:** Compute Pearson's r between `Distance_Metres` and `Household_Deaths`. Test whether the correlation is statistically significant and report the result with correct interpretation.

**Part 2:** Fit a simple linear regression model predicting `Household_Deaths` from `Distance_Metres`. Interpret β₀, β₁, and R² from the output. Plot the scatterplot with the regression line overlaid. Make a numerical prediction for a specific distance value.

---

### Option A — PSPP

#### Part 1 — Pearson's Correlation

1. Open your `snow_1854_outbreak.sav` file.
2. Go to **Analyze → Correlate → Bivariate**.
3. Move both **`Distance_Metres`** and **`Household_Deaths`** into the **Variables** box.
4. Ensure that **Pearson** is ticked under *Correlation Coefficients*.
5. Ensure that **Flag significant correlations** is ticked.
6. Click **OK**.

**Reading the output:**

The output is a correlation matrix — a square table showing the correlation between every pair of variables. Find the cell where `Distance_Metres` and `Household_Deaths` intersect.

| Row in the cell | What it shows |
|---|---|
| Top number | Pearson's r — the correlation coefficient |
| Middle number | Sig. (2-tailed) — the p-value for H₀: ρ = 0 |
| Bottom number | N — the sample size used |

A double asterisk (**) next to r indicates p < 0.01. A single asterisk (*) indicates p < 0.05.

---

#### Part 2 — Linear Regression

1. Go to **Analyze → Regression → Linear**.
2. Move **`Household_Deaths`** into the **Dependent** box (this is y — the outcome).
3. Move **`Distance_Metres`** into the **Independent(s)** box (this is x — the predictor).
4. Click **Statistics**. Tick **Estimates** and **Model fit**. Click **Continue**.
5. Click **OK**.

**Reading the output:**

PSPP produces three tables. Work through them in order:

| Table | What to find |
|---|---|
| **Model Summary** | The **R Square** column — this is R². Multiply by 100 for the percentage of variance explained. |
| **ANOVA** | The **Sig.** column for the regression row — this tests whether the overall model is statistically significant (equivalent to testing β₁ ≠ 0). |
| **Coefficients** | The **B** column: the top row (Constant) is β₀; the bottom row (Distance_Metres) is β₁. The **Sig.** column shows whether each coefficient is statistically significant. |

---

### Option B — R / RStudio

```r
# ---------------------------------------------------------
# Chapter 8 Lab: Correlation and Simple Linear Regression
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded.
# library(HistData)
# cholera_data <- Snow.deaths

# ── PART 1: PEARSON'S CORRELATION ─────────────────────────

# Step 2: Inspect the relationship visually before computing r.
# Always plot first — correlation assumes linearity,
# and a scatterplot will reveal whether this holds.
plot(cholera_data$Distance_Metres,
     cholera_data$Household_Deaths,
     main = "Distance vs. Household Deaths — Scatterplot",
     xlab = "Distance from Broad Street Pump (Metres)",
     ylab = "Deaths per Household",
     pch  = 19,
     col  = "steelblue")

# Step 3: Run the Pearson's correlation test.
# cor.test() returns r, t-statistic, df, p-value, and 95% CI for rho.
cor_result <- cor.test(cholera_data$Distance_Metres,
                       cholera_data$Household_Deaths,
                       method = "pearson")
cor_result

# Step 4: Extract and display the key values.
cat("=== Pearson's Correlation ===\n")
cat("r =", round(cor_result$estimate, 3), "\n")
cat("t-statistic:", round(cor_result$statistic, 3),
    "| df:", cor_result$parameter, "\n")
cat("p-value:", round(cor_result$p.value, 4), "\n")
cat("95% CI for rho: [",
    round(cor_result$conf.int[1], 3), ",",
    round(cor_result$conf.int[2], 3), "]\n")

# Step 5: For comparison, run Spearman's r.
# Use this if the scatterplot suggests non-linearity
# or if the Shapiro-Wilk test flags non-Normality.
cor.test(cholera_data$Distance_Metres,
         cholera_data$Household_Deaths,
         method = "spearman")

# ── PART 2: SIMPLE LINEAR REGRESSION ─────────────────────

# Step 6: Fit the linear model.
# lm() fits the line of best fit by Ordinary Least Squares.
# Formula: Household_Deaths ~ Distance_Metres
# means: "Household_Deaths is predicted by Distance_Metres"
model <- lm(Household_Deaths ~ Distance_Metres,
            data = cholera_data)

# Step 7: Examine the full model summary.
summary(model)

# In the output, locate:
# Coefficients:
#   (Intercept)       <- this is beta_0
#   Distance_Metres   <- this is beta_1
# The Pr(>|t|) column gives the p-value for each coefficient.
#
# Multiple R-squared:  <- this is R^2
# F-statistic: ... p-value: ... <- overall model significance

# Step 8: Extract and display the key regression values.
b0 <- coef(model)["(Intercept)"]
b1 <- coef(model)["Distance_Metres"]
r2 <- summary(model)$r.squared

cat("\n=== Simple Linear Regression Results ===\n")
cat("Regression equation: Deaths =",
    round(b0, 3), "+", round(b1, 3), "* Distance\n")
cat("Intercept (beta_0):", round(b0, 3),
    "— predicted deaths at Distance = 0\n")
cat("Slope (beta_1):", round(b1, 4),
    "— deaths change per 1-metre increase in distance\n")
cat("R-squared:", round(r2, 3),
    "— i.e.", round(r2 * 100, 1), "% of variance in deaths explained\n")

# Step 9: Plot the scatterplot with the regression line overlaid.
plot(cholera_data$Distance_Metres,
     cholera_data$Household_Deaths,
     main = "Simple Linear Regression: Deaths ~ Distance",
     xlab = "Distance from Broad Street Pump (Metres)",
     ylab = "Deaths per Household",
     pch  = 19,
     col  = "steelblue")
abline(model, col = "red", lwd = 2)

# Optional: add a legend
legend("topright",
       legend = paste0("y = ", round(b0, 2),
                       " + ", round(b1, 4), "x  |  R² = ",
                       round(r2, 2)),
       col = "red", lwd = 2, bty = "n")

# Step 10: Make a specific prediction.
# Predict deaths for a household 150 metres from the pump.
new_data <- data.frame(Distance_Metres = c(50, 100, 150, 200, 300))
predictions <- predict(model,
                       newdata   = new_data,
                       interval  = "prediction",  # 95% prediction interval
                       level     = 0.95)

cat("\n=== Predicted Household Deaths by Distance ===\n")
print(cbind(new_data, round(predictions, 2)))
# 'fit' = point prediction; 'lwr' and 'upr' = 95% prediction interval

# Step 11: Check regression diagnostic plots.
# These four plots diagnose whether the regression assumptions hold.
par(mfrow = c(2, 2))   # arrange four plots in a 2x2 grid
plot(model)
par(mfrow = c(1, 1))   # reset to single-plot layout

# Plot 1 (Residuals vs Fitted): checks linearity and homoscedasticity.
#   A random horizontal scatter is good; a pattern or funnel is bad.
# Plot 2 (Normal Q-Q): checks normality of residuals.
#   Points close to the diagonal line indicate normality.
# Plot 3 (Scale-Location): another check on homoscedasticity.
# Plot 4 (Residuals vs Leverage): identifies influential outliers.
```

**What to record and report:**

For the **correlation**:

Report as: r(df) = [value], p = [value], 95% CI [lower, upper]. Describe the direction and strength using the scale table from Section 1.3.

For the **regression**:

Write the full regression equation with the actual β values substituted in. Interpret β₁ in a single plain-language sentence (e.g., "For every additional metre from the pump, predicted household deaths decrease by..."). Report R² as a percentage and describe what the remaining unexplained variance represents. Make and report the prediction for 150 metres, showing the arithmetic.

---

---

### 🧪 Test Your Knowledge

Using the Snow dataset, fit a simple linear regression predicting `Household_Deaths` from `Distance_Metres`. **Task:** (a) Write the full R code including scatterplot, `lm()`, `summary()`, and diagnostic plots. (b) Write out the regression equation using the actual coefficients. (c) Predict the expected deaths for a household 80 metres from the pump, with a 95% prediction interval. (d) State two assumptions you should check and how you checked them.

```{dropdown} Show Solution
```r
# (a) Full R code
# Scatterplot first — always plot before fitting
plot(cholera_data$Distance_Metres, cholera_data$Household_Deaths,
     main = "Household Deaths vs Distance to Pump",
     xlab = "Distance (metres)", ylab = "Deaths",
     pch = 19, col = "steelblue")

# Fit the model
model <- lm(Household_Deaths ~ Distance_Metres, data = cholera_data)
summary(model)
abline(model, col = "red", lwd = 2)

# Diagnostic plots
par(mfrow = c(2,2)); plot(model); par(mfrow = c(1,1))

# (b) Regression equation (fill in actual coefficients from summary output):
# ŷ = [β₀] + [β₁] × Distance_Metres
# Example: ŷ = 4.20 − 0.018 × Distance_Metres
# Interpretation: each additional 1m from the pump is associated with
# 0.018 fewer predicted deaths per household.

# (c) Prediction at Distance = 80m
new_data <- data.frame(Distance_Metres = 80)
predict(model, newdata = new_data, interval = "prediction")
# Output columns: fit (point estimate), lwr, upr (95% prediction interval)

# (d) Two key assumptions:
# 1. Linearity — checked via Residuals vs Fitted plot (look for no curve)
# 2. Normality of residuals — checked via Normal Q-Q plot 
#    (points should track the diagonal line closely)
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Pearson's r** | A measure of the strength and direction of the linear association between two continuous variables, ranging from −1 (perfect negative) through 0 (no association) to +1 (perfect positive). |
| **Positive correlation** | A relationship in which both variables increase together. Pearson's r is positive. |
| **Negative correlation** | A relationship in which one variable increases as the other decreases. Pearson's r is negative. |
| **Confounding variable** | A third variable associated with both the predictor (x) and the outcome (y) that creates a spurious or misleading association between them. |
| **Simple linear regression** | A statistical model describing the linear relationship between one continuous predictor (x) and one continuous outcome (y), expressed as the equation ŷ = β₀ + β₁x. |
| **Slope (β₁)** | The change in the predicted value of y for every one-unit increase in x. Carries sign (direction) and magnitude. |
| **Intercept (β₀)** | The predicted value of y when x = 0 — the point where the regression line crosses the y-axis. Not always biologically interpretable. |
| **R² (Coefficient of Determination)** | The proportion of the total variance in y explained by the linear relationship with x. Computed as r². Expressed as a percentage by multiplying by 100. |
| **Residual** | The difference between an observed y value and the value predicted by the regression line for the same x (residual = observed − predicted). |
| **Line of best fit** | The straight line through a scatterplot that minimises the sum of squared residuals. |
| **Ordinary Least Squares (OLS)** | The standard method for fitting a regression line, minimising the total squared vertical distance between data points and the line. |
| **Interpolation** | Using the regression equation to predict y for x values within the range of the original data — statistically reliable. |
| **Extrapolation** | Using the regression equation to predict y for x values outside the range of the original data — potentially unreliable and misleading. |

---

## Review Questions

1. A researcher calculates r = +0.23 between daily sugar intake and fasting blood glucose in a sample of n = 40 patients. Using the r-scale table from Section 1.3, describe the strength and direction of this correlation. The accompanying p-value is 0.15. What does this p-value tell you, and how does it affect your interpretation of the r-value?

2. A public health study finds a strong positive correlation (r = +0.81) between the number of swimming pools per capita in a neighbourhood and the local drowning rate. A community group argues that swimming pools should be closed to reduce drowning deaths. Using the principle of confounding, explain why this conclusion is premature. Identify a plausible confounding variable and describe how it produces the observed correlation without pools directly causing drowning deaths.

3. Using the regression equation `Household_Deaths = 4.2 − 0.018 × Distance_Metres`, predict the expected number of deaths for a household 200 metres from the pump. Show all arithmetic. Then interpret the intercept β₀ = 4.2 in plain language — what does this value represent, and is it biologically meaningful?

4. A regression model predicting hospital readmission within 30 days from patient age alone returns β₁ = 0.04 and R² = 0.31. Write two sentences — one interpreting β₁ and one interpreting R² — suitable for a hospital administrator who has no statistical training.

5. If Pearson's r between two variables is −0.89, what is R²? Show the calculation. Interpret R² in a plain-language sentence. Then explain what the remaining percentage of unexplained variance represents and give one plausible example of what might account for it in a clinical dataset.

6. A clinical researcher reports: "The correlation between hours of sleep per night and post-operative recovery speed (days to discharge) is r = −0.61, p = 0.04, 95% CI for ρ [−0.81, −0.27]." Write a complete interpretation of this result covering: direction, strength, statistical significance, what the confidence interval adds to the interpretation, and a one-sentence caution about the causal interpretation of this finding.

---

```{admonition} Key Takeaways
:class: tip

- **Pearson's r:** Ranges from −1 to +1. Sign = direction. Magnitude: |r| < 0.3 weak, 0.3–0.6 moderate, > 0.7 strong.
- **Correlation ≠ Causation:** A significant r only tells you the variables co-vary. Confounders may explain the relationship.
- **Regression equation:** ŷ = β₀ + β₁x. β₁ = change in y per 1-unit increase in x. β₀ = predicted y when x = 0.
- **R²:** Proportion of variance in y explained by x. R² = r². Multiply by 100 for percentage.
- **Diagnostic plots:** Residuals vs Fitted (linearity + homoscedasticity), Normal Q-Q (residual Normality).
- **Extrapolation warning:** Using the model to predict beyond the observed range of x is unreliable.
- **In R:** `cor.test()` for r; `lm()` for regression; `predict(..., interval='prediction')` for forecasting.
```

## Chapter Summary

This chapter has moved the Biostatistics Compass from the comparison of distinct categories to the description of continuous relationships between two variables — the analytical foundation for prediction in epidemiology and clinical research.

Pearson's r summarises the strength and direction of a linear association on a bounded, universally interpretable scale. Its statistical significance can be tested, and it provides the first exploratory evidence for whether a relationship is worth modelling formally. Its critical limitation is that it establishes association only — not causation. The miasma/elevation example from 1854 Soho remains one of the most instructive illustrations in the history of science of what happens when correlation is mistaken for cause: an internally consistent but fundamentally wrong theory is retained, and people die as a consequence.

Simple linear regression extends the correlation framework into a predictive instrument. The slope β₁ quantifies exactly how much y changes per unit of x; the intercept β₀ anchors the line; and R² communicates honestly how much of the observed variance in y the model actually accounts for. Making numerical predictions by substituting x values into the regression equation is the direct application of everything the model has learnt from the data — but only within the range from which the model was trained, and only under the assumptions the method requires.

In Chapter 9, the final chapter of this course, we step back from quantitative methods to examine the qualitative and ethical dimensions of health sciences research — mixed methods, research ethics, the Belmont Report, and the problem of p-hacking — before synthesising all nine chapters in a course-wide summary.

---

*Next: **Chapter 9 — The Qualitative Bridge and Data Ethics:** Mixed Methods, Research Ethics, and P-Hacking*

---

> Part III — Complex Connections
