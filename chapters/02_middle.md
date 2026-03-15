# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 2: The Middle and the Mess
## *Measures of Central Tendency and Variability*

> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants, baseline examination.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Calculate and interpret the **mean**, **median**, and **mode**
- Calculate the **range**, **variance** (with Bessel's correction), and **standard deviation**
- Choose the correct measure of central tendency based on distribution shape
- Produce and interpret descriptive statistics in PSPP and R
```

## Before You Begin: Why Summaries Matter

500 rows of blood pressure readings, cholesterol values, and smoking histories tell the human brain almost nothing actionable. A physician cannot treat a spreadsheet. A public health officer cannot design a hypertension intervention from 500 individual numbers.

To make decisions, we compress data into two questions:

1. **Where is the centre?** — *Central tendency* (mean, median, mode)
2. **How spread out is it?** — *Variability* (range, variance, standard deviation)

For the Framingham cohort, these questions have immediate clinical meaning. What is the typical systolic blood pressure of a middle-aged American in the 1950s? How widely does cholesterol vary across the population? These summaries are the foundation of every cardiovascular risk guideline.

## Section 1: Finding the Centre — Central Tendency

### 1.1 The Mean

> 💡 **Plain English first:** Add all values together and divide by how many there are. The result is pulled toward any extreme values in the data.

The **mean** (x̄, "x-bar") is the arithmetic average.

$$\bar{x} = \frac{\sum x_i}{n}$$

**Worked example — systolic blood pressure:**
Seven participants have systolic BPs (mmHg): 118, 135, 122, 148, 110, 165, 128.

$$\bar{x} = \frac{118+135+122+148+110+165+128}{7} = \frac{926}{7} \approx 132.3 \text{ mmHg}$$

**When to use the mean:** Continuous, ratio-level data that is approximately symmetrically distributed.

> **⚠️ Sensitivity to outliers:** A single participant with a hypertensive crisis (e.g., 220 mmHg) would drag the mean upward. In a population where 95% of participants have systolic BP between 100–170 mmHg, that one extreme value makes the "average" appear higher than most people actually experience.

```{figure} ../images/ch02_skewness.png
:name: fig-skewness
:width: 80%
:align: center

**Figure 2.1** Symmetric, right-skewed, and left-skewed distributions. Systolic blood pressure tends to be slightly right-skewed — most participants cluster in a normal range, but hypertensive outliers create a long right tail. When the mean substantially exceeds the median, report the median.
```

### 1.2 The Median

The **median** is the middle value when all observations are ranked in ascending order.

- If n is **odd**: value at position $\frac{n+1}{2}$
- If n is **even**: average of the two middle values

**Worked example:** Seven BPs ranked: 110, 118, 122, **128**, 135, 148, 165.
n = 7 (odd) → median at position 4 → **128 mmHg**.

The outlier of 165 has no effect on the median. It would remain 128 mmHg whether that participant had 165 or 265 mmHg.

**When to use the median in cardiovascular epidemiology:**
- Cholesterol (right-skewed in population samples)
- Cigarettes per day (right-skewed — many non-smokers, a few heavy smokers)
- Glucose levels (right-skewed — most normal, a few diabetic outliers)
- Time-to-event variables (always use median survival, never mean)

### 1.3 The Mode

The **mode** is the most frequently occurring value. It is the only measure of central tendency valid for nominal categorical data.

**Framingham examples:**
- Modal `SEX` = Female (57% of the sample)
- Modal `EDUC` = Level 1 (0–11 years of schooling) — 193 of 500 participants
- Modal `CURSMOKE` = Smoker (51% at baseline — reflecting 1950s smoking prevalence)

For nominal variables like `ANYCHD`, the mean and median are not appropriate summaries. The mode tells us: the most common outcome in the Framingham cohort was *no CHD event* (69% had no event).

### 1.4 Choosing the Right Measure

| Situation | Measure | Reason |
|---|---|---|
| Continuous, symmetric data | Mean | Uses all values; most statistically powerful |
| Continuous, skewed data | Median | Resistant to distortion by outliers |
| Nominal categorical data | Mode | Mean and median require arithmetic that nominal data cannot support |
| Ordinal data | Median or mode | Ordinal data has rank but not equal intervals |

## Section 2: Measuring the Mess — Variability

Two Framingham cohorts with the same mean systolic BP could have very different clinical implications — one tightly clustered, one with many hypertensive extremes. Variability captures that difference.

### 2.1 The Range

$$\text{Range} = \text{Maximum} - \text{Minimum}$$

**Framingham example:** If the highest SYSBP in the dataset is 228 mmHg and the lowest is 86 mmHg, the range is 142 mmHg.

**Limitation:** Uses only two data points. If 498 of 500 participants have SYSBP between 100–160 mmHg but two extreme outliers exist, the range exaggerates spread.

### 2.2 Variance

> 💡 **Plain English first:** Variance asks — *on average, how far does each blood pressure reading sit from the mean?* It measures spread in squared units.

$$s^2 = \frac{\sum (x_i - \bar{x})^2}{n - 1}$$

**Why (n − 1)?** When working with a sample (our 500 participants are a sample from the population of all middle-aged Americans), dividing by n slightly underestimates the true population variability. Dividing by (n − 1) corrects this. This is **Bessel's correction**.

> ⚡ **Common mistake:** Students ask why we square the deviations rather than take absolute values. Both work mathematically, but squaring has cleaner algebraic properties and is standard across all parametric statistics.

### 2.3 Standard Deviation

The **standard deviation** (s) takes the square root of the variance, returning spread to the original units.

$$s = \sqrt{\frac{\sum (x_i - \bar{x})^2}{n - 1}}$$

- A **small SD** means BPs cluster tightly around the mean — the cohort is homogeneous.
- A **large SD** means BPs scatter widely — the cohort spans a broad clinical range.

```{figure} ../images/ch02_boxplot_anatomy.png
:name: fig-boxplot
:width: 85%
:align: center

**Figure 2.2** Anatomy of a box plot. The box spans the middle 50% of the data (IQR = Q3 − Q1). The red line is the median. Whiskers extend to the most extreme non-outlier values. Dots beyond the whiskers are outliers (> 1.5 × IQR from the box edges). In the Framingham dataset, a box plot of `SYSBP` would show the median around 128 mmHg with a right-skewed distribution.
```

### 2.4 Worked Example — Cholesterol

Seven participants' total cholesterol readings (mg/dL): 195, 220, 245, 210, 280, 230, 215. Mean = 227.9 mg/dL.

| xᵢ | xᵢ − x̄ | (xᵢ − x̄)² |
|:---:|:---:|:---:|
| 195 | −32.9 | 1082.4 |
| 220 | −7.9 | 62.4 |
| 245 | +17.1 | 292.4 |
| 210 | −17.9 | 320.4 |
| 280 | +52.1 | 2714.4 |
| 230 | +2.1 | 4.4 |
| 215 | −12.9 | 166.4 |
| **Sum** | **0.0** | **4642.8** |

> Note: The sum of deviations always equals zero — positive and negative deviations cancel exactly.

$$s^2 = \frac{4642.8}{6} = 773.8 \text{ (mg/dL)}^2 \qquad s = \sqrt{773.8} \approx 27.8 \text{ mg/dL}$$

**Interpretation:** The typical participant's cholesterol is about 227.9 mg/dL, and individual values typically deviate from that by about 27.8 mg/dL. At the population level, a clinical guideline threshold of 240 mg/dL sits less than one SD above the mean — suggesting that a substantial fraction of the Framingham cohort was at or near elevated risk.

## Section 3: Descriptive Statistics and Clinical Interpretation

Descriptive statistics are always reported together. The mean alone is incomplete; the mean with its SD tells the clinical story.

| Variable | Mean ± SD | Median | Clinical interpretation |
|---|---|---|---|
| AGE | 48.4 ± 8.7 years | 48 years | Middle-aged cohort; range 32–70 |
| SYSBP | 131.6 ± 21.9 mmHg | ~128 mmHg | Mean above normal (120); right-skewed |
| TOTCHOL | 235 ± 42 mg/dL | ~232 mg/dL | Mean approaching "borderline high" threshold of 240 |
| BMI | 26.0 ± 4.4 kg/m² | ~25.6 | Mean in overweight range (25–29.9) |
| CIGPDAY | right-skewed | 0 (median = non-smoker) | 51% smoke; among smokers, wide range |

## 🔬 Lab Manual — Chapter 2

### Objective
Calculate descriptive statistics for `SYSBP`, `TOTCHOL`, `AGE`, and `BMI`. Identify which variables are skewed and why.

### Option A — PSPP

1. Open `framingham_study.sav`.
2. **Analyze → Descriptive Statistics → Descriptives**.
3. Move `SYSBP`, `TOTCHOL`, `AGE`, `BMI` to the Variables box.
4. Click **Options** → tick Mean, Std. deviation, Minimum, Maximum → **Continue → OK**.
5. For median: **Analyze → Descriptive Statistics → Explore** → add the same variables → **OK**.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 2 Lab: Descriptive Statistics
# Dataset: framingham_teaching.csv
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")

# ── Descriptives for key continuous variables ─────────
vars <- c("SYSBP", "TOTCHOL", "AGE", "BMI", "CIGPDAY", "GLUCOSE")
for (v in vars) {
  cat("\n──", v, "──\n")
  cat("  Mean:  ", round(mean(fram_data[[v]],   na.rm=TRUE), 2), "\n")
  cat("  Median:", median(fram_data[[v]],        na.rm=TRUE), "\n")
  cat("  SD:    ", round(sd(fram_data[[v]],      na.rm=TRUE), 2), "\n")
  cat("  Range: ", range(fram_data[[v]],         na.rm=TRUE), "\n")
}

# ── Summary for all variables ──────────────────────────
summary(fram_data)

# ── Mode for categorical variables ───────────────────
fram_data$SEX      <- factor(fram_data$SEX, levels=c(1,2), labels=c("Male","Female"))
fram_data$CURSMOKE <- factor(fram_data$CURSMOKE, levels=c(0,1), labels=c("Non-smoker","Smoker"))
fram_data$EDUC     <- factor(fram_data$EDUC, levels=1:4,
                             labels=c("0-11yrs","HS_GED","Some_college","College_grad"), 
                             ordered=TRUE)

table(fram_data$SEX)
table(fram_data$CURSMOKE)
table(fram_data$EDUC)

# ── Descriptives by smoking group ─────────────────────
tapply(fram_data$SYSBP, fram_data$CURSMOKE, mean,   na.rm=TRUE)
tapply(fram_data$SYSBP, fram_data$CURSMOKE, median, na.rm=TRUE)
tapply(fram_data$SYSBP, fram_data$CURSMOKE, sd,     na.rm=TRUE)

# ── Variance ──────────────────────────────────────────
var(fram_data$TOTCHOL, na.rm=TRUE)
```

**What to examine:**
- For `SYSBP` and `TOTCHOL`: is the mean close to the median, or does the mean exceed the median (indicating right skew)?
- For `CIGPDAY`: the median will be 0 (most people are non-smokers or do not smoke heavily). How does the mean compare?
- The SD of `SYSBP` (~22 mmHg) means approximately 68% of participants fall within ±22 mmHg of the mean — from about 110 to 154 mmHg.

### 🧪 Test Your Knowledge

Compute the mean, median, and SD of `TOTCHOL` separately for smokers and non-smokers. **(a)** Write the R code using `tapply()`. **(b)** Is the difference in mean cholesterol clinically meaningful? **(c)** Does smoking appear to be associated with higher cholesterol in this sample?

````{dropdown} Show Solution
```r
# (a) R code
fram_data$CURSMOKE <- factor(fram_data$CURSMOKE, levels=c(0,1),
  labels=c("Non-smoker","Smoker"))

tapply(fram_data$TOTCHOL, fram_data$CURSMOKE, mean,   na.rm=TRUE)
tapply(fram_data$TOTCHOL, fram_data$CURSMOKE, median, na.rm=TRUE)
tapply(fram_data$TOTCHOL, fram_data$CURSMOKE, sd,     na.rm=TRUE)

# (b) A difference of ~5-10 mg/dL between smokers and non-smokers
#     is modest but directionally consistent with the Framingham literature.
#     Whether it is clinically meaningful depends on the absolute values —
#     if both groups average above 230 mg/dL, the difference matters less
#     than both groups being above the "borderline high" threshold.

# (c) We can note an association from descriptives, but we cannot yet
#     determine if it is statistically significant — that requires a
#     hypothesis test (Chapter 5/6).
```
````

## Key Terms

| Term | Definition |
|---|---|
| **Central tendency** | A single value summarising the "centre" of a distribution: mean, median, or mode. |
| **Mean (x̄)** | Arithmetic average. Sensitive to outliers; best for symmetric distributions. |
| **Median** | Middle value when ranked. Resistant to outliers; preferred for skewed health data. |
| **Mode** | Most frequent value. The only valid central tendency measure for nominal data. |
| **Variability** | Degree to which values spread around the centre. |
| **Range** | Maximum − Minimum. Simple but distorted by extreme values. |
| **Variance (s²)** | Average squared deviation from the mean. Uses (n−1) for unbiased estimation. |
| **Standard deviation (s)** | √variance. In original units; most interpretable spread measure. |
| **Bessel's correction** | Using (n−1) rather than n to produce an unbiased sample variance estimate. |

## Review Questions

1. The Framingham dataset shows a mean SYSBP of 131.6 mmHg and a median of approximately 128 mmHg. What does this tell you about the distribution? Would you report the mean or the median as the "typical" systolic BP? Justify your answer.

2. Calculate the mean and SD for these cholesterol values (mg/dL): 180, 210, 225, 240, 195, 310, 220. Show your working. What does the large deviation of 310 tell you about the limitations of the mean?

3. `CIGPDAY` (cigarettes per day) has a median of 0 and a mean well above 0. Explain why, and state which measure better represents the "typical" smoking level in this cohort.

4. Why is neither the mean nor the median appropriate for summarising `ANYCHD`? What measure should be used, and what does it reveal?

5. The SD of `TOTCHOL` in the Framingham cohort is approximately 42 mg/dL. Using the Empirical Rule (Chapter 4), what range of cholesterol values would contain approximately 95% of participants if cholesterol were normally distributed?

6. In R, run `tapply(fram_data$BMI, fram_data$SEX, summary)`. Compare the mean and median BMI by sex. In which group is BMI more skewed, and how can you tell?

```{admonition} Key Takeaways
:class: tip

- **Mean** = Σxᵢ/n — sensitive to outliers; best for symmetric distributions (SYSBP, AGE).
- **Median** = middle value — resistant to outliers; best for skewed data (CIGPDAY, time-to-event).
- **SD** = √s² — in same units as the variable; most interpretable spread measure.
- **Bessel's correction** uses (n−1) to give an unbiased sample variance estimate.
- **Skewness rule:** Mean > Median → right (positive) skew.
- In R: `mean()`, `median()`, `sd()`, `var()`, `summary()`. In PSPP: Analyze → Descriptive Statistics.
```

*Next: **Chapter 3 — The Margin of Error** asks how confident we can be that our sample statistics reflect the broader population of middle-aged Americans.*

---

> Part I — Describing the World in Numbers
