# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 3: The Margin of Error
## *Uncertainty, Sampling, and Confidence Intervals*

> **Course:** HS 4510 Biostatistics | **Unit:** 3 | **Part:** I — Describing the World in Numbers
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Distinguish between a **population parameter** and a **sample statistic**
- Calculate the **Standard Error** of the mean and explain what it measures
- Construct and correctly interpret a **95% Confidence Interval** for a population mean
- Identify and avoid the three most common misinterpretations of a confidence interval
```


---

## Introduction: Admitting That Our Numbers Might Be Wrong

In Chapter 2, you learnt how to find the centre of a dataset and measure the spread around it. You can now calculate a mean, a median, and a standard deviation. These are essential skills. But there is a harder skill that separates a competent analyst from a genuinely rigorous one:

**The ability to quantify your own uncertainty.**

When Dr John Snow was recording the dead in Soho during August and September of 1854, he faced an uncomfortable reality. He almost certainly missed people. Some victims fled the neighbourhood before dying and were recorded elsewhere, or not at all. Others may have been misdiagnosed as dying from a different cause. A handful of deaths in lodging houses and institutions may never have appeared in his records.

Snow was not working with complete population data. He was working with a sample — the best sample he could construct under extraordinary circumstances. Every figure he calculated from that sample — the average age of victims, the mean distance from the pump — was an *estimate* of the true population value, and estimates carry error.

This chapter is about measuring that error precisely. We call it the margin of error, and learning to express it honestly is one of the most important professional habits a health science practitioner can develop.

---

## Section 1: Accuracy and Precision

Before we can quantify uncertainty, we must understand where measurement errors originate. Our instruments — whether calibrated laboratory centrifuges or patient self-report questionnaires — can fail us in two fundamentally different ways, and it is essential to distinguish between them because they require entirely different remedies.

---

### 1.1 Accuracy

**Accuracy** is the closeness of a measurement to the true value.

An accurate thermometer reads 37.0 °C when the patient's actual temperature is 37.0 °C. A thermometer that reads 35.0 °C when the true temperature is 37.0 °C is *inaccurate*, regardless of how consistently it gives that wrong reading.

Inaccuracy in public health typically arises from:
- Poorly calibrated instruments
- Systematically biased questionnaire wording
- Sampling frames that exclude certain populations (e.g., a telephone survey in an era when poorer households had no telephone)
- Social desirability bias — respondents giving the answer they believe is expected rather than the true answer

---

### 1.2 Precision

**Precision** is the consistency of repeated measurements, entirely independent of whether those measurements are correct.

A precise instrument gives the same result every time it is used under the same conditions. A blood pressure cuff that reads 142/88 mmHg for the same patient on five consecutive measurements is highly precise. If the patient's true blood pressure is 130/78 mmHg, it is also highly inaccurate.

> **⚠️ The most dangerous kind of measurement error**
>
> Precision without accuracy is the most insidious form of error in health sciences research, because it looks like good data. A broken thermometer that consistently reads 2.0 °C below the patient's true temperature will produce five identical measurements — every decimal place consistent, every reading reproducible. A researcher who does not independently validate the instrument will have complete confidence in numbers that are systematically wrong.
>
> In public health, a poorly translated survey question behaves exactly like that broken thermometer. A question that asks non-English-speaking participants to rate their pain on a scale that does not translate idiomatically into their language will produce consistent, precise, and entirely inaccurate responses. No amount of statistical sophistication applied afterwards can correct this. The error is baked into the data collection.

---

### 1.3 The Four Combinations

The relationship between accuracy and precision can be summarised in four scenarios, which are worth committing to memory:

| Scenario | Accurate? | Precise? | What it looks like | Remedy |
|---|---|---|---|---|
| Best case | ✓ Yes | ✓ Yes | Measurements cluster tightly around the true value | Maintain this |
| Random error | ✓ Yes | ✗ No | Measurements scatter around the true value | Increase sample size |
| Systematic bias | ✗ No | ✓ Yes | Measurements cluster tightly around the *wrong* value | Fix the instrument or question |
| Worst case | ✗ No | ✗ No | Measurements scatter randomly around the wrong value | Redesign the study |

The critical column is the remedy. **Random error** — the imprecise but accurate case — can be reduced by collecting more data. Statistics has powerful tools for quantifying and minimising it. **Systematic bias** — the precise but inaccurate case — cannot be corrected by any statistical method once it has entered the data. It must be prevented at the design stage.

---

## Section 2: The Golden Rule — Populations and Samples

The foundational distinction of inferential statistics is the gap between the people you measured and the people you are drawing conclusions about.

---

### 2.1 The Population

The **population** (denoted N, uppercase) is the entire group of individuals about which you wish to draw conclusions. In statistical notation, values that describe a population are called **parameters** and are written using Greek letters:

- **μ (mu)** — the population mean
- **σ (sigma)** — the population standard deviation

For Dr Snow in 1854, the population was every resident of the Soho district of London — all men, women, and children who were exposed to the contaminated water source. This population was, in practice, impossible to enumerate completely.

For a modern clinical researcher studying hypertension, the population might be "all adults aged 40–65 with Stage 1 hypertension in England." No researcher can measure all of them.

---

### 2.2 The Sample

The **sample** (denoted n, lowercase) is the subset of the population that you actually collected data from. In statistical notation, values that describe a sample are called **statistics** and are written using Latin letters:

- **x̄ (x-bar)** — the sample mean
- **s** — the sample standard deviation

Dr Snow's door-to-door records represented a sample — the households he was able to reach and from which he could obtain reliable information. It was an excellent sample, but it was not the population.

> **Why the notation matters**
>
> The distinction between Greek letters (parameters) and Latin letters (statistics) is not pedantic convention. It encodes a crucial epistemological distinction. We almost *never* know the true population parameter. We *always* estimate it from a sample statistic. When you write μ, you are referring to an unknown truth. When you write x̄, you are referring to your best available estimate of that truth. Conflating them is the statistical equivalent of confusing a map with the territory.

---

### 2.3 The Fundamental Problem of Inference

Because we only ever work with samples, our sample mean (x̄) will never perfectly match the population mean (μ). If we drew a different random sample from the same population, we would almost certainly get a slightly different mean. Draw one hundred samples and calculate one hundred means — each one would differ slightly from the others. This variability *across samples* is not a flaw. It is an inevitable mathematical property of sampling.

The question we must answer is: **how large is this variability likely to be?**

That is precisely what the Standard Error measures.

---

## Section 3: Standard Error — The Uncertainty of the Mean

The Standard Error of the Mean (SE) quantifies how much the sample mean would be expected to vary if the study were repeated multiple times on different samples drawn from the same population.

It is the bridge between descriptive statistics — describing *your particular sample* — and inferential statistics — making claims about the *population*.

---

### 3.1 Standard Deviation Versus Standard Error

These two quantities are frequently confused, even by practising clinicians. The distinction is fundamental.

| Quantity | Symbol | What it measures | Question it answers |
|---|---|---|---|
| Standard Deviation | s | The spread of individual observations around the mean *within your sample* | How different are patients from one another? |
| Standard Error | SE | The expected variability of the sample mean *across repeated studies* | How reliable is our estimate of the population mean? |

Standard deviation describes your data. Standard error describes your estimate.

A large standard deviation means patients in your dataset vary widely from one another in the variable you measured. A large standard error means your particular sample may yield a mean that differs substantially from another researcher's sample — your estimate of the population mean is imprecise.

---

### 3.2 The Formula

$$SE = \frac{s}{\sqrt{n}}$$

Where:
- **s** is the sample standard deviation (from Chapter 2)
- **n** is the number of observations in the sample

**Worked example:** Suppose a sample of 64 cholera victims has a mean age of x̄ = 31.2 years, with a standard deviation of s = 14.8 years.

$$SE = \frac{14.8}{\sqrt{64}} = \frac{14.8}{8} = 1.85 \text{ years}$$

The standard error of the mean age is 1.85 years. This means that if we repeated Dr Snow's survey many times, we would expect the sample mean age to vary by approximately ±1.85 years around the true population mean.

---

### 3.3 The Key Insight — Larger Samples Produce Smaller Error

Examine the formula carefully. The sample size n sits inside the square root in the denominator. This means that as n increases, the denominator grows, and SE shrinks. This is the mathematical proof of an intuitive principle that every public health researcher understands: **the larger your sample, the more precise your estimate of the population mean.**

| Sample size (n) | √n | SE (given s = 14.8) |
|:---:|:---:|:---:|
| 10 | 3.16 | 4.68 years |
| 25 | 5.00 | 2.96 years |
| 64 | 8.00 | 1.85 years |
| 100 | 10.00 | 1.48 years |
| 400 | 20.00 | 0.74 years |

Notice two things from this table. First, doubling the sample size does not halve the standard error — because of the square root, you must *quadruple* the sample size to halve the SE. Going from n = 25 to n = 100 halves the SE from 2.96 to 1.48. This is the law of diminishing returns in sampling: the early additions to your sample size improve precision rapidly; later additions improve it more slowly.

Second, with only n = 10 observations, the SE of 4.68 years is substantial — our estimate of the mean age could easily be 4–5 years away from the truth. With n = 400, the SE falls to 0.74 years — a much tighter margin.

If Dr Snow had interviewed only 10 households, his estimate of the average distance to the pump would have been statistically shaky. By recording nearly 600 deaths, his estimate became precise enough to be clinically and legally persuasive.

---

## Section 4: Confidence Intervals — Expressing Uncertainty Honestly

With the Standard Error calculated, we can now build the most important reporting tool in health sciences: the **Confidence Interval** (CI).

---

### 4.1 The Problem With Point Estimates

A **point estimate** is a single number used to estimate a population parameter — for example, reporting that "the mean age of cholera victims was 31.2 years." Point estimates are useful but inherently misleading, because they present a single value with false precision. Every sample mean comes with uncertainty, and reporting only the mean conceals that uncertainty from the reader.

Consider the difference between a clinician saying:

- *"The average recovery time is 5 days."*
- *"The average recovery time is 5 days (95% CI: 4.2 to 5.8 days)."*

The first statement sounds authoritative. The second is honest. The second also tells a hospital administrator something practically important: they should plan for recovery times ranging from just over 4 days to nearly 6 days, not exactly 5.

---

### 4.2 The 95% Confidence Interval

The **Confidence Interval** is a range of plausible values for the population parameter, constructed from the sample statistic and the standard error. The standard in health sciences is the **95% Confidence Interval**, calculated as:

$$95\% \ CI = \bar{x} \pm 1.96 \times SE$$

The value 1.96 is a **critical value** drawn from the standard Normal distribution (which we shall study in depth in Chapter 4). It is the value that captures the central 95% of a Normal distribution, leaving 2.5% in each tail. For large samples, 1.96 is a fixed constant used universally for 95% CIs.

**Worked example:** Using our cholera victim data where x̄ = 31.2 years and SE = 1.85 years:

$$95\% \ CI = 31.2 \pm (1.96 \times 1.85)$$
$$= 31.2 \pm 3.63$$
$$= [27.6, \ 34.8] \text{ years}$$

---

### 4.3 How to Interpret a Confidence Interval Correctly

The correct interpretation of a 95% CI is one of the most commonly misunderstood concepts in applied statistics. Read the following carefully.

**Correct interpretation:**

> "If we repeated this sampling procedure 100 times and constructed a 95% confidence interval from each sample, approximately 95 of those 100 intervals would contain the true population mean."

**What this means in practice for our example:**

> "We do not know the exact mean age of every person who died in the 1854 Soho cholera outbreak. Based on our sample, we are 95% confident that the true population mean age lies somewhere between 27.6 and 34.8 years."

**The two most common misinterpretations to avoid:**

> ❌ *"There is a 95% probability that the true mean lies between 27.6 and 34.8."*

This is incorrect. The true mean is a fixed (unknown) value — it either is or is not in your particular interval. Probability applies to the procedure, not to any single interval.

> ❌ *"95% of individual patients have ages between 27.6 and 34.8 years."*

This is also incorrect. The CI is about the *mean*, not about individual observations. The standard deviation describes the spread of individuals; the CI describes the uncertainty of the mean.

---

### 4.4 Width of the Confidence Interval and What It Tells You

The **width** of a confidence interval is a direct measure of the precision of your estimate.

- A **narrow CI** (e.g., [31.0, 31.4]) indicates a precise estimate — your sample was large enough that the true population mean is probably very close to your sample mean.
- A **wide CI** (e.g., [12.0, 50.4]) indicates an imprecise estimate — your sample was too small, or the data too variable, to pin down the population mean with confidence.

Wide confidence intervals in public health reports are not failures to be hidden. They are honest signals that more data is needed before a reliable clinical or policy decision can be made.

---

## Section 5: The Qualitative Bridge — When Statistics Cannot Save You

The concepts introduced in this chapter — standard error, confidence intervals, margins of error — all address one specific type of uncertainty: **random error** arising from working with a sample rather than the full population. Statistical methods are exceptionally good at quantifying and managing random error.

They are completely powerless against **systematic bias**.

---

### 5.1 Recall Bias

When you ask patients to report past behaviour or symptoms from memory, their recollections are rarely accurate. A patient asked to estimate how many units of alcohol they consumed per week over the past year will almost certainly underreport — not because they are deliberately dishonest, but because humans systematically underestimate their own behaviour when it carries social stigma.

This underreporting does not scatter randomly. It consistently pushes the reported value in one direction. No confidence interval can correct for this. The data itself is wrong, and it is wrong in a predictable, systematic way.

---

### 5.2 The Hawthorne Effect

When people know they are being observed, they tend to modify their behaviour — typically towards the behaviour they believe the observer wants to see. This phenomenon, documented in a series of industrial psychology studies in the 1920s and 1930s, is known as the **Hawthorne Effect**.

In a clinical trial measuring patient adherence to a new medication regimen, a patient who is normally non-compliant may adhere perfectly for the duration of the trial — not because the medication is effective or because the intervention is working, but simply because they know a researcher is monitoring them. The trial then reports excellent adherence rates that will evaporate the moment the study ends.

---

### 5.3 The Fundamental Principle

> **No matter how narrow your confidence interval, if your data were collected through a systematically biased instrument, a poorly translated questionnaire, or a sampling frame that excluded part of the population, your statistics will be confidently wrong.**

A 95% confidence interval of [4.98, 5.02] days looks extraordinarily precise. If the underlying data was collected from patients who all knew they were being monitored and therefore all behaved atypically, that interval is precisely, confidently, and usefully meaningless.

This is why the discipline of epidemiology spends as much energy on study design, questionnaire validation, and sampling strategy as it does on statistical analysis. The mathematics in this course can only be trusted when the data it operates on has been collected with rigour.

---

## 🔬 Lab Manual — Chapter 3

### Objective

Calculate the Standard Error and 95% Confidence Interval for the mean distance that cholera victims lived from the Broad Street pump. Interpret the interval in plain language suitable for a non-specialist audience.

---

### Option A — PSPP

PSPP's **Explore** procedure generates a 95% Confidence Interval automatically alongside the mean.

**Step-by-step instructions:**

1. Open your `snow_1854_outbreak.sav` file from Chapter 1.
2. Go to **Analyze → Descriptive Statistics → Explore**.
3. Move `Distance_Metres` into the **Dependent List** box.
4. Click the **Statistics** button. Confirm that **Descriptives** is ticked and that the confidence interval percentage is set to **95%**.
5. Click **Continue**, then **OK**.
6. Examine the output table. You will see the following rows:

| Row in output | What it contains |
|---|---|
| Mean | Your point estimate — the sample mean (x̄) |
| 95% Confidence Interval for Mean — Lower Bound | The lower limit of the CI |
| 95% Confidence Interval for Mean — Upper Bound | The upper limit of the CI |
| Std. Deviation | The sample standard deviation (s) |
| Std. Error Mean | The Standard Error (SE = s / √n) |

> **What to record:** Write down the lower and upper bounds of the confidence interval. Then write a one-sentence plain-language interpretation: "We are 95% confident that the true mean distance from the Broad Street pump, across all households in Soho, lies between __ metres and __ metres."

---

### Option B — R / RStudio

R's `t.test()` function, which you will use extensively in Chapter 6 to compare groups, also generates a 95% Confidence Interval when applied to a single variable. This is an efficient and widely used shortcut.

```r
# ---------------------------------------------------------
# Chapter 3 Lab: Standard Error and Confidence Intervals
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded from Chapter 1.
# If you are starting a new session, run these lines first:
# library(HistData)
# cholera_data <- Snow.deaths

