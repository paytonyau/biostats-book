# Chapter 4: The Laws of Chance
## *Probability Distributions and Normality Assessment*
---
> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants, baseline examination.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Describe the Normal, Binomial, and Poisson distributions and when each applies
- Apply the Empirical Rule to interpret clinical data
- Assess Normality using a histogram, Q-Q plot, and Shapiro-Wilk test
- Explain what an epidemic curve shows and how to read one
```

## Before You Begin: From Outcomes to Probabilities

Chapters 1–3 described data. Now we use mathematical models — **probability distributions** — to ask: *how likely is a given outcome?*

In cardiovascular epidemiology, this means: given a population with mean cholesterol of 235 mg/dL, how likely is it that a randomly selected participant has cholesterol above 300 mg/dL? Given a 31% 10-year CHD event rate, how surprising would it be if 45 out of a new sample of 100 patients had events? Those questions are the engine behind every hypothesis test in Chapters 5–8.

## Section 1: Three Distributions That Matter

### 1.1 Properties of the Normal Distribution

> 💡 **Plain English first:** Many biological measurements — height, blood pressure, cholesterol — cluster around a middle value and tail off symmetrically in both directions. That bell-shaped pattern is the Normal distribution.

The **Normal distribution** is completely defined by two parameters: the mean (μ) and the standard deviation (σ).

**The Empirical Rule:**
- 68% of observations fall within μ ± 1σ
- 95% fall within μ ± 1.96σ
- 99.7% fall within μ ± 3σ

**Clinical application — Framingham SYSBP:**
With μ = 132 mmHg and σ = 22 mmHg:
- 68% of participants have SYSBP between 110 and 154 mmHg
- 95% have SYSBP between 89 and 175 mmHg
- A participant with SYSBP = 190 mmHg sits (190−132)/22 = 2.6 SDs above the mean — in the top 0.5% of the distribution

**Z-score:**
$$z = \frac{x - \mu}{\sigma}$$

A z-score expresses how many SDs an observation lies from the mean, enabling comparisons across variables with different units.

```{figure} ../images/ch04_normal_distribution.png
:name: fig-normal
:width: 80%
:align: center

**Figure 4.1** The Normal distribution with the Empirical Rule. In the Framingham cohort, systolic blood pressure is approximately Normal with μ ≈ 132 mmHg and σ ≈ 22 mmHg. The 68% band spans roughly 110–154 mmHg; the 95% band spans roughly 89–175 mmHg.
```

### 1.2 The Binomial Distribution

> 💡 **Plain English first:** The Binomial counts how many "successes" occur in a fixed number of yes/no trials. Each trial has the same probability of success.

**When to use it:** Fixed number of independent binary trials (n), constant probability of the event (p).

**Framingham context:** 31% of the 500 participants had a CHD event during follow-up. If we take a new random sample of 50 Framingham-like participants, what is the probability that exactly 15 have CHD events?

$$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$$

With n=50, p=0.31: $P(X=15) = \binom{50}{15}(0.31)^{15}(0.69)^{35}$

The Binomial answers this precisely without assuming anything about the underlying biology.

### 1.3 The Poisson Distribution

> 💡 **Plain English first:** The Poisson counts how many times a rare event occurs per unit time or space.

**When to use it:** Rare, independent events occurring at a constant average rate (λ) per unit.

**Cardiovascular epidemiology context:** If heart attacks occur at an average rate of λ = 2.3 per 1,000 person-years in this cohort, the Poisson distribution describes how many heart attacks we would expect in any given year in a clinic of 200 patients.

## Section 2: The Central Limit Theorem

The **Central Limit Theorem (CLT)** is the mathematical foundation of all hypothesis testing:

> *Regardless of the shape of the population distribution, the distribution of sample means approaches Normal as sample size increases — provided n is sufficiently large (typically n ≥ 30).*

**Why this matters:** `CIGPDAY` (cigarettes per day) in the Framingham dataset is severely right-skewed — 49% of participants have 0, and a few smoke 40–60 per day. The distribution of individual values is far from Normal. But with n = 500, the CLT guarantees that the *sampling distribution of the mean* is approximately Normal. This is what makes t-tests valid even when individual data is skewed.

## Section 3: Epidemic Curves — Reading Disease Over Time

An **epidemic curve (Epi Curve)** is a histogram where the x-axis represents time and each bar represents the number of new cases in that period. Reading the shape of the curve tells you how a disease spreads.

```{figure} ../images/ch04_epidemic_curves.png
:name: fig-epicurves
:width: 80%
:align: center

