# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 4: The Laws of Chance
## *Probability and Statistical Distributions*

> **Course:** HS 4510 Biostatistics | **Unit:** 4 | **Part:** II — Making Decisions Under Uncertainty
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Describe the properties of the **Normal distribution** and apply the 68–95–99.7 rule
- Convert a raw score to a **Z-score** and use it to find probabilities
- Distinguish between the **Binomial** and **Poisson** distributions and identify when each applies
- State the **Central Limit Theorem** and explain why it justifies using Normal-based inference on real health data
```


---

## Introduction: From Description to Prediction

In the first three chapters, you worked as a statistician in the role of historian. You described what happened in Dr Snow's Soho outbreak: the mean age of victims, the standard deviation of distances from the pump, the confidence interval around those estimates. All of that work looked backward at data that already existed.

Public health, however, demands that practitioners look forward. Before the pump handle is removed, the question is not "what happened?" but "what is likely to happen next, and how certain are we?"

If a new case of cholera appears in Soho, what is the probability that the victim lives within 50 metres of the Broad Street pump? If a household of six is infected, what is the probability that at least four members will die? If Soho normally averages two gastrointestinal deaths per week, what is the probability that 127 deaths in a single week is an ordinary fluctuation rather than an outbreak signal?

These are questions about **probability** — the mathematical language of uncertainty. In biostatistics, probability is not guesswork. It is the application of well-characterised mathematical models, called **distributions**, to quantify how likely different outcomes are. This chapter introduces the three distributions that appear most frequently in health sciences work: the Normal, the Binomial, and the Poisson.

---

## Section 1: The Normal Distribution

### 1.1 Properties of the Normal Distribution

The **Normal distribution** (also called the Gaussian distribution, after the German mathematician Carl Friedrich Gauss) is the most important probability distribution in statistics. Its defining shape is the symmetrical, bell-shaped curve that appears throughout nature.

When you measure a biological characteristic across a large, unselected human population — systolic blood pressure, height, birth weight, serum cholesterol — the resulting distribution tends to approximate the Normal curve. This is not a coincidence. The **Central Limit Theorem** (which underpins nearly all the inferential tests in Chapters 5 through 8) states that when many independent random factors contribute to an outcome, the sum of those factors will follow an approximately Normal distribution, regardless of the individual distributions of the contributing factors.

A Normal distribution has three defining properties:

1. It is perfectly **symmetrical** around the mean. The left half is a mirror image of the right half.
2. The **mean, median, and mode are all equal** — they coincide at the exact centre of the distribution.
3. It is completely defined by just two parameters: the **mean (μ)**, which determines where the centre sits, and the **standard deviation (σ)**, which determines how wide or narrow the bell is.

> **What changes the shape of the bell?**
> Shifting μ moves the entire distribution left or right along the number line without changing its shape. Increasing σ flattens the bell and spreads it wider; decreasing σ makes it taller and narrower. Two Normal distributions with the same mean but different standard deviations are centred on the same point but have very different spreads.

---

### 1.2 The Empirical Rule

The most practically useful property of the Normal distribution is its mathematical predictability. Once you know the mean and standard deviation of a Normally distributed variable, you know precisely what proportion of observations fall within any given range. The **Empirical Rule** summarises the three most clinically important proportions:

| Range | Proportion of observations contained | Clinical interpretation |
|---|:---:|---|
| μ ± 1σ | ~68% | Approximately 2 in every 3 patients falls within one standard deviation of the mean — this is the "typical" range. |
| μ ± 2σ | ~95% | Only 1 in every 20 patients falls outside this range. Values beyond ±2σ are considered statistically unusual. |
| μ ± 3σ | ~99.7% | Fewer than 3 patients in every 1,000 fall outside this range. Values here are extremely rare under normal conditions. |

**Worked example — clinical reference ranges:**

A laboratory measures serum sodium in a healthy adult population and finds μ = 140 mmol/L, σ = 3 mmol/L.

- The **normal reference range** (μ ± 2σ) is: 140 − (2 × 3) to 140 + (2 × 3) = **134 to 146 mmol/L**
- A patient with a serum sodium of 148 mmol/L lies more than 2σ above the mean. Fewer than 2.5% of the healthy population have values this high. The result is clinically flagged as elevated.
- A patient with a serum sodium of 135 mmol/L lies within μ ± 2σ and is considered to be within the normal range.

This is not merely an academic exercise. **Every clinical laboratory reference range printed on a blood test report is derived from the Empirical Rule.** When a result is flagged as "H" (high) or "L" (low), it means the value lies more than approximately 2 standard deviations from the population mean. The logic is statistical before it is clinical.

---

### 1.3 The Z-Score: A Universal Measuring Stick

The Empirical Rule is powerful, but it only answers questions in units of standard deviations (1σ, 2σ, 3σ). In practice, we often want to know where a *specific observed value* sits relative to the distribution — not in the original units of measurement, but in a standardised form that can be compared across different variables and different populations.

The **Z-score** (also called the standard score) converts any raw observation into a common scale: **the number of standard deviations the observation lies above or below the mean.**

$$Z = \frac{x - \mu}{\sigma}$$

Where:
- **x** is the observed value
- **μ** is the population mean
- **σ** is the population standard deviation

**Properties of the Z-score:**
- A Z-score of **0** means the observation is exactly at the mean.
- A **positive** Z-score means the observation is above the mean.
- A **negative** Z-score means the observation is below the mean.
- A Z-score above **+2** or below **−2** corresponds to a value that falls in the outer 5% of the distribution — statistically unusual by the 95% standard used throughout health sciences.

---

**Worked example — comparing two outbreaks:**

Suppose you want to compare the severity of the 1854 Soho cholera outbreak against a 1849 cholera epidemic in a different London district. The raw death counts are in completely different absolute ranges because the districts differ in population size. You cannot compare the numbers directly. But you can compare Z-scores.

- **Soho 1854:** The outbreak week recorded 127 deaths. The historical mean for that district was μ = 2 deaths/week, with σ = 1.2 deaths/week.

$$Z = \frac{127 - 2}{1.2} = \frac{125}{1.2} \approx 104.2$$

A Z-score of +104.2 is astronomically far from anything that could be explained by ordinary variation. This is the mathematical expression of the fact that what happened in Soho in late August 1854 was not a bad week — it was a catastrophe with a single, identifiable cause.

---

**Worked example — distance to the pump:**

A household in Snow's dataset lived 180 metres from the Broad Street pump. Suppose the mean distance for all recorded victims is μ = 52 metres, with σ = 38 metres.

$$Z = \frac{180 - 52}{38} = \frac{128}{38} \approx +3.4$$

A Z-score of +3.4 places this household in the outermost 0.03% of the distribution — they lived extraordinarily far from the pump relative to most victims. Snow would want to investigate why this household was included in the death records despite the distance. (Perhaps they had a habit of collecting water from the Broad Street pump by preference, even though other pumps were closer.)

> **The Z-score and clinical decision-making**
>
> The logic of the Z-score is embedded in every clinical reference range you will encounter in practice. When a haematology report flags a white cell count as "abnormal," it is because the observed value exceeds approximately ±2 standard deviations from the healthy population mean — a Z-score criterion. When a growth chart places a child "below the 3rd percentile," it is because the child's Z-score falls below −2. The statistical framework is identical.

---

## Section 2: The Binomial Distribution

### 2.1 When to Use the Binomial Distribution

The **Binomial distribution** models a very specific situation that appears frequently in clinical and public health contexts:

- You have a **fixed number of independent trials** (n).
- Each trial has exactly **two possible outcomes**: success or failure, infected or not infected, survived or died.
- The **probability of success is constant** (p) across all trials — one patient's outcome does not influence another's.

Under these conditions, the Binomial distribution gives the exact probability of observing any specific number of "successes" out of n trials.

> **⚠️ The independence assumption**
>
> The Binomial distribution assumes that trials are independent — the outcome for one patient does not affect the probability for another. In infectious disease, this assumption is sometimes violated. If one member of a household is infected, others are at higher risk because they share the same exposure. When the independence assumption fails, more complex models (such as the negative binomial or the household secondary attack rate model) are required.

---

### 2.2 Public Health Application — Household Mortality

The case fatality rate of cholera without treatment is approximately 25–50%, depending on the strain and the state of the individual. Let us use p = 0.50 (50% probability of death) as a reasonable figure for an unvaccinated, untreated household in 1854.

If six members of a household are infected, the Binomial distribution answers questions such as:
- What is the probability that **exactly three** of the six will die?
- What is the probability that **at least four** will die?
- What is the probability that **nobody** will die?

These calculations sit at the heart of outbreak modelling, vaccine efficacy trials, and clinical decision analysis. When a pharmaceutical company reports that "71% of vaccinated patients were protected," they used a Binomial model to determine whether that proportion is significantly greater than what would be expected from an unvaccinated population.

**Conditions summary — use the Binomial distribution when:**

| Condition | Required? |
|---|:---:|
| Fixed number of trials (n) | ✓ Yes |
| Binary outcome (success / failure) | ✓ Yes |
| Constant probability of success (p) | ✓ Yes |
| Independent trials | ✓ Yes |

---

## Section 3: The Poisson Distribution

### 3.1 When to Use the Poisson Distribution

The **Poisson distribution** models a different type of problem entirely. Rather than a fixed number of trials, you have a **fixed interval of time or space**, and you wish to model the number of rare events that occur within that interval.

The Poisson distribution applies when:
- Events occur **randomly and independently** within the interval.
- The **average rate** at which events occur (λ, pronounced "lambda") is known and constant.
- Events are **rare** relative to the total number of opportunities for them to occur.

The Poisson distribution answers: *Given that events occur at an average rate of λ per interval, what is the probability of observing exactly k events in one interval?*

---

### 3.2 Public Health Application — Detecting an Outbreak

This is where the Poisson distribution becomes one of the most powerful tools in epidemiology.

**Establishing the baseline:** Soho in the early 1850s experienced an average of approximately 2 deaths per week from gastrointestinal illness (diarrhoea, dysentery, and related causes). This is the Poisson rate parameter λ = 2 deaths/week under normal conditions.

**The outbreak week:** In the week beginning 31 August 1854, over 127 people died in Soho from cholera in a single 24-hour period. The total for the week was several hundred.

**The Poisson question:** Under the historical baseline of λ = 2 deaths/week, what is the probability of observing 127 or more deaths in a single week?

The answer is a number so close to zero that no standard calculator can display it with meaningful precision. It is not 0.000001. It is not 0.0000000001. It is effectively indistinguishable from zero.

**The epidemiological conclusion:** When observed data completely defies the Poisson probability for the established background rate, you have mathematical proof that something other than ordinary random variation is producing the events. In 1854, this was the contaminated pump. In modern epidemiology, an alert system based on Poisson statistics — comparing observed counts against the expected rate — would trigger an outbreak investigation automatically.

> **Modern applications of the Poisson distribution in health sciences:**
> - Monitoring adverse drug reaction reports per million prescriptions
> - Detecting clusters of surgical complications in a hospital unit
> - Modelling cancer incidence in geographic clusters
> - Counting bacteriophage plaques per culture plate
> - Surveillance systems for emerging infectious diseases

---

### 3.3 The Three Distributions — Choosing Between Them

| Distribution | What you are counting | Fixed n? | Time / space interval? | Typical health sciences example |
|---|---|:---:|:---:|---|
| **Normal** | A continuous measurement | N/A | N/A | Blood pressure, height, birth weight |
| **Binomial** | Successes in n independent binary trials | ✓ Yes | ✗ No | Patients who respond to treatment in a trial of n patients |
| **Poisson** | Rare events in a fixed interval | ✗ No | ✓ Yes | Adverse reactions per month, infections per week |

The three distributions are not interchangeable. Applying the Normal distribution to a count of rare events, or applying the Binomial to a dataset where the number of trials is not fixed, produces incorrect probabilities. Choosing the right distribution is as fundamental as choosing the right level of measurement was in Chapter 1.

---

## Section 4: The Epidemic Curve — Reading the Shape of an Outbreak

### 4.1 What an Epidemic Curve Is

An **epidemic curve** (or Epi Curve) is a specific type of histogram in which:
- The **horizontal axis** represents time (days, weeks, or months since the outbreak began).
- The **vertical axis** represents the number of new cases reported in each time interval.
- Each bar shows the incidence — the count of *new* cases — during that period.

Epi Curves are one of the primary diagnostic tools in field epidemiology. Their shape is not merely descriptive; it carries direct information about the *source* and the *mode of transmission* of the outbreak.

---

### 4.2 Point-Source Outbreaks

A **point-source outbreak** occurs when all cases are exposed to a single, common source at approximately the same time. The Epi Curve of a point-source outbreak has a characteristic appearance:

- A **sharp, rapid rise** to a single high peak
- A **steep decline** after the peak
- The entire curve occupies a time window that is approximately equal to one incubation period of the disease

The 1854 Soho cholera outbreak is the textbook example of a point-source Epi Curve. The overwhelming majority of cases occurred within a 24–48 hour window. Once Snow identified and removed the source (the pump handle), new cases ceased almost immediately.

> **Why the curve ends quickly:** In a point-source outbreak, the source is removed (or exhausted) and no further transmission occurs. The cases you see are all the people who were exposed to that single event.

---

### 4.3 Propagated Outbreaks

A **propagated outbreak** (also called a progressive-source outbreak) occurs when disease spreads from person to person, with each generation of cases infecting the next. The Epi Curve of a propagated outbreak looks fundamentally different:

- A **gradual initial rise** of cases
- **Multiple successive peaks**, each generally larger than the last as the infectious pool grows
- The interval between peaks corresponds roughly to the **serial interval** (the average time between a primary case and the secondary cases it generates)
- The curve continues as long as susceptible individuals remain and the pathogen circulates

COVID-19 pandemic waves are the most widely recognised modern example of propagated outbreak curves. The successive peaks corresponded to the emergence of new variants, changes in population immunity, and relaxation of control measures — each wave representing a new generation of transmission.

---

### 4.4 Comparison Table

| Feature | Point-source | Propagated |
|---|---|---|
| **Curve shape** | Single sharp spike, rapid decline | Multiple rolling peaks of increasing height |
| **Exposure type** | All cases share one common source at one time | Person-to-person transmission across generations |
| **Incubation window** | Short — cases cluster within one incubation period | Extended — new cases continue to appear over many serial intervals |
| **1854 example** | Broad Street pump — 127 deaths in 24 hours | Not applicable — cholera is primarily waterborne, not person-to-person |
| **Modern example** | A food poisoning outbreak at a single event | COVID-19 pandemic waves; seasonal influenza |
| **Control strategy** | Remove the single source | Interrupt transmission chains; vaccination; isolation |

> **Why this matters for public health action:** The shape of the Epi Curve tells a field epidemiologist what kind of investigation to launch *before* laboratory confirmation is available. A single spike means: find the common source. Multiple rolling peaks mean: identify and isolate the transmission chain. These are different investigations requiring different resources and different timelines. Reading an Epi Curve correctly can save days of misallocated effort during an active outbreak.

---

## Section 5: Testing for Normality

### 5.1 Why Normality Testing Matters

Many of the statistical tests you will learn in Chapters 5 through 8 — the t-test, ANOVA, Pearson's correlation, linear regression — are called **parametric tests**. They are built on the mathematical assumption that the underlying data follows an approximately Normal distribution. If this assumption is violated, these tests produce inaccurate p-values and confidence intervals.

Before applying any parametric test to a new dataset, you must assess whether the data is sufficiently close to Normal. You do this in two complementary ways: visually, with a histogram, and statistically, with the Shapiro-Wilk test.

---

### 5.2 The Histogram: Visual Assessment

A histogram of a Normally distributed variable should appear approximately bell-shaped: a single central peak, with the bars decreasing symmetrically towards both sides.

**What to look for:**
- **Right skew (positive skew):** The bars pile up on the left and trail off to the right. This is common in health data such as income, length of stay, waiting times, and — importantly for our dataset — distance to a pump. Most victims lived close to the source; a smaller number lived further away.
- **Left skew (negative skew):** The bars pile up on the right and trail off to the left.
- **Bimodal:** Two distinct peaks, suggesting two sub-populations mixed in the dataset.

---

### 5.3 The Q-Q Plot: A More Precise Visual Check

A **Quantile-Quantile plot** (Q-Q plot) compares the observed distribution of your data directly against what would be expected from a perfect Normal distribution.

- The horizontal axis represents the theoretical quantiles of a Normal distribution.
- The vertical axis represents the observed quantiles of your data.
- If the data is approximately Normal, the points will fall along a **straight diagonal line**.
- Deviations from the line in the tails indicate skewness or heavy tails.
- An S-shaped deviation pattern indicates both tails of your distribution differ from the Normal expectation.

The Q-Q plot is a more sensitive visual diagnostic than the histogram, particularly for detecting problems in the tails of a distribution where clinical outliers tend to appear.

---

### 5.4 The Shapiro-Wilk Test: Statistical Assessment

The **Shapiro-Wilk test** is a formal statistical test of the hypothesis that the data was drawn from a Normal distribution.

- **Null hypothesis (H₀):** The data is drawn from a Normal distribution.
- **Alternative hypothesis (Hₐ):** The data is not drawn from a Normal distribution.
- **Decision rule:** If p < 0.05, reject H₀ — the data departs significantly from Normality.

> **An important caveat about large samples**
>
> The Shapiro-Wilk test becomes extremely sensitive with large sample sizes. With n > 100, even trivial, clinically irrelevant departures from Normality — the sort of minor asymmetry that would never affect a t-test — will produce a statistically significant result (p < 0.05). In large samples, the histogram and Q-Q plot are often more informative than the Shapiro-Wilk p-value alone.
>
> Conversely, with very small samples (n < 15), the test has low power and will frequently fail to detect genuine non-Normality. In small samples, visual inspection combined with biological knowledge about the variable is particularly important.

---

## 🔬 Lab Manual — Chapter 4

### Objective

Test the `Distance_Metres` variable in the cholera dataset for approximate Normality using both visual and statistical methods. Interpret the results and state what they imply for the choice of statistical test in subsequent chapters.

---

### Option A — PSPP

PSPP's Graphs menu produces a histogram with an optional overlaid Normal curve, allowing a quick visual comparison.

**Step-by-step instructions:**

1. Open your `snow_1854_outbreak.sav` file.
2. Go to **Graphs → Histogram**.
3. Move `Distance_Metres` into the **Variable** box.
4. Tick the box labelled **Display normal curve**.
5. Click **OK**.

**Interpreting the output:**

Examine how closely the bars align with the overlaid Normal curve.
- If the bars are concentrated on the left and trail off to the right, the distribution is **right-skewed** — exactly what you would expect for distance data, where most victims lived close to the pump and fewer lived far away.
- If the bars match the bell curve closely, the distribution is approximately Normal.

**For the Shapiro-Wilk test in PSPP:**

1. Go to **Analyze → Descriptive Statistics → Explore**.
2. Move `Distance_Metres` into the **Dependent List**.
3. Click the **Plots** button.
4. Under *Normality Plots with Tests*, tick **Shapiro-Wilk**.
5. Click **Continue**, then **OK**.

The output will include the Shapiro-Wilk statistic (W) and its p-value. If p < 0.05, the data departs significantly from Normality.

---

### Option B — R / RStudio

R provides both a visual and a formal statistical assessment of Normality in a small number of lines of code.

```r
# ---------------------------------------------------------
# Chapter 4 Lab: Testing for Normality
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded.
# library(HistData)
# cholera_data <- Snow.deaths