# Step 2: Apply t.test() to a single variable.
# When used on one variable, t.test() tests whether the mean is
# significantly different from zero — but more usefully for us now,
# it outputs the 95% confidence interval for the mean.
t.test(cholera_data$Distance_Metres)

# Step 3: Locate the confidence interval in the console output.
# The relevant section will look like this:
#
#   95 percent confidence interval:
#    [lower_bound]   [upper_bound]
#
# Record both values.

# Step 4: Calculate the Standard Error manually to verify.
# SE = s / sqrt(n)
# We can extract these components from R as follows:
s  <- sd(cholera_data$Distance_Metres)
n  <- length(cholera_data$Distance_Metres)
SE <- s / sqrt(n)

# Print each component clearly labelled:
cat("Standard deviation (s):", round(s, 3), "\n")
cat("Sample size (n):", n, "\n")
cat("Standard error (SE):", round(SE, 3), "\n")

# Step 5: Calculate the 95% CI manually using the formula.
# 95% CI = x_bar +/- 1.96 * SE
x_bar <- mean(cholera_data$Distance_Metres)
lower <- x_bar - 1.96 * SE
upper <- x_bar + 1.96 * SE

cat("95% CI: [", round(lower, 2), ",", round(upper, 2), "]\n")

# Step 6: Compare the manual CI to the t.test() output.
# They will be very close but not identical: t.test() uses the
# t-distribution critical value (which accounts for sample size),
# whilst the manual formula uses 1.96 (the large-sample approximation).
# For n > 30, the difference is negligible.