**Figure 4.2** Three epidemic curve shapes: point source (single sharp peak — e.g., a contaminated food event), propagated (successive waves — person-to-person transmission), and endemic (roughly constant background rate). Cardiovascular disease is endemic — a persistent, ongoing cause of morbidity and mortality, not a single outbreak event.
```

**The Framingham connection:** Cardiovascular disease is not an outbreak — it is an epidemic in the public health sense: a disease occurring at higher-than-expected rates in a population over a sustained period. The Framingham Study was designed precisely because heart disease had become the leading cause of death in post-war America and no one yet understood why. Plotting incident CHD events by year in the Framingham cohort produces an endemic-pattern curve — a persistent baseline incidence, not a single-source spike.

## Section 4: Normality Assessment

Before applying parametric tests, assess whether the continuous outcome variable is approximately Normally distributed.

### 4.1 The Histogram

A histogram of a Normally distributed variable should appear approximately bell-shaped.

**For `SYSBP`:** Expect an approximately Normal histogram — slightly right-skewed due to hypertensive outliers.
**For `TOTCHOL`:** Similarly approximately Normal with a right tail.
**For `CIGPDAY`:** Expect a severely right-skewed histogram — spike at 0, long tail.
**For `BMI`:** Expect slight right skew — most participants in normal-to-overweight range, a few obese.

### 4.2 The Q-Q Plot

A **Q-Q plot** compares the observed distribution against what a perfect Normal distribution would predict.

- Points **along the diagonal** → approximately Normal.
- Points **curving upward at the right** → positive skew (right tail).
- Points **bowing away at both ends** → heavy tails.

```{figure} ../images/ch04_qqplot_interpretation.png
:name: fig-qqplot
:width: 95%
:align: center

**Figure 4.3** Three Q-Q plots. (a) Approximately Normal — SYSBP in the Framingham cohort would resemble this. (b) Right-skewed — CIGPDAY or GLUCOSE would show this pattern. (c) Heavy-tailed — rare in the Framingham variables but important to recognise.
```

### 4.3 The Shapiro-Wilk Test

- **H₀:** The data comes from a Normal distribution.
- **p > 0.05:** No significant departure from Normality detected.
- **p ≤ 0.05:** Significant departure from Normality.

> ⚡ **Common mistake:** With n = 500, Shapiro-Wilk flags even trivial departures from Normality as "significant." At this sample size, trust the histogram and Q-Q plot over the Shapiro-Wilk p-value. A minor, clinically irrelevant asymmetry will produce p < 0.05 purely because of sample size.

## 🔬 Lab Manual — Chapter 4

### Objective
Assess the Normality of `SYSBP`, `TOTCHOL`, `BMI`, and `CIGPDAY`. Plot an incidence-over-time chart for CHD events.

### Option A — PSPP

1. **Graphs → Chart Builder → Histogram** → drag `SYSBP` to x-axis → tick "Display normal curve" → **OK**.
2. **Analyze → Descriptive Statistics → Explore** → add `SYSBP`, `TOTCHOL`, `CIGPDAY` → under Plots tick "Normality plots with tests" → **OK**.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 4 Lab: Normality Assessment and Distributions
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")

# ── Histograms ────────────────────────────────────────
par(mfrow = c(2, 2))

hist(fram_data$SYSBP,
     main = "Systolic Blood Pressure",
     xlab = "SYSBP (mmHg)", col = "#BDD7EE", border = "white", breaks = 20)

hist(fram_data$TOTCHOL,
     main = "Total Cholesterol",
     xlab = "TOTCHOL (mg/dL)", col = "#C7E9C0", border = "white", breaks = 20)

hist(fram_data$BMI,
     main = "Body Mass Index",
     xlab = "BMI (kg/m²)", col = "#FDD0A2", border = "white", breaks = 20)

hist(fram_data$CIGPDAY,
     main = "Cigarettes Per Day",
     xlab = "CIGPDAY", col = "#FEE0D2", border = "white", breaks = 20)

par(mfrow = c(1, 1))

# ── Q-Q plots ─────────────────────────────────────────
par(mfrow = c(1, 2))
qqnorm(fram_data$SYSBP,  main = "Q-Q: SYSBP",   col="#2166AC", pch=16, cex=0.7)
qqline(fram_data$SYSBP,  col = "#C00000", lwd = 2)
qqnorm(fram_data$CIGPDAY, main = "Q-Q: CIGPDAY", col="#E6550D", pch=16, cex=0.7)
qqline(fram_data$CIGPDAY, col = "#C00000", lwd = 2)
par(mfrow = c(1, 1))

# ── Shapiro-Wilk tests ────────────────────────────────
shapiro.test(fram_data$SYSBP)
shapiro.test(fram_data$TOTCHOL)
shapiro.test(fram_data$BMI)
shapiro.test(fram_data$CIGPDAY)

# ── Incidence plot: CHD events over time ──────────────
# TIMECHD gives days to event — convert to approximate year
chd_patients <- fram_data[fram_data$ANYCHD == 1, ]
chd_year <- ceiling(chd_patients$TIMECHD / 365)
barplot(table(chd_year),
        main = "Incident CHD Events by Follow-Up Year",
        xlab = "Year of Follow-Up",
        ylab = "Number of Events",
        col = "#FEE0D2", border = "white")
```

