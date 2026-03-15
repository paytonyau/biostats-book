# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 3: The Margin of Error
## *Uncertainty, Sampling, and Confidence Intervals*

> **Dataset:** Framingham Heart Study teaching subset — `framingham_teaching.csv`, n = 500 participants, baseline examination.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Distinguish between a **population parameter** and a **sample statistic**
- Calculate the **standard error** of the mean
- Construct and correctly interpret a **95% confidence interval**
- Distinguish **random error** from **systematic bias**
```

## Before You Begin: From Description to Inference

Chapter 2 described our 500 Framingham participants. The scientific goal is broader: to use this sample to draw conclusions about the wider population — all middle-aged Americans of that era, and by extension, populations at cardiovascular risk today.

This is the shift from **descriptive** to **inferential** statistics. It requires accepting one fact: **every sample statistic carries uncertainty.** This chapter gives you the tools to quantify that uncertainty: the **standard error** and the **confidence interval**.

## Section 1: Populations and Samples
### 1.1 Key Definitions

- **Population parameter** — the true value for the entire population (e.g., the true mean systolic BP of all middle-aged Americans in the 1950s). Denoted $\mu$ (mean) and $\sigma$ (SD).
- **Sample statistic** — the value calculated from our specific 500 participants. Denoted $\bar{x}$ and $s$.
- **Sampling error** — the inevitable, random difference between a sample statistic and the population parameter it estimates. Not a mistake — a mathematical certainty.

Our 500 participants are a sample. Their mean SYSBP ($\bar{x}$) estimates the population mean ($\mu$) — but $\bar{x} \neq \mu$ exactly. How far off might it be? The standard error answers this.

### 1.2 Accuracy vs Precision

```{figure} ../images/ch03_accuracy_precision.png
:name: fig-accuracy
:width: 75%
:align: center

**Figure 3.1** Accuracy vs precision illustrated with target diagrams. High precision with low accuracy (systematic bias) is more dangerous than low precision alone — it produces confident, reproducible, and consistently wrong results. In epidemiology, a miscalibrated blood pressure cuff produces this pattern.
```

**Framingham example:** If the blood pressure cuffs used in the 1950s Framingham exams were systematically over-reading by 5 mmHg (a calibration error), every measurement would be precise but inaccurate. A larger sample would not fix this — it would just give a more precisely wrong answer.

## Section 2: The Standard Error

### 2.1 Standard Deviation vs Standard Error

> 💡 **Plain English first:** If you drew 100 different samples of 500 Framingham participants, each would give a slightly different mean SYSBP. The SE measures how much those sample means typically vary — it is the standard deviation *of the sampling distribution of the mean*.

| Statistic | What it measures | Formula |
|---|---|---|
| SD ($s$) | Spread of individual observations | $s = \sqrt{\frac{\sum(x_i-\bar{x})^2}{n-1}}$ |
| SE | Precision of the sample mean | $SE = \frac{s}{\sqrt{n}}$ |

**Worked example — SYSBP:**
- $\bar{x}$ = 131.6 mmHg
- $s$ = 21.9 mmHg
- $n$ = 500

$$SE = \frac{21.9}{\sqrt{500}} = \frac{21.9}{22.36} \approx 0.98 \text{ mmHg}$$

Our sample mean of 131.6 mmHg estimates the population mean, and we expect that estimate to be off by about 1 mmHg on average.

```{figure} ../images/ch03_sample_size_se.png
:name: fig-se
:width: 80%
:align: center

**Figure 3.2** The relationship between sample size and standard error. The Framingham study's large sample (here $n=500$) yields a very small SE — the mean blood pressure estimate is highly precise. Halving the sample to 250 would increase SE by a factor of $\sqrt{2} \approx 1.41$, not by double.
```

> **Key relationship:** SE = s/√n. To halve the SE, you must quadruple n. This has major implications for clinical trial design — precision is expensive.

## Section 3: The 95% Confidence Interval

### 3.1 Construction

$$95\% \text{ CI} = \bar{x} \pm 1.96 \times SE$$

**Worked example — mean SYSBP:**
$$95\% \text{ CI} = 131.6 \pm 1.96 \times 0.98 = 131.6 \pm 1.92$$
$$= [129.7 \text{ mmHg}, 133.5 \text{ mmHg}]$$

### 3.2 Correct Interpretation

We are 95% confident that the true mean systolic blood pressure in the population from which this sample was drawn is between 129.7 and 133.5 mmHg.

> ⚡ **Common mistake — the two most frequent CI misinterpretations:**
>
> ❌ "There is a 95% probability the true mean lies in this interval."
> ✅ The 95% refers to the *procedure* — 95% of intervals constructed this way will contain the true mean. This specific interval either contains it or does not.
>
> ❌ "95% of individual participants have SYSBP between 129.7 and 133.5 mmHg."
> ✅ The CI is about the *mean*, not about individuals. The range of individual SYBPs is much wider (86–228 mmHg in our dataset).

### 3.3 What Affects CI Width?

| Factor | Effect on CI | Clinical implication |
|---|---|---|
| Larger n | Narrower | The Framingham study's long run produced very narrow CIs |
| Smaller SD | Narrower | More homogeneous population → more precise estimate |
| 99% vs 95% confidence | Wider | More certainty requires a wider net |

### 3.4 Bias: What a CI Cannot Fix

A CI quantifies **random error** — unavoidable sampling variation. It cannot fix **systematic bias** — a consistent, directional error in data collection.

| Error type | Cause | Fixed by more data? | Example |
|---|---|---|---|
| Random error | Sampling variation | ✓ Yes — larger n shrinks SE | Different nurses get slightly different BP readings |
| Selection bias | Unrepresentative sample | ✗ No | Framingham enrolled only White participants |
| Information bias | Systematic mismeasurement | ✗ No | Miscalibrated BP cuff over-reads by 5 mmHg consistently |

**Information bias** is a specific form of systematic error where the measurement of exposure or outcome is systematically wrong. In the Framingham study, if participants under-reported cigarettes per day (social desirability bias), the true smoking–CHD association would be underestimated. No sample size correction can fix a recording error that happens for every participant.

**Epidemiological example:** The Framingham cohort was almost entirely White, drawn from one Massachusetts town. The mean SYSBP may be a precise estimate of *this population* whilst being a biased estimate of the broader American — let alone global — population. No amount of additional Framingham participants can fix selection bias.

## 🔬 Lab Manual — Chapter 3

### Objective
Calculate the SE and 95% CI for mean SYSBP and mean TOTCHOL. Compare CI widths and interpret the results.

### Option A — PSPP

1. Open `framingham_study.sav`.
2. **Analyze → Compare Means → One-Sample T Test**.
3. Move `SYSBP` and `TOTCHOL` to the Test Variables box. Set Test Value = 0.
4. Click **OK** — the CI appears in the output table.

### Option B — R / RStudio

```r
# -------------------------------------------------------
# Chapter 3 Lab: Standard Error and Confidence Intervals
# -------------------------------------------------------