# ----- VISUAL CHECK 1: Histogram -----

# Step 2: Draw a histogram of distances to the pump.
# The argument prob = TRUE rescales the y-axis to probability density,
# which allows us to overlay a Normal curve for comparison.
hist(cholera_data$Distance_Metres,
     main = "Distance from Victim's Home to Broad Street Pump",
     xlab = "Distance (Metres)",
     ylab = "Frequency",
     col  = "lightblue",
     border = "white")

# Step 3: Overlay a Normal curve fitted to the data.
# curve() draws the theoretical Normal density using the sample mean and SD.
curve(dnorm(x,
            mean = mean(cholera_data$Distance_Metres),
            sd   = sd(cholera_data$Distance_Metres)),
      add = TRUE,    # add to the existing histogram
      col = "darkred",
      lwd = 2)

# Does the histogram follow the red Normal curve?
# If the bars pile up on the left and trail off right, the data is right-skewed.

# ----- VISUAL CHECK 2: Q-Q Plot -----

# Step 4: Draw a Quantile-Quantile plot.
# Points that fall along the diagonal dashed line indicate Normality.
# Deviations in the tails indicate skewness or heavy tails.
qqnorm(cholera_data$Distance_Metres,
       main = "Q-Q Plot: Distance to Broad Street Pump")