**What to examine:**

| Variable | Expected histogram | Expected Q-Q | Expected Shapiro-Wilk |
|---|---|---|---|
| `SYSBP` | Approximately bell-shaped, slight right skew | Points close to diagonal | p may be borderline |
| `TOTCHOL` | Approximately Normal, slight right tail | Near diagonal | p may be borderline |
| `BMI` | Slight right skew | Mild curve at right | p < 0.05 likely |
| `CIGPDAY` | Severe spike at 0, long right tail | Strong S-curve departure | p << 0.001 |

### 🧪 Test Your Knowledge

`shapiro.test(fram_data$CIGPDAY)` returns p < 0.001. **(a)** State H₀ and H₁. **(b)** Interpret the result. **(c)** Does this mean you cannot run any parametric tests involving `CIGPDAY`? Explain, with reference to the CLT.

````{dropdown} Show Solution
```r
# (a) H₀: CIGPDAY comes from a Normal distribution.
#     H₁: CIGPDAY does not come from a Normal distribution.

# (b) p < 0.001: Reject H₀. CIGPDAY departs significantly from Normality —
#     consistent with the severe spike at 0 and long right tail.

# (c) NOT necessarily — with n = 500, the CLT ensures the SAMPLING
#     DISTRIBUTION OF THE MEAN is approximately Normal regardless of the
#     shape of the raw data. A t-test on the mean of CIGPDAY remains
#     approximately valid. For very small subgroups (n < 30), a
#     non-parametric alternative (e.g., Mann-Whitney U) should be used.
```
````

## Key Terms

| Term | Definition |
|---|---|
| **Normal distribution** | Symmetric, bell-shaped distribution defined by μ and σ. 95% within ±1.96σ. |
| **Binomial distribution** | Models count of successes in n binary trials with fixed probability p. |
| **Poisson distribution** | Models count of rare events at constant rate λ per unit time/space. |
| **Central Limit Theorem** | Sample means → Normal as n increases, regardless of population shape. |
| **Epidemic curve** | Histogram of new cases over time; shape reveals transmission pattern. |
| **Q-Q plot** | Quantile plot comparing observed vs theoretical Normal quantiles. |
| **Shapiro-Wilk test** | Formal test of Normality. p > 0.05 → no significant departure detected. |

## Review Questions

1. Using the Empirical Rule and the Framingham SYSBP values (mean = 132, SD = 22), what proportion of participants would you expect to have SYSBP above 176 mmHg? Is a value of 176 mmHg unusual in this cohort?

2. In the Framingham dataset, 31% of participants had a CHD event. If you drew a new sample of 80 participants from the same population, what distribution would you use to model the number of CHD events? Identify n and p.

3. Run `hist(fram_data$CIGPDAY)` in R. Describe the shape. Why is this distribution shaped the way it is, given the sample demographics?

4. Shapiro-Wilk on `SYSBP` returns p = 0.04 with n = 500. Should you conclude that SYSBP is not Normally distributed for practical purposes? Explain, referencing the CLT.

5. Explain the difference between a point-source epidemic curve and a propagated outbreak curve. Which pattern would you expect for cardiovascular disease incidence, and why?

```{admonition} Key Takeaways
:class: tip

- **Normal:** bell-shaped; 68/95/99.7% within ±1/2/3σ.
- **Binomial:** counts successes in n binary trials with fixed p.
- **Poisson:** counts rare events at rate λ per unit time/space.
- **CLT:** sample means → Normal for n ≥ 30, even if raw data is skewed.
- **Normality check:** histogram + Q-Q plot + Shapiro-Wilk. With n > 100, visual tools are more informative than Shapiro-Wilk alone.
- **Epidemic curve:** shape reveals outbreak source and transmission pattern.
```

*Next: **Chapter 5 — The Hypothesis Gamble** uses these distributions to build formal hypothesis tests.*

---

> Part II — Testing What We Think We Know
