# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 2: The Middle and the Mess
## *Measures of Central Tendency and Variability*

> **Course:** HS 4510 Biostatistics | **Unit:** 2 | **Part:** I — Describing the World in Numbers
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Calculate and interpret the **mean**, **median**, and **mode** for a health dataset
- Calculate the **range**, **variance** (with Bessel's correction), and **standard deviation**
- Choose the appropriate measure of central tendency based on distribution shape
- Produce descriptive statistics in PSPP and R and interpret the output table
```


---

## Before You Begin: Why Summaries Matter

In Chapter 1, you classified the variables in Dr John Snow's 1854 cholera dataset. You now know that `Age` and `Deaths` are ratio variables, `Distance_Metres` is ratio, and `Pump_Used` is nominal. That classification work was not procedural — it determined which calculations you are permitted to perform on each column.

Now consider what you are working with. The `Snow.deaths` dataset contains 578 individual death records from the Soho cholera outbreak. Each row is a person, or more precisely, a location where people died. Looking at 578 rows of numbers gives the human brain almost no information it can act upon. A physician cannot prescribe a treatment to 578 rows. A public health officer cannot convince a city council to remove a pump handle by handing them a spreadsheet.

To make clinical and public health decisions from data, we must compress those 578 observations into a small number of summary statistics that answer two fundamental questions:

1. **Where is the centre of this data?** — This is called *central tendency*.
2. **How spread out is this data around that centre?** — This is called *variability*.

These two questions form the foundation of descriptive statistics, and descriptive statistics form the foundation of every inferential test you will encounter in Chapters 5 through 8.

---

## Section 1: Finding the Centre — Central Tendency

Central tendency gives us a single representative number for the "typical" observation in a dataset. There are three measures: the mean, the median, and the mode. Each is appropriate under different conditions, and choosing the wrong one produces a misleading summary.

---

### 1.1 The Mean

The **mean** (denoted x̄, pronounced "x-bar") is the arithmetic average. You sum all the values in a variable and divide by the number of observations.

**Formula:**

$$\bar{x} = \frac{\sum x_i}{n}$$

Where:
- x̄ is the sample mean
- Σxᵢ is the sum of all individual values
- n is the number of observations

**Worked example:** Suppose Dr Snow recorded the following ages for the first seven cholera victims: 25, 31, 44, 18, 72, 29, 35.

$$\bar{x} = \frac{25 + 31 + 44 + 18 + 72 + 29 + 35}{7} = \frac{254}{7} \approx 36.3 \text{ years}$$

The mean age of these seven victims is approximately 36.3 years.

**When to use the mean:** The mean is appropriate for continuous, ratio-level data that is approximately symmetrically distributed — where extreme values are rare and the data does not trail off heavily in one direction.

> **⚠️ Critical warning — sensitivity to outliers**
>
> The mean is severely distorted by extreme values. Consider what happens if one household in Soho suffered 15 cholera deaths whilst most households had 1 or 2. That single tragic address would drag the mean deaths-per-household upward, making the "average" household appear to have experienced far more deaths than most actually did. The mean becomes a number that describes nobody's experience accurately.
>
> This is not a minor technical inconvenience. In a public health report, a distorted mean can misallocate resources, understate the severity of an outbreak, or cause an intervention to target the wrong population.

---

### 1.2 The Median

The **median** is the middle value in a dataset when all observations are arranged in ascending order. If you lined up all 578 cholera victims from youngest to oldest, the median age would be the age of the person standing exactly in the middle of that line.

**Calculating the median:**

- If n is **odd**: the median is the value at position $\frac{n+1}{2}$.
- If n is **even**: the median is the average of the two middle values, at positions $\frac{n}{2}$ and $\frac{n}{2} + 1$.

**Worked example:** Using the same seven ages: 18, 25, 29, **31**, 35, 44, 72.

There are 7 observations (odd), so the median is at position $\frac{7+1}{2} = 4$. The fourth value is **31 years**.

Notice that the victim aged 72 — who sits far to the right — has no effect whatsoever on the median. It would remain 31 regardless of whether that victim was 72 or 720. This is the median's defining strength.

**When to use the median:** The median is the preferred measure of central tendency whenever data is skewed by outliers. In public health practice, the following variables are almost always reported as medians rather than means:

- Duration of hospitalisation (a small number of very long stays distort the mean)
- Household income (a small number of very high earners distort the mean)
- Waiting times for treatment (rare extreme delays distort the mean)
- Survival time after diagnosis (a small number of very long survivors distort the mean)

> **How to detect skew without drawing a graph:** Calculate both the mean and the median for the same variable. If the two values are close together, the distribution is approximately symmetrical and the mean is trustworthy. If the mean is noticeably larger than the median, the distribution is *positively skewed* — pulled rightward by high outliers. If the mean is noticeably smaller than the median, the distribution is *negatively skewed* — pulled leftward by low outliers. This two-number check is the fastest diagnostic in descriptive statistics.

---

### 1.3 The Mode

The **mode** is the value that appears most frequently in a dataset. Unlike the mean and median, the mode requires no arithmetic — it is identified by inspection or by frequency count.

A dataset may have:
- **One mode** (unimodal): one value appears more than all others.
- **Two modes** (bimodal): two values appear equally often and more than all others — this often signals that the dataset contains two distinct sub-populations.
- **No mode**: every value appears exactly once.

**When to use the mode:** The mode is the *only* measure of central tendency that can legitimately be applied to nominal categorical data. The mean and median require numerical operations. Nominal data does not support arithmetic.

Consider `Pump_Used` in the Snow dataset. We cannot calculate the average pump. We cannot find the middle pump. But we can count how many deaths are associated with each pump and identify the one that appears most frequently. In Dr Snow's records, the modal `Nearest_Pump` value is *Broad Street* — it appears in more death records than any other pump. This single observation, expressed as the mode, was a cornerstone of his argument to the Board of Guardians.

> **A note for continuous data:** The mode is rarely reported for continuous ratio variables such as `Age` or `Distance_Metres`, because with sufficient decimal precision every value in a continuous dataset may be unique and the mode would simply be "every value occurs once." The mode becomes more meaningful when continuous data is grouped into intervals (e.g., age brackets).

---

### 1.4 Summary — Choosing the Right Measure of Central Tendency

| Situation | Recommended measure | Reason |
|---|---|---|
| Continuous, symmetrical data | Mean | Uses all values; most statistically powerful |
| Continuous, skewed data or data with outliers | Median | Resistant to distortion by extreme values |
| Nominal categorical data | Mode | Mean and median require arithmetic that nominal data cannot support |
| Ordinal data | Median (or mode) | Ordinal data has rank but not equal intervals; the mean may be misleading |
| Bimodal distribution | Report both modes | A single mean or median conceals the two-group structure |

---

## Section 2: Measuring the Mess — Variability

Knowing the centre of a distribution is necessary but not sufficient. Consider two hospital wards, both with a mean patient age of 55 years. In Ward A, every patient is between 50 and 60 years old. In Ward B, the patients range from 15 to 95 years old. The mean is identical; the clinical reality is completely different. To capture that difference, we measure *variability* — how widely the individual observations are spread around the centre.

---

### 2.1 The Range

The **range** is the simplest measure of spread.

$$\text{Range} = \text{Maximum value} - \text{Minimum value}$$

**Worked example:** If the oldest cholera victim was 82 years old and the youngest was 3 months old (recorded as age 0), the range of ages is 82 − 0 = **82 years**.

**Limitation of the range:** The range uses only two data points — the extreme maximum and the extreme minimum. It tells you the total width of your data but reveals nothing about the distribution within that width. If 576 of your 578 victims are aged between 20 and 50, but one is aged 2 and one is aged 82, the range reports 80 years — which suggests wide spread when the reality is tight clustering in a narrow band.

The range is useful for a quick, rough sense of scale, but it should never be the sole measure of variability in a research report.

---

### 2.2 Variance

**Variance** (s²) measures the average squared deviation of each observation from the mean. It answers the question: *On average, how far does each individual data point sit from the centre?*

**The calculation proceeds in four steps:**

**Step 1:** Calculate the mean (x̄).

**Step 2:** For each observation xᵢ, calculate the deviation from the mean: (xᵢ − x̄).

**Step 3:** Square each deviation: (xᵢ − x̄)². This removes negative signs — deviations below the mean become positive — and amplifies large deviations, giving outliers more weight.

**Step 4:** Sum all squared deviations and divide by (n − 1):

$$s^2 = \frac{\sum (x_i - \bar{x})^2}{n - 1}$$

> **Why (n − 1) and not n?**
>
> When we calculate variance from a *sample* (as is nearly always the case in public health — we have data from some patients, not all possible patients), dividing by n produces a result that slightly underestimates the true variability in the broader population. Dividing by (n − 1) corrects for this systematic underestimation. This adjustment is called **Bessel's correction**, named after the German mathematician Friedrich Bessel. It ensures that the sample variance is an *unbiased estimator* of the population variance.
>
> You will encounter this distinction again in Chapter 3 when we discuss the difference between population parameters and sample statistics.

**Limitation of variance:** The units of variance are squared. If you measured distances in metres, variance is expressed in metres². If you measured age in years, variance is expressed in years². Squared units are mathematically valid but practically uninterpretable — a clinician cannot meaningfully say "the variance in patient ages is 196 years²."

---

### 2.3 Standard Deviation

The **standard deviation** (s) resolves this problem by taking the square root of the variance, which returns the spread to the original units of measurement.

$$s = \sqrt{\frac{\sum (x_i - \bar{x})^2}{n - 1}}$$

If the variance is 196 years², the standard deviation is √196 = **14 years**. That is a number a clinician can interpret: patients in this dataset typically fall within approximately 14 years of the mean age.

**Interpreting the standard deviation:**

- A **small standard deviation** means the observations are tightly clustered around the mean. The mean is a reliable representative of the typical observation.
- A **large standard deviation** means the observations are widely scattered. The mean is less reliable as a summary because individual observations may differ substantially from it.

> **🏥 Public health application — proving the pump**
>
> If we calculate the mean distance from victims' homes to the Broad Street pump, and the standard deviation of those distances is very small, this proves mathematically that the deaths are tightly clustered around that specific pump — they are *not* scattered randomly across the neighbourhood in the way we would expect if cholera were caused by atmospheric miasma, which would produce roughly equal infection rates in all directions.
>
> This is precisely the statistical argument Dr Snow used to convince the Board of Guardians to remove the pump handle. The small standard deviation of distances was not a footnote — it was the evidence. A large standard deviation would have supported the miasma theory. A small one demolished it.

---

### 2.4 Worked Example — Calculating Variance and Standard Deviation

Using a simplified dataset of seven distance measurements (in metres) from victims' homes to the Broad Street pump:

**Distances:** 45, 62, 38, 71, 50, 55, 48

**Step 1 — Calculate the mean:**
$$\bar{x} = \frac{45 + 62 + 38 + 71 + 50 + 55 + 48}{7} = \frac{369}{7} \approx 52.7 \text{ m}$$

**Step 2 and 3 — Deviations and squared deviations:**

| Observation (xᵢ) | Deviation (xᵢ − x̄) | Squared deviation (xᵢ − x̄)² |
|:---:|:---:|:---:|
| 45 | −7.7 | 59.29 |
| 62 | +9.3 | 86.49 |
| 38 | −14.7 | 216.09 |
| 71 | +18.3 | 334.89 |
| 50 | −2.7 | 7.29 |
| 55 | +2.3 | 5.29 |
| 48 | −4.7 | 22.09 |
| **Sum** | **0.0** | **731.43** |

> Note that the sum of deviations is always zero — deviations above and below the mean cancel out exactly. This is why we must square them.

**Step 4 — Variance:**
$$s^2 = \frac{731.43}{7 - 1} = \frac{731.43}{6} \approx 121.9 \text{ m}^2$$

**Step 5 — Standard deviation:**
$$s = \sqrt{121.9} \approx 11.0 \text{ m}$$

**Interpretation:** The typical victim in this small sample lived approximately 52.7 metres from the Broad Street pump, and individual distances typically deviated from that figure by about 11.0 metres. The clustering is relatively tight, supporting the pump as the outbreak source.

---

## Section 3: The Relationship Between Central Tendency and Variability

These two families of statistics — central tendency and variability — are always reported together in public health research. To report only one is to provide an incomplete picture.

Consider three hypothetical cholera outbreaks with identical means but different standard deviations:

| Outbreak | Mean deaths per address | Standard deviation | Interpretation |
|---|---|---|---|
| A | 3.2 deaths | 0.4 deaths | Deaths are tightly clustered — nearly every address has approximately 3 deaths. Strong signal around a single source. |
| B | 3.2 deaths | 2.1 deaths | Moderate spread — some addresses have 1 death, others have 5 or 6. The source may be more diffuse. |
| C | 3.2 deaths | 6.8 deaths | Wide scatter — many addresses have 0 deaths, a few have very many. The "average" of 3.2 describes almost no actual address accurately. |

All three outbreaks have the same mean. The standard deviation is what distinguishes them analytically and epidemiologically.

---

## 🔬 Lab Manual — Chapter 2

### Objective

Calculate measures of central tendency and variability for the `Distance_Metres` variable in the cholera dataset. Compare the mean and median to assess whether the distribution is skewed.

---

### Option A — PSPP

PSPP's Descriptives procedure calculates the mean, standard deviation, minimum, and maximum in a single operation.

**Step-by-step instructions:**

1. Ensure your `snow_1854_outbreak.sav` file is open from Chapter 1.
2. Go to **Analyze → Descriptive Statistics → Descriptives**.
3. In the dialog box, find `Distance_Metres` in the left panel and click the arrow to move it to the **Variables** box on the right.
4. Click the **Options** button. Tick the following boxes:
   - Mean
   - Std. deviation
   - Minimum
   - Maximum
5. Click **Continue**, then **OK**.
6. Examine the output window. You will see a table with one row per variable.

**For the median**, PSPP requires a separate procedure:

7. Go to **Analyze → Descriptive Statistics → Explore**.
8. Move `Distance_Metres` into the **Dependent List** box.
9. Click **OK**. The output will include the median, along with the 25th and 75th percentiles (the interquartile range, which we shall study in later chapters).

**What to record:**
- Mean distance
- Median distance
- Standard deviation
- Minimum and maximum (from which you can calculate the range)
- The difference between mean and median: if it is large, the distribution is skewed

---

### Option B — R / RStudio

R calculates all descriptive statistics with a small number of simple functions. The `summary()` function is the most efficient starting point.

```r
# ---------------------------------------------------------
# Chapter 2 Lab: Central Tendency and Variability
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded from Chapter 1.
# If you are starting a new session, run these two lines first:
# library(HistData)
# cholera_data <- Snow.deaths

# Step 2: Calculate the mean distance to the pump.
# The $ operator selects a single column from the data frame.
mean(cholera_data$Distance_Metres)

# Step 3: Calculate the median distance.
# Compare this to the mean. A large gap indicates skewness.
median(cholera_data$Distance_Metres)

# Step 4: Calculate the standard deviation.
# This is expressed in the same units as Distance_Metres (i.e., metres).
sd(cholera_data$Distance_Metres)

# Step 5: Calculate the range.
# range() returns a vector with two values: [minimum, maximum].
range(cholera_data$Distance_Metres)

# Step 6: Get everything at once with summary().
# summary() returns: Min, 1st quartile, Median, Mean, 3rd quartile, Max.
# This single line replaces Steps 2 through 5 for a quick overview.
summary(cholera_data$Distance_Metres)

# Step 7: Calculate the variance directly if you wish to verify your
# hand calculation from Section 2.4.
var(cholera_data$Distance_Metres)

# Step 8: Calculate descriptive statistics for the Age variable
# and compare them. Is the age distribution more or less skewed
# than the distance distribution?
summary(cholera_data$Age)
```

**What to examine in the output:**

- After running `summary()`, note whether the **Mean** and **Median** are close together or far apart.
- A mean that is substantially larger than the median indicates that a small number of victims lived very far from the pump — positive skew. These are likely households that nevertheless drank from the Broad Street pump by choice or habit, despite the distance.
- The standard deviation from `sd()` tells you how tightly the deaths cluster geographically around the pump. A small standard deviation strengthens Snow's spatial argument; a large one would weaken it.

> **Extending the analysis:** If you wish to calculate the mean and standard deviation for multiple variables at once, use `sapply()`:
>
> ```r
> sapply(cholera_data[, c("Age", "Distance_Metres", "Deaths")], mean)
> sapply(cholera_data[, c("Age", "Distance_Metres", "Deaths")], sd)
> ```

---

---

### 🧪 Test Your Knowledge

Using the Snow dataset, compute the mean, median, and standard deviation of the `Age` variable **separately for each pump group** (Broad St, Rupert St, Marlborough Mews). **Task:** (a) Write the R code using `tapply()`. (b) In one sentence, state which pump group had the highest mean age and interpret what that might suggest about the outbreak.

```{dropdown} Show Solution
```r
# (a) R code
tapply(cholera_data$Age, cholera_data$Pump_Used, mean, na.rm = TRUE)
tapply(cholera_data$Age, cholera_data$Pump_Used, median, na.rm = TRUE)
tapply(cholera_data$Age, cholera_data$Pump_Used, sd, na.rm = TRUE)

# (b) Example interpretation (your numbers may vary with dataset version):
# If Broad Street victims have the highest mean age, this could suggest
# that older residents were less mobile and more reliant on the nearest
# pump, whilst younger residents may have sourced water from further away.
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Central tendency** | A single value that summarises the "centre" of a distribution. The three principal measures are the mean, the median, and the mode. |
| **Mean (x̄)** | The arithmetic average of all values in a dataset, calculated by dividing the sum of all values by the number of observations. Sensitive to extreme outliers. |
| **Median** | The middle value when all observations are ranked in ascending order. Preferred over the mean when data is skewed or contains outliers. |
| **Mode** | The most frequently occurring value in a dataset. The only measure of central tendency valid for nominal categorical data. |
| **Variability** | The degree to which values in a dataset are spread out around the centre. |
| **Range** | The difference between the maximum and minimum values. The simplest measure of spread, but vulnerable to distortion by a single extreme value. |
| **Variance (s²)** | The average squared deviation of each observation from the mean. Expressed in squared units, which limits direct interpretability. |
| **Standard deviation (s)** | The square root of the variance. Expressed in the original units of measurement. A small standard deviation indicates tight clustering around the mean. |
| **Bessel's correction** | The use of (n − 1) rather than n in the denominator of the sample variance formula, which corrects for the tendency of the sample variance to underestimate the population variance. |
| **Skewed distribution** | A distribution in which data is asymmetrical — pulled towards high values (positive skew) or low values (negative skew) by extreme observations. |

---

## Review Questions

1. The number of days that patients spend in hospital following surgery is recorded for 50 patients. Three patients had exceptionally long stays of 60, 75, and 90 days. Would you report the mean or the median as the "typical" length of stay? Justify your answer with specific reference to the properties of each measure.

2. Calculate the mean and median for the following set of daily cholera death counts recorded over ten consecutive days: 2, 3, 3, 4, 5, 5, 5, 6, 7, 40. Show your working. What does the difference between the two values indicate about the distribution?

3. A clinical researcher reports: "The standard deviation of patient recovery times in our trial was only 1.2 days." What does this small standard deviation tell you about the patients in the study? What would a standard deviation of 14.7 days tell you instead?

4. The `Pump_Used` variable in the Snow dataset identifies which pump each household drew water from. Explain why neither the mean nor the median is an appropriate summary statistic for this variable. What measure of central tendency should be used, and what would it reveal about the outbreak?

5. In your own words, explain why we use (n − 1) rather than n in the denominator of the sample standard deviation formula. Your answer should include the name of this correction and a brief explanation of why dividing by n would produce a biased result.

6. Using the R code from the Lab Manual, run `summary(cholera_data$Age)` and `summary(cholera_data$Distance_Metres)`. For each variable, state whether the distribution appears to be skewed and in which direction. Explain how you reached this conclusion from the `summary()` output alone, without drawing a graph.

---

```{admonition} Key Takeaways
:class: tip

- **Mean** = Σxᵢ / n — sensitive to outliers; best for symmetric distributions.
- **Median** = middle value — resistant to outliers; best for skewed distributions.
- **Variance** s² = Σ(xᵢ − x̄)² / (n−1) — uses n−1 (Bessel's correction) to give an unbiased estimate.
- **Standard Deviation** s = √s² — in the *same units* as the original variable; most interpretable spread measure.
- **Skewness rule of thumb:** Mean > Median → right (positive) skew. Mean < Median → left (negative) skew.
- **In R:** `mean()`, `median()`, `sd()`, `var()`, `summary()`. **In PSPP:** Analyze → Descriptive Statistics → Descriptives.
```

## Chapter Summary

This chapter has introduced the two foundational questions of descriptive statistics: where is the centre, and how much scatter surrounds it?

The mean, median, and mode each answer the first question under different conditions. The mean is powerful but fragile — one extreme value can render it unrepresentative. The median is resistant to outliers and is the standard reporting measure for skewed health data. The mode is the only legitimate summary for nominal categorical variables.

The range, variance, and standard deviation answer the second question. The range is a quick rough indicator. The variance quantifies average squared spread. The standard deviation translates that quantity back into interpretable units — and in Dr Snow's case, a small standard deviation of distances was not merely a statistic. It was the evidence that ended an outbreak.

In Chapter 3, we shall ask a harder question: we have described our sample, but how confident can we be that our sample statistics accurately reflect the broader population from which it was drawn? This moves us from *description* to *inference* — and introduces the concept of the confidence interval.

---

*Next: **Chapter 3 — The Margin of Error:** Sampling, Standard Error, and Confidence Intervals*

---

> Part I — Describing the World in Numbers