qqline(cholera_data$Distance_Metres,
       col = "red", lwd = 2)

# ----- STATISTICAL TEST: Shapiro-Wilk -----

# Step 5: Run the Shapiro-Wilk test.
# H0: the data is drawn from a Normal distribution.
# If p < 0.05, we reject H0 — the data is NOT Normal.
shapiro.test(cholera_data$Distance_Metres)

# Step 6: Repeat for the Age variable.
# Is Age more or less Normal than Distance_Metres?
hist(cholera_data$Age,
     main  = "Age Distribution of Cholera Victims",
     xlab  = "Age (Years)",
     col   = "lightgreen",
     border = "white")

shapiro.test(cholera_data$Age)

# Step 7: Compare the two Shapiro-Wilk p-values.
# The variable with the smaller p-value departs more severely from Normality.
# This matters for Chapter 5: if Distance_Metres is not Normal, we may need
# to use non-parametric alternatives to the t-test.
```

**What to record and interpret:**

| Output | What to look for |
|---|---|
| Histogram shape | Does it resemble a bell curve? Or is it skewed to the right? |
| Q-Q plot | Do the points follow the diagonal line, or do they deviate at the ends? |
| Shapiro-Wilk W statistic | W close to 1.0 indicates near-Normality; W well below 1.0 indicates departure |
| Shapiro-Wilk p-value | p < 0.05 → statistically significant departure from Normality |

> **Expected findings for this dataset:** The `Distance_Metres` variable is expected to show substantial right skew — most victims lived close to the pump and the distribution trails off to the right. The Shapiro-Wilk test is likely to return p < 0.05, indicating the data is not Normally distributed. For `Age`, the distribution may be somewhat more symmetrical but still unlikely to pass a strict Normality test. Keep these findings in mind when selecting tests in Chapter 5.

---

---

### 🧪 Test Your Knowledge

In a district with a long-run average of 3.2 cholera deaths per week, assume deaths follow a Poisson distribution. **Task:** (a) Write the R code to find the probability of observing exactly 0 deaths in a week. (b) Find the probability of observing 6 or more deaths (a potential outbreak signal). (c) In a separate scenario: if 40% of households in a street use the Broad Street pump and you randomly select 10 households, what is the probability that exactly 4 of them use that pump? State which distribution you are using and why.

```{dropdown} Show Solution
```r
# (a) P(X = 0) for Poisson with lambda = 3.2
dpois(0, lambda = 3.2)      # ≈ 0.0408 (about 4% chance of zero deaths)

# (b) P(X >= 6) = 1 - P(X <= 5)
1 - ppois(5, lambda = 3.2)  # ≈ 0.0948 (about 9.5% chance — notable but not extreme)

# (c) Binomial: n=10 trials, p=0.40, X = number using Broad St pump
# Use Binomial because: fixed n, binary outcome per household,
# constant probability p, independent selections.
dbinom(4, size = 10, prob = 0.40)  # ≈ 0.2508 (about 25% chance)
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Normal distribution** | A symmetrical, bell-shaped probability distribution defined entirely by its mean (μ) and standard deviation (σ). The mean, median, and mode are equal. |
| **Empirical Rule** | The property of a Normal distribution that approximately 68%, 95%, and 99.7% of observations fall within one, two, and three standard deviations of the mean, respectively. |
| **Z-score** | A standardised score that expresses how many standard deviations an observation lies above or below the population mean. Calculated as Z = (x − μ) / σ. |
| **Binomial distribution** | A probability distribution for a fixed number (n) of independent binary trials, each with the same probability of success (p). Used to model outcomes such as treatment response or survival. |
| **Poisson distribution** | A probability distribution for the number of rare events occurring in a fixed interval of time or space, given a known average rate (λ). Used to detect outbreak clusters. |
| **Epidemic curve (Epi Curve)** | A histogram plotting the number of new cases over time, used to characterise the source and transmission pattern of an infectious disease outbreak. |
| **Point-source outbreak** | An outbreak in which all cases share a single common exposure at approximately the same time, producing a sharp, single-peaked Epi Curve. |
| **Propagated outbreak** | An outbreak sustained by person-to-person transmission, producing a multi-peaked Epi Curve as successive generations of cases are infected. |
| **Shapiro-Wilk test** | A statistical test of the null hypothesis that a sample was drawn from a Normal distribution. A p-value below 0.05 indicates a statistically significant departure from Normality. |
| **Q-Q plot** | A quantile-quantile plot that compares the observed distribution of a sample against the theoretical quantiles of a Normal distribution. Points falling along the diagonal indicate approximate Normality. |

---

## Review Questions

1. A patient's systolic blood pressure is 148 mmHg. The population from which this patient is drawn has μ = 120 mmHg and σ = 15 mmHg. Calculate the patient's Z-score. Using the Empirical Rule, what proportion of the population has a systolic blood pressure higher than this patient's? Is this value clinically unusual?

2. The mean birth weight in a maternity unit is 3,200 g with a standard deviation of 400 g. Assuming birth weight is Normally distributed:
   - (a) Between what two values would you expect approximately 68% of all newborn weights to fall?
   - (b) Between what two values would you expect approximately 95% of all newborn weights to fall?
   - (c) A baby is born weighing 2,100 g. Calculate this baby's Z-score and comment on how unusual this weight is relative to the population.

3. A clinical trial enrols 40 patients with Type 2 diabetes. Each patient is randomly assigned to receive either a new drug or a placebo. The expected response rate for the drug is 70%. Explain which probability distribution — Normal, Binomial, or Poisson — is most appropriate for modelling the number of patients who respond, and justify your answer by checking each of the required conditions.

4. A hospital pharmacovigilance unit records an average of 1.5 adverse drug reactions per month over a three-year baseline period. In a single month, 9 adverse reactions are reported. Which probability distribution would you use to assess whether this represents a statistically unusual cluster? State clearly why the Binomial distribution would be inappropriate here.

5. You run a Shapiro-Wilk test on a dataset of cholera incubation times (n = 45) and obtain p = 0.002. State the null hypothesis of the Shapiro-Wilk test, interpret this p-value, and explain how this finding should influence your choice of statistical test in Chapter 5.

6. An Epi Curve shows a single massive spike on Day 3 of an outbreak, concentrated within a 24-hour window, followed by a rapid decline to baseline within five days. A second Epi Curve from a different outbreak shows three successive peaks over a period of six weeks, each peak larger than the previous one. For each curve: identify the outbreak type, state the most probable mode of transmission, and outline the immediate public health priority that the curve shape suggests.

---

```{admonition} Key Takeaways
:class: tip