# Step 7: Repeat the analysis for the Age variable.
# Is the confidence interval for mean age wider or narrower than
# the interval for Distance_Metres? What does this tell you about
# the relative precision of each estimate?
t.test(cholera_data$Age)
```

**What to examine in the output:**

- Compare the width of the CI for `Distance_Metres` against the CI for `Age`. The wider interval corresponds to the more variable distribution — the one where individual observations scatter more widely around the mean.
- Notice that the lower bound of the CI for Distance is positive (it must be — distances cannot be negative). Is the lower bound of the CI for Age clinically plausible?

---

---

### 🧪 Test Your Knowledge

A subsample of n = 25 cholera victims from the Broad Street pump area has a mean age of 34.2 years and a standard deviation of 11.8 years. **Task:** (a) Calculate the Standard Error. (b) Construct the 95% CI. (c) Write a one-sentence interpretation of the interval for a public health report. (d) What would happen to the width of the CI if the sample size were increased to n = 100?

```{dropdown} Show Solution
```r
# (a) Standard Error
n  <- 25
xbar <- 34.2
s  <- 11.8
SE <- s / sqrt(n)
SE          # 2.36

# (b) 95% Confidence Interval (using z = 1.96 for large n; 
#     for n=25 strictly use t(24) = 2.064 — both shown below)
CI_z <- c(xbar - 1.96 * SE, xbar + 1.96 * SE)
CI_z        # [29.57, 38.83] (z approximation)

t_crit <- qt(0.975, df = 24)   # 2.064
CI_t   <- c(xbar - t_crit * SE, xbar + t_crit * SE)
CI_t        # [29.33, 39.07] (exact t-interval)

# (c) Interpretation:
# "We are 95% confident that the true mean age of Broad Street pump
#  victims in the Soho population lies between 29.3 and 39.1 years."

# (d) At n = 100: SE = 11.8 / sqrt(100) = 1.18 — half the original SE,
#     giving a CI approximately half as wide: roughly [31.9, 36.5].
#     Quadrupling the sample size halves the CI width.
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Accuracy** | The closeness of a measured value to the true or accepted value. A measurement can be precise but inaccurate. |
| **Precision** | The consistency or reproducibility of repeated measurements, regardless of whether those measurements are correct. |
| **Population (N)** | The entire group of individuals about which conclusions are to be drawn. Described by parameters, denoted with Greek letters (μ, σ). |
| **Sample (n)** | A subset of the population that is actually measured. Described by statistics, denoted with Latin letters (x̄, s). |
| **Parameter** | A summary value that describes an entire population. Parameters are typically unknown and must be estimated from sample statistics. |
| **Statistic** | A summary value calculated from a sample, used to estimate the corresponding population parameter. |
| **Standard Error (SE)** | The expected variability of the sample mean across repeated samples of the same size. Calculated as s / √n. Smaller SE indicates a more precise estimate. |
| **Point estimate** | A single value used to estimate a population parameter, such as reporting a sample mean as an estimate of the population mean. |
| **Confidence Interval (CI)** | A range of plausible values for a population parameter. A 95% CI is constructed such that, if the sampling procedure were repeated 100 times, approximately 95 of the resulting intervals would contain the true parameter. |
| **Recall bias** | A systematic error arising when participants inaccurately remember past events or behaviours, typically in a consistent and predictable direction. |
| **Hawthorne Effect** | The tendency for people to modify their behaviour when they are aware that they are being observed, which can distort the results of a study. |

---

## Review Questions

1. A nurse uses a blood pressure cuff that consistently reads 8 mmHg higher than the patient's true systolic pressure. Is this primarily an accuracy problem or a precision problem? Explain precisely why no statistical technique applied to data collected with this cuff can correct the error.

2. Explain the difference between a population parameter and a sample statistic. Write out the correct notation for the population mean, the population standard deviation, the sample mean, and the sample standard deviation.

3. A researcher studying post-operative recovery time collects data from 25 patients and finds: x̄ = 6.2 days, s = 2.1 days.
   - (a) Calculate the Standard Error of the Mean. Show your working.
   - (b) Construct the 95% Confidence Interval.
   - (c) Write a one-sentence plain-language interpretation of this interval for a hospital administrator who has no statistical training.
   - (d) What would happen to the width of the CI if the sample size were increased from 25 to 100 patients? Calculate the new SE to support your answer.

4. A research paper reports: "Mean length of hospital stay: 6.1 days (95% CI: 4.8 to 7.4 days)." A colleague reads this and says: "So there is a 95% chance the true mean is between 4.8 and 7.4 days." Is your colleague correct? If not, write the correct interpretation.

5. Describe the Hawthorne Effect and provide a specific example from clinical practice or public health research where it could distort the findings of a study. Explain why a narrow confidence interval would not protect against this distortion.

6. Dr Snow's dataset almost certainly omitted some victims who fled Soho before dying and were therefore never recorded. Is this an example of random error or systematic bias? In which direction would this omission likely bias his estimates of the total death toll and the mean age of victims? Explain your reasoning.

---

```{admonition} Key Takeaways
:class: tip