fram_data <- read.csv("data/framingham_teaching.csv")

# ── Manual SE and 95% CI for SYSBP ───────────────────
n     <- length(na.omit(fram_data$SYSBP))
x_bar <- mean(fram_data$SYSBP, na.rm = TRUE)
s     <- sd(fram_data$SYSBP,   na.rm = TRUE)
SE    <- s / sqrt(n)

CI_lower <- x_bar - 1.96 * SE
CI_upper <- x_bar + 1.96 * SE

cat("SYSBP: mean =", round(x_bar,2), "| SD =", round(s,2),
    "| SE =", round(SE,3), "\n")
cat("95% CI: [", round(CI_lower,2), ",", round(CI_upper,2), "] mmHg\n")

# ── Using t.test() — more accurate for any sample size ──
t.test(fram_data$SYSBP)
t.test(fram_data$TOTCHOL)

# ── Compare CI widths ─────────────────────────────────
# Which variable has more uncertainty in its mean estimate?
ci_sysbp <- t.test(fram_data$SYSBP)$conf.int
ci_chol  <- t.test(fram_data$TOTCHOL)$conf.int

cat("SYSBP CI width:  ", round(diff(ci_sysbp), 3), "mmHg\n")
cat("TOTCHOL CI width:", round(diff(ci_chol),  3), "mg/dL\n")

# ── CI for SYSBP by smoking group ─────────────────────
by(fram_data$SYSBP,
   factor(fram_data$CURSMOKE, labels=c("Non-smoker","Smoker")),
   function(x) t.test(x)$conf.int)
```

### 🧪 Test Your Knowledge

A clinician reads your report and says: "Your 95% CI for mean SYSBP is [129.7, 133.5] mmHg. That means 95% of your participants have blood pressure in that range." **(a)** Identify the error. **(b)** Write the correct interpretation. **(c)** What is the actual range of individual SYSBP values?

````{dropdown} Show Solution
```r
# (a) The clinician has confused the CI for the MEAN with the
#     distribution of INDIVIDUAL observations.

# (b) Correct interpretation: We are 95% confident that the true mean
#     systolic blood pressure in the population from which these
#     500 participants were sampled is between 129.7 and 133.5 mmHg.

# (c) Range of individual values:
range(fram_data$SYSBP, na.rm = TRUE)
# Approximately 86 to 228 mmHg — far wider than the CI for the mean.
```
````

## Key Terms

| Term | Definition |
|---|---|
| **Population parameter** | True value for the entire population. Denoted μ, σ. |
| **Sample statistic** | Value calculated from a specific sample. Denoted x̄, s. |
| **Sampling error** | Inevitable random difference between a sample statistic and the population parameter. |
| **Standard error (SE)** | SE = s/√n. How precisely the sample mean estimates the population mean. |
| **95% Confidence interval** | x̄ ± 1.96 × SE. The plausible range for the true population mean. |
| **Random error** | Sampling variation — reduced by increasing n. |
| **Systematic bias** | Consistent directional error — not fixed by larger sample size. |

## Review Questions

1. In the Framingham dataset (n=500), the SD of AGE is 8.7 years. Calculate the SE and construct the 95% CI for mean age. Interpret the result.

2. If the Framingham study had sampled only n=50 participants instead of 500, what would happen to the SE and CI width for mean SYSBP? Show the calculation.

3. Explain in plain English why a narrow CI does not guarantee the estimate is correct.

4. A published study reports a very narrow 95% CI for mean blood pressure measured in a study where all participants were recruited from a single hospital clinic. A critic says this precision is misleading. Explain why.

5. Run `t.test(fram_data$TOTCHOL)` in R. Identify the 95% CI and write a correct one-sentence interpretation.

```{admonition} Key Takeaways
:class: tip

- **SE** = s/√n — measures precision of the sample mean estimate.
- **95% CI** = x̄ ± 1.96 × SE — plausible range for the true population mean.
- Wider CI = less precision. Narrower = more precision (achieved by larger n).
- **Bias is the silent threat** — a narrow CI around the wrong value is worse than a wide CI around the right one.
- In R: `t.test(variable)` returns the CI directly.
```

*Next: **Chapter 4 — The Laws of Chance** introduces the probability distributions that underpin all hypothesis testing.*

---

> Part I — Describing the World in Numbers