- **Normal distribution:** Symmetric, bell-shaped. Defined by μ and σ. 68% within ±1σ, 95% within ±1.96σ, 99.7% within ±3σ.
- **Z-score:** Z = (x − μ) / σ — converts any value to standard deviations from the mean. Z = 0 is the mean; Z = +2 is two SDs above.
- **Binomial:** Counts successes in n independent trials with fixed probability p. Discrete. Used for binary outcomes (infected / not infected).
- **Poisson:** Counts rare events in a fixed time or area with known average rate λ. Discrete. Used for case counts per week/district.
- **Central Limit Theorem:** The sampling distribution of x̄ approaches Normal as n increases, regardless of the population's shape. Justifies t-tests on non-Normal data when n ≥ 30.
- **Normality check:** Shapiro-Wilk test — p > 0.05 means Normality is not violated. Also use histogram and Q-Q plot visually.
```

## Chapter Summary

This chapter has moved beyond description and into prediction — from "what was the average distance?" to "what is the probability that an observation this extreme would arise by chance?"

The Normal distribution is the mathematical backbone of all the parametric tests in the chapters that follow. Its Empirical Rule provides the basis for clinical reference ranges, for the 95% threshold used in confidence intervals, and for the p < 0.05 significance standard you will apply in Chapter 5. The Z-score translates that framework into a universal language that permits comparison across variables measured on different scales and in different populations.

The Binomial and Poisson distributions extend statistical thinking to different data structures — binary outcomes from a fixed number of trials, and rare event counts in a fixed time window. Together, these three distributions cover the vast majority of probability questions that arise in public health practice.

The epidemic curve is a tool that translates probability into action: the shape of the curve tells a field epidemiologist not only what is happening but what kind of response is required and how urgently.

Finally, the normality tests — histogram, Q-Q plot, and Shapiro-Wilk — form an essential preliminary check before any parametric test is applied. The Distance_Metres variable in our dataset is expected to fail this check, which means that as we move into hypothesis testing in Chapter 5, we must keep the question of distributional assumptions in mind.

---

*Next: **Chapter 5 — The Hypothesis Gamble:** Hypothesis Testing, P-Values, and Statistical Power*

---

> Part II — Making Decisions Under Uncertainty