- **Standard Error:** SE = s / √n — measures how precisely the sample mean estimates the population mean.
- **95% CI formula:** x̄ ± 1.96 × SE — applies when n is large; use t-distribution for small samples.
- **Correct interpretation:** 'We are 95% confident the true population mean lies between [lower] and [upper].' The interval, not the parameter, is uncertain.
- **Width of CI:** Wider = less precise. Narrower = more precise. Increasing n narrows the interval.
- **❌ Common error:** 'There is a 95% probability the mean is in this interval.' The mean is fixed; the interval is what varies across repeated samples.
- **In R:** `t.test(x)` returns the 95% CI directly. **In PSPP:** Analyze → Descriptive Statistics → Explore.
```

## Chapter Summary

This chapter has introduced the concept that lies at the heart of all inferential statistics: **our data is a sample, our sample contains error, and that error can be measured and communicated honestly.**

Accuracy and precision are distinct properties of measurement, and only precision — random error — can be addressed by collecting more data or by statistical adjustment. Systematic bias must be prevented through careful study design; no formula can remove it once it has entered the data.

The population/sample distinction, formalised through the notation of Greek letters for parameters and Latin letters for statistics, encodes the most important epistemological reality in biostatistics: we almost never know the truth; we estimate it. The Standard Error quantifies how variable those estimates would be across repeated studies. The Confidence Interval translates that variability into a range of plausible values that a decision-maker can act upon.

Dr Snow did not know the true mean age or the true mean distance of all Soho residents from the pump. He knew his sample estimates — and he knew that with nearly 600 observations, his standard errors were small enough that his estimates were trustworthy. That mathematical confidence gave him the authority to make a life-saving recommendation.

In Chapter 4, we shall look at the mathematical foundations of the 95% figure itself — the Normal distribution — and discover why the number 1.96 is so central to everything we have calculated in this chapter.

---

*Next: **Chapter 4 — The Laws of Chance:** Probability and Statistical Distributions*

---

> Part I — Describing the World in Numbers
