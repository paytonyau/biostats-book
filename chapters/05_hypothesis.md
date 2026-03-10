# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 5: The Hypothesis Gamble
## *Sampling Methods, Study Design, Hypothesis Testing, and Statistical Power*

> **Course:** HS 4510 Biostatistics | **Unit:** 5 | **Part:** II — Making Decisions Under Uncertainty
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Explain the four main **probability sampling methods** and state why sampling design determines the validity of inference
- State a **Null and Alternative Hypothesis** using correct statistical notation
- Define the **p-value** precisely and identify the four most common misinterpretations
- Distinguish **Type I** and **Type II errors**, define **statistical power**, and identify the four levers that increase it
```


---

## Introduction: Resolving Scientific Disagreements With Evidence

In 1854, the medical establishment of London was firmly committed to the **miasma theory** of disease — the belief that cholera and other epidemic illnesses were caused by foul, toxic air rising from rotting organic matter in the streets and sewers. The theory was coherent, internally consistent, and supported by respected physicians and public officials. It was also completely wrong.

Dr John Snow believed the mechanism was waterborne transmission — that cholera was ingested through contaminated water, not inhaled through contaminated air. He had good circumstantial reasons for this belief. But a belief, however well reasoned, is not scientific evidence. And in the face of an entrenched theory supported by powerful institutions, Snow could not simply assert that miasma was wrong.

He had to prove it.

How does a scientist resolve a disagreement like this? Not by debating opinions, not by appealing to authority, and not by waiting for a consensus to form. In biostatistics, we resolve scientific disagreements by constructing a **formal mathematical trial** — a procedure called **hypothesis testing** that transforms a research question into a decision that can be made, with quantified uncertainty, from data.

This is the chapter where epidemiology moves from describing a problem to making a definitive, defensible, life-saving decision. Should the local council remove the handle from the Broad Street pump? A hypothesis test will give us the answer — and more importantly, it will tell us exactly how confident we are in that answer.

---

## Section 1: Sampling — The Foundation of Inference

Before any hypothesis test is run, before any p-value is computed, a more fundamental question must be answered: where did the data come from? The validity of every inferential statistic — every confidence interval, every t-test, every regression — rests entirely on how the sample was obtained. A mathematically perfect analysis of a badly collected sample produces a confidently wrong answer.

**Inferential statistics** allow us to draw conclusions about a *population* from a *sample*. But this inference is only legitimate when the sample was obtained through a process that gives every member of the target population a known, non-zero chance of being selected. The methods that achieve this are called **probability sampling designs**. Methods that do not are called **non-probability sampling designs**.

---

### 1.1 The Target Population and the Sampling Frame

Before selecting a sample, two things must be defined:

- The **target population** is the complete group about which you wish to draw conclusions. For Dr Snow, the target population was all residents of the Soho district who used the Broad Street pump.
- The **sampling frame** is the practical list or enumeration from which the sample is actually drawn — the set of individuals who *could* be selected. In practice, the sampling frame rarely covers the target population perfectly. Households without a fixed address, residents who moved away before the investigation began, or individuals who refused to be recorded are excluded from the frame but belong to the target population. This mismatch is called **coverage error**, and no statistical method can correct it after the fact.

A well-conducted study minimises the gap between target population and sampling frame, documents any known differences between them, and discusses how those differences might affect the conclusions.

---

### 1.2 Probability Sampling Methods

In a **probability sample**, every individual in the sampling frame has a known, calculable probability of being selected. This property is what makes the central limit theorem applicable and the confidence interval formula valid — the mathematics of inference assumes that the sample was obtained this way.

#### Simple Random Sampling

Every individual in the sampling frame has an equal probability of being selected. Selection is performed by assigning a number to each member and using a random number generator (or random number table) to select the required sample size.

**Advantage:** Produces an unbiased sample with no systematic exclusion of any subgroup.
**Limitation:** Requires a complete, accurate sampling frame. For very large populations, this may be logistically impractical.

**Snow example:** Assign a unique number to each of the 600 households in the Soho district. Use a random number generator to select 100. Every household has a 100/600 ≈ 16.7% probability of selection.

#### Stratified Random Sampling

The population is first divided into mutually exclusive, non-overlapping subgroups called **strata** (based on a variable known before sampling — e.g., distance from the pump, floor of building, or socioeconomic status). A random sample is then drawn independently from each stratum.

**Advantage:** Guarantees that important subgroups are represented in the sample, and increases precision compared to simple random sampling when the strata differ substantially from each other on the outcome of interest.
**Limitation:** Requires prior knowledge of the stratifying variable for all members of the sampling frame.

**Snow example:** Divide households into three strata by distance from the pump: <100m, 100–300m, >300m. Randomly sample 30 households from each stratum. This ensures the analysis includes sufficient representation from households at all distances.

#### Systematic Sampling

The sampling frame is sorted and every k-th individual is selected, where k = (population size) / (desired sample size). The starting point is chosen at random within the first k individuals.

**Advantage:** Simple to implement in practice when a sorted list is available. Distributes the sample evenly across the frame.
**Limitation:** If the list has a periodic pattern at the same interval k, systematic sampling can introduce bias. For example, if every 10th house on a street is a corner house, and the sample interval is 10, every selected house will be a corner house.

**Snow example:** The 600 households are listed by street address. Desired sample size: 60. k = 600/60 = 10. Choose a random starting point between 1 and 10 — say, 4. Select households 4, 14, 24, 34, ... 594.

#### Cluster Sampling

The population is divided into naturally occurring groups called **clusters** (e.g., city blocks, households, schools, hospital wards). A random sample of *clusters* is selected, and then *all* individuals within the selected clusters are surveyed.

**Advantage:** Highly practical when the population is geographically dispersed and a complete individual-level sampling frame does not exist. Significantly reduces the cost and logistic burden of data collection.
**Limitation:** Individuals within the same cluster tend to be more similar to each other than to individuals in other clusters (intra-cluster correlation). This reduces the effective sample size and must be accounted for in the analysis. Cluster samples generally require larger total sample sizes than simple random samples to achieve the same statistical power.

**Snow example:** Divide Soho into 60 city blocks. Randomly select 10 blocks. Survey every household in those 10 blocks. No individual-level sampling frame is needed — only a list of blocks.

---

### 1.3 Non-Probability Sampling Methods

In a **non-probability sample**, the probability of any individual being selected is either unknown or zero for some members of the target population. This does not make such studies worthless — they are often the only practical option in exploratory research, qualitative research, or emergency outbreak investigations — but it means that formal inferential statistics (confidence intervals, p-values) carry important caveats. The results cannot be assumed to generalise to the target population with the precision that probability-based formulas imply.

| Method | How participants are selected | Typical use case | Key limitation |
|---|---|---|---|
| **Convenience sampling** | Whoever is available and willing at the time of data collection. | Preliminary studies, student research, rapid assessments. | May systematically exclude people who are harder to reach, sicker, or less engaged — producing selection bias. |
| **Purposive sampling** | Participants selected deliberately because they possess specific characteristics relevant to the research question. | Qualitative research; expert panels; rare-population studies. | Cannot produce generalisable quantitative estimates; appropriate only when the goal is depth, not breadth. |
| **Snowball sampling** | Existing participants recruit further participants from their social networks. | Hard-to-reach populations (undocumented migrants, drug users, people with stigmatised conditions). | Sampling is constrained by network structure — isolated individuals or those outside the initial network are unreachable. |

> **⚠️ Why sampling method determines the validity of inference**
>
> Confidence intervals and p-values are derived from probability theory that assumes a random sample. When a convenience sample is used, the 95% confidence interval formula still produces a numerical output — but the output is only as valid as the assumption that the sample is representative. A study that reports a p-value from a convenience sample without acknowledging this limitation is making a stronger inferential claim than the data can support. This does not mean such studies are useless; it means their conclusions must be qualified accordingly and that replication in a probability sample is required before policy decisions are made.

---

### 1.4 Sampling in the Snow Dataset Context

Dr Snow's investigation of the 1854 Broad Street outbreak did not use a probability sample in the modern sense — he recorded deaths from all households in the affected area (effectively a census of the outbreak zone), not a random subset. This is one reason his conclusions were so compelling: he was not generalising from a sample to a population; he was documenting the population of interest in its entirety.

Modern outbreak investigations rarely have this luxury. A public health team investigating a foodborne illness outbreak at a hospital with 400 patients cannot conduct a detailed interview with every patient. They would use **stratified random sampling** — stratifying by ward, meal type, or symptom onset time — to select a manageable sample whilst ensuring all subgroups are represented.

The choice of sampling method is made at the *design stage*, before any data is collected, and it is one of the most consequential methodological decisions a researcher makes. A well-designed study with a modest sample size will produce more trustworthy conclusions than a poorly designed study with a large one.

---

## Section 2: The Framework of the Trial

### 2.1 The Presumption of No Effect

Every hypothesis test begins with the same intellectual posture: **scepticism**. Before examining the data, we presume that nothing interesting is happening — that any pattern we observe is merely the result of random chance. We require the data to overcome this presumption with sufficiently strong evidence before we will change our minds.

This mirrors the structure of a criminal trial. In law, a defendant is presumed innocent until the prosecution presents evidence sufficient to establish guilt beyond reasonable doubt. In statistics, the "innocent" position is called the **Null Hypothesis**, and the standard of evidence required to overturn it is expressed as a probability threshold.

---

### 2.2 The Two Competing Hypotheses

Every hypothesis test involves exactly two mutually exclusive hypotheses:

| | Null Hypothesis (H₀) | Alternative Hypothesis (Hₐ) |
|---|---|---|
| **Role** | The sceptic's position — the default assumption | The researcher's claim — what they believe is actually true |
| **Claims** | No real effect, no real difference — any pattern is random chance | A real, measurable effect exists that cannot be explained by chance alone |
| **Snow example** | "There is no difference in cholera rates between people who drank from the Broad Street pump and those who drank from the Rupert Street pump." | "People who drank from the Broad Street pump have a significantly higher rate of cholera than those who drank from other pumps." |
| **Our goal** | Gather evidence to *reject* this | Indirectly supported if H₀ is rejected |

> **The asymmetry of hypothesis testing — a critical principle**
>
> Our goal is never to "prove" the Alternative Hypothesis. We can only ever *reject* or *fail to reject* the Null Hypothesis. This asymmetry is not a limitation of the method; it is its intellectual foundation. Science cannot prove that something is true — it can only accumulate evidence that the opposite is implausible. We prove guilt; we do not prove innocence.
>
> This means that even when a study fails to find a statistically significant result, this does not prove that no effect exists. It means only that *this particular study, with this particular sample size, did not find sufficient evidence to reject the Null Hypothesis.* The distinction matters enormously — see Section 3.

---

### 2.3 One-Tailed and Two-Tailed Tests

The Alternative Hypothesis can be stated in two ways, and this choice affects the mathematics of the test:

- A **two-tailed test** asks: "Is there a difference in either direction?" It considers both the possibility that the Broad Street pump victims are older *and* younger than the comparison group.
  - Written as: Hₐ: μ₁ ≠ μ₂
  - This is the standard choice for most clinical research, where we wish to detect any effect regardless of direction.

- A **one-tailed test** asks: "Is there a difference in a specific direction?" It considers only one possibility — for example, that the pump victims are *younger* than the comparison group.
  - Written as: Hₐ: μ₁ < μ₂ or Hₐ: μ₁ > μ₂
  - This is appropriate only when there is a strong prior theoretical reason to expect the effect in one direction specifically, and it should be pre-specified before data collection begins.

> **Practical guidance:** Use a two-tailed test by default. One-tailed tests produce smaller p-values for effects in the predicted direction, which can be tempting — but using a one-tailed test to make a borderline result "significant" after looking at the data is a form of p-hacking (discussed in Chapter 9) and undermines the validity of the test.

---

## Section 3: The P-Value — The Most Misunderstood Number in Science

### 3.1 What the P-Value Measures

After running a hypothesis test, the software returns a single number called the **p-value** (probability value). It is the most reported and the most misinterpreted number in all of health sciences research.

The p-value answers one precise question:

> **"If the Null Hypothesis is completely true, what is the probability of observing data at least as extreme as what we actually observed, purely by chance?"**

A **small p-value** means the observed data would be very improbable if H₀ were true. This improbability is evidence against H₀ — the data is difficult to explain as random noise if nothing is actually happening.

A **large p-value** means the observed data is quite consistent with what we would expect by chance if H₀ were true. There is no strong reason to reject H₀.

---

### 3.2 The Significance Threshold (α)

The **significance threshold**, denoted α (alpha), is the pre-specified probability below which we will declare the result "statistically significant" and reject H₀. The universal convention in health sciences is:

$$\alpha = 0.05$$

This means we accept a **5% risk** of rejecting H₀ when it is actually true — a false positive rate of 1 in 20.

The three scenarios below illustrate how to interpret different p-values in the context of Snow's pump investigation:

| p-value | Interpretation | Decision |
|:---:|---|---|
| p = 0.80 | Not surprising at all. Results this extreme would occur by chance 80% of the time if the pump were safe. No evidence against H₀. | **Fail to reject H₀** — cannot conclude the pump is contaminated. |
| p = 0.03 | Surprising. If the pump were truly safe, data this extreme would arise by chance only 3% of the time. This falls below α = 0.05. | **Reject H₀** — statistically significant evidence that the pump is contaminated. |
| p = 0.001 | Extremely surprising. Only a 0.1% chance of seeing data this extreme if H₀ is true. Very strong evidence against H₀. | **Reject H₀** — remove the pump handle immediately. |

---

### 3.3 What the P-Value Does NOT Mean

The p-value is perhaps the most frequently misinterpreted concept in all of applied statistics. The following misinterpretations appear regularly in published research, clinical reports, and policy documents. Each one is wrong.

---

> ❌ **Misinterpretation 1:** "The p-value is the probability that H₀ is true."
>
> ✅ **Correct statement:** The p-value is the probability of observing *data this extreme* if H₀ were true. These two statements are not equivalent. H₀ is either true or it is not — it does not have a probability. The p-value is a property of the data and the test, not of the hypothesis.

---

> ❌ **Misinterpretation 2:** "A p-value of 0.04 means there is a 4% chance the pump is safe."
>
> ✅ **Correct statement:** It means that if the pump were safe (H₀ true), data as extreme as ours would occur only 4% of the time. Whether the pump is actually safe is a question about the world, not about a probability distribution.

---

> ❌ **Misinterpretation 3:** "A p-value below 0.05 means the result is important or clinically meaningful."
>
> ✅ **Correct statement:** Statistical significance and clinical significance are entirely separate concepts. With a very large sample size, even a trivially small effect can produce p < 0.05. A drug that reduces systolic blood pressure by 0.3 mmHg might be "statistically significant" in a trial of 50,000 patients, but no clinician would prescribe it for this reason. The p-value tells you whether the effect is real; it tells you nothing about whether the effect matters.

---

> ❌ **Misinterpretation 4:** "A p-value of 0.06 means the result is not significant and therefore the treatment has no effect."
>
> ✅ **Correct statement:** A p-value of 0.06 means the evidence did not reach the pre-specified threshold in *this study*. It does not mean the treatment is ineffective. It may mean the sample was too small to detect a real but modest effect (see Section 3 — Type II error).

---

### 3.4 "Statistically Significant" Does Not Mean "Proven"

When we say a result is "statistically significant at p < 0.05," we are making a probabilistic statement within a framework that accepts a 5% false positive rate. Out of every 20 statistically significant findings published in the literature, one is expected to be a false positive — a chance result that happened to cross the threshold. This is not a flaw in the method; it is a clearly stated and quantified feature of it.

The problem arises when "statistically significant" is reported or read as though it means "definitely true." It does not. It means "unlikely to be due to chance under the stated assumptions, at this level of evidence."

---

## Section 4: The Two Ways to Be Wrong — Type I and Type II Errors

Because hypothesis testing relies on probability rather than certainty, it will occasionally produce the wrong decision. There are exactly two ways this can happen, and they have different causes, different consequences, and different remedies.

---

### 4.1 The Error Table

| | **H₀ is actually true** (no real effect) | **H₀ is actually false** (real effect exists) |
|---|---|---|
| **You reject H₀** | ❌ **Type I Error** — False Positive | ✅ Correct decision — True Positive |
| **You fail to reject H₀** | ✅ Correct decision — True Negative | ❌ **Type II Error** — False Negative |

---

### 4.2 Type I Error (α) — The False Positive

A **Type I error** occurs when you reject H₀, but H₀ was actually true. You have detected an effect that does not exist.

- **Controlled by:** The significance threshold α. Setting α = 0.05 means you accept a 5% probability of making this error.
- **The metaphor:** Crying wolf — raising a public health alarm that was not warranted.
- **The consequence in Snow's scenario:** You tell the city council that the Broad Street pump is contaminated. The council removes the pump handle. The water was actually safe, and hundreds of residents now have no access to clean water — all because of a false positive.
- **The consequence in a clinical trial:** You conclude that a new drug is effective when it is not. Patients are prescribed an ineffective treatment, potentially at significant cost and with side-effect risk.

> **Note:** Reducing α (e.g., to 0.01) reduces the Type I error rate but increases the Type II error rate. You become less likely to cry wolf, but more likely to miss a genuine outbreak. These two error rates are in direct tension with one another.

---

### 4.3 Type II Error (β) — The False Negative

A **Type II error** occurs when you fail to reject H₀, but H₀ was actually false. A real effect existed and you missed it.

- **Controlled by:** Statistical power (1 − β). Higher power means lower β.
- **The metaphor:** Missing the wolf — staying silent when danger is genuinely present.
- **The consequence in Snow's scenario:** The data does not look convincing enough. You conclude the pump is safe and leave the handle on. The water is contaminated. Hundreds more people die.
- **The consequence in a clinical trial:** You conclude that an effective drug does not work. Patients are denied access to a treatment that would have benefited them. Future researchers may not pursue the drug because of a false null result.

---

### 4.4 Comparison Summary

| | Type I Error (α) | Type II Error (β) |
|---|---|---|
| **What happened** | Rejected H₀, but H₀ was true | Failed to reject H₀, but H₀ was false |
| **Common name** | False positive | False negative |
| **The metaphor** | Crying wolf | Missing the wolf |
| **Public health consequence** | Unnecessary intervention; resource waste; loss of public trust | Failure to act; preventable harm; effective treatments withheld |
| **Controlled by** | Significance threshold α | Statistical power (1 − β), primarily through sample size n |

---

### 4.5 The Context-Dependence of Error Tolerance

The relative severity of Type I and Type II errors is not fixed — it depends entirely on the context of the decision.

Consider two scenarios:

**Scenario A — Screening for a rare, fatal cancer:** A false negative (Type II error) means a patient with cancer is told they are healthy and receives no treatment. A false positive (Type I error) means a healthy patient is referred for further investigation. In this context, the Type II error is far more harmful. Clinicians accept a higher Type I error rate (lower specificity) in screening tests to ensure they miss as few true cases as possible.

**Scenario B — Prescribing a treatment with severe side effects:** A false positive (Type I error) means a patient without the target condition receives a powerful drug unnecessarily, exposing them to serious risk. A false negative (Type II error) means a patient who would have benefited does not receive the treatment. Here, the relative costs are more balanced and depend on the magnitude of the side effects.

There is no universal correct answer about which error is worse. The choice of α must be made deliberately, with explicit consideration of what each type of error costs in the specific clinical or public health context.

---

## Section 5: Statistical Power — The Shield Against Type II Errors

### 5.1 The Definition of Power

**Statistical power** is the probability that a study will correctly reject H₀ when H₀ is actually false — that is, the probability of detecting a real effect when one exists.

$$\text{Power} = 1 - \beta$$

If the Type II error rate β = 0.20, then:

$$\text{Power} = 1 - 0.20 = 0.80$$

This means an 80% probability of detecting a real effect. The conventional minimum standard for clinical and public health research is **Power ≥ 0.80** (80%). Studies that fall below this threshold are considered under-powered and their null findings are considered unreliable.

> **Interpreting power in practice:** Suppose a new antibiotic genuinely reduces cholera mortality by 15 percentage points compared to standard care. A study with 80% power means that if we ran this study 100 times under identical conditions, we would expect to find a statistically significant result (and correctly reject H₀) in approximately 80 of those 100 repetitions. In the remaining 20, we would commit a Type II error and incorrectly conclude the drug does not work.

---

### 5.2 The Four Levers of Power

Power can be increased by modifying four factors in the study design:

| What you change | Effect on power | Mechanism |
|---|:---:|---|
| **Increase sample size (n)** | ↑ Increases | Larger samples produce smaller standard errors, making real effects easier to distinguish from noise. This is the primary and most reliable lever. |
| **Increase the effect size** | ↑ Increases | A larger true difference between groups is easier to detect. Effect size cannot always be controlled — it is a property of the intervention — but it informs the minimum detectable effect in a power calculation. |
| **Raise the significance threshold (α)** | ↑ Increases | Accepting a higher false positive rate (e.g., α = 0.10 instead of 0.05) reduces the evidence bar needed to reject H₀, making Type II errors less likely. This is a legitimate design choice only when explicitly justified and pre-specified. |
| **Decrease data variability (s)** | ↑ Increases | Tighter, more homogeneous data produces a larger t-statistic for the same mean difference. This can be achieved through better measurement instruments, stricter eligibility criteria, or paired designs (see Chapter 6). |

---

### 5.3 Power Analysis — A Pre-Study Calculation

A **power analysis** is a calculation performed *before* data collection to determine the minimum sample size required to achieve a specified level of power (typically 80% or 90%), given:
- An assumed significance threshold (α, typically 0.05)
- An expected or minimum clinically important effect size
- An estimate of the variability in the outcome measure (s)
- The desired power (1 − β)

Running a study without performing a power analysis first is the equivalent of planning a journey without calculating whether you have enough fuel. You may arrive — but you may also fall short at a critical point, having consumed resources along the way.

---

### 5.4 The Ethical Dimension of Power Analysis

Power analysis is not merely a methodological requirement. It is an **ethical obligation**.

**Under-powered studies** — those with too few participants to reliably detect the effect of interest — are problematic on multiple grounds:

- They expose participants to the risks of a trial (intervention side effects, time burden, data privacy risks) without a reasonable probability of generating useful knowledge.
- They are likely to report false null findings, which may discourage further research into treatments that actually work.
- They waste institutional resources — funding, laboratory capacity, researcher time — that could have been deployed in adequately powered studies.

**Over-powered studies** — those with far more participants than needed — are also ethically problematic:

- They expose surplus participants to experimental interventions unnecessarily.
- They make it almost certain that even clinically trivial effects will reach statistical significance, conflating statistical significance with clinical importance.

Power analysis finds the exact minimum sample size that makes a study simultaneously rigorous and ethical — large enough to detect a meaningful effect if it exists, but not so large that it becomes exploitative.

---

## Section 6: The One-Sample T-Test

### 6.1 When to Use It

The **one-sample t-test** is used when you wish to compare the mean of a single sample against a known or hypothesised population value. It answers the question:

> "Is our sample mean statistically different from a pre-specified target value?"

This is precisely the scenario we face with Dr Snow's data. In 1854, the average life expectancy for a working-class Londoner was approximately 40 years. We want to know whether cholera victims were a representative cross-section of the London population in terms of age, or whether the outbreak disproportionately killed the young, the old, or a specific age group.

- **H₀:** The true mean age of cholera victims is 40 years (μ = 40). The outbreak shows no age preference.
- **Hₐ:** The true mean age of cholera victims is not 40 years (μ ≠ 40). The outbreak disproportionately affects a particular age group.

---

### 6.2 The T-Statistic

The one-sample t-test computes a **t-statistic**, which measures how far the sample mean lies from the hypothesised population mean, expressed in units of standard error:

$$t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}} = \frac{\bar{x} - \mu_0}{SE}$$

Where:
- **x̄** is the sample mean
- **μ₀** is the hypothesised population mean (the value in H₀)
- **s** is the sample standard deviation
- **n** is the sample size
- **SE** is the standard error (from Chapter 3)

A large t-statistic (positive or negative) means the sample mean is many standard errors away from the hypothesised value — the data is inconsistent with H₀. A small t-statistic means the sample mean is close to the hypothesised value — the data is consistent with H₀.

The t-statistic is then compared against a **t-distribution** with (n − 1) degrees of freedom to obtain the p-value.

> **Degrees of freedom** (df = n − 1) account for the fact that once you know the mean of a sample and the first n − 1 values, the final value is completely determined — it is not free to vary. In effect, you "spend" one degree of freedom estimating the mean, leaving n − 1 for the test. For large samples, the t-distribution approximates the Normal distribution, and the critical value approaches 1.96 (as used for confidence intervals in Chapter 3).

---

### 6.3 Reading the Full Output

When you run a one-sample t-test in PSPP or R, the output contains several important pieces of information:

| Output element | What it tells you |
|---|---|
| **t-statistic** | How many standard errors the sample mean is from the hypothesised value. Large absolute value → small p-value. |
| **Degrees of freedom (df)** | n − 1. Confirms the sample size used in the test. |
| **p-value** | The probability of observing a t-statistic this large by chance if H₀ is true. Compare to α = 0.05. |
| **95% Confidence Interval** | The range of plausible values for the true population mean (from Chapter 3). If this CI does not include μ₀, the result is significant at α = 0.05. |
| **Sample mean (x̄)** | Your point estimate. How far is it from μ₀ = 40? Is this difference clinically meaningful? |

---

## 🔬 Lab Manual — Chapter 5

### Objective

Run a one-sample t-test to determine whether the mean age of cholera victims in Dr Snow's dataset differs significantly from the baseline life expectancy of 40 years for a working-class Londoner in 1854.

**Hypotheses:**
- H₀: μ = 40 (the mean victim age is 40 years — no age preference in the outbreak)
- Hₐ: μ ≠ 40 (the mean victim age differs significantly from 40 — the outbreak shows an age pattern)

---

### Option A — PSPP

**Step-by-step instructions:**

1. Open your `snow_1854_outbreak.sav` file.
2. Go to **Analyze → Compare Means → One-Sample T Test**.
3. In the dialog, find `Age` in the left panel and move it into the **Test Variable(s)** box.
4. At the bottom of the dialog, locate the **Test Value** box. This is where you enter your H₀ value. Type **40**.
5. Click **OK**.

**Reading the output:**

The output contains two tables. Focus on the second table — **One-Sample Test**.

| Column | What to read |
|---|---|
| `t` | The t-statistic. Note whether it is positive (sample mean > 40) or negative (sample mean < 40). |
| `df` | Degrees of freedom. Should equal n − 1. |
| `Sig. (2-tailed)` | Your **p-value**. Compare this to 0.05. |
| `Mean Difference` | x̄ − 40. How many years older or younger than 40 is the average victim? |
| `95% CI of the Difference` | The CI around the mean difference. If this interval does not include 0, the result is significant. |

> **Interpretation guide:** If `Sig. (2-tailed)` < 0.05, you reject H₀ and conclude that victims' ages differ significantly from the general London population baseline. State the direction: were victims significantly younger or older than average?

---

### Option B — R / RStudio

```r
# ---------------------------------------------------------
# Chapter 5 Lab: The One-Sample T-Test
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Ensure the dataset is loaded.
# library(HistData)
# cholera_data <- Snow.deaths

# Step 2: Before running the test, examine the Age variable.
# Check the sample mean and compare it visually to the baseline of 40.
summary(cholera_data$Age)
hist(cholera_data$Age,
     main = "Age Distribution of 1854 Cholera Victims",
     xlab = "Age (Years)",
     col  = "lightblue",
     border = "white")
abline(v = 40, col = "red", lwd = 2, lty = 2)  # mark the H0 baseline

# Step 3: Run the one-sample t-test.
# mu = 40 specifies the hypothesised population mean (H0 value).
# alternative = "two.sided" specifies a two-tailed test (default).
t.test(cholera_data$Age, mu = 40, alternative = "two.sided")

# Step 4: Read the console output carefully.
# R will produce output in this format:
#
#   One Sample t-test
#
#   data:  cholera_data$Age
#   t = [value], df = [n-1], p-value = [value]
#   alternative hypothesis: true mean is not equal to 40
#   95 percent confidence interval:
#    [lower]  [upper]
#   sample estimates:
#   mean of x
#     [x-bar]

# Step 5: Interpret each component.
# - Is the p-value below 0.05? If so, reject H0.
# - Does the 95% CI include 40? If not, the result is significant.
# - What is the sample mean? Is it above or below 40?
# - Is this difference clinically meaningful, or merely statistically significant?

# Step 6: Extract and display the key values clearly.
result <- t.test(cholera_data$Age, mu = 40)

cat("Sample mean age:  ", round(result$estimate, 2), "years\n")
cat("t-statistic:      ", round(result$statistic, 3), "\n")
cat("Degrees of freedom:", result$parameter, "\n")
cat("p-value:          ", round(result$p.value, 4), "\n")
cat("95% CI:           [",
    round(result$conf.int[1], 2), ",",
    round(result$conf.int[2], 2), "]\n")

# Step 7: Check the normality of Age before accepting the t-test result.
# The one-sample t-test assumes approximate Normality in the outcome variable.
shapiro.test(cholera_data$Age)
# If p < 0.05, the Age distribution departs from Normality.
# For large n (> 30), the t-test is robust to modest non-Normality
# due to the Central Limit Theorem. Report the Shapiro-Wilk result
# and note whether it raises concerns.
```

**What to record and report:**

Write a single paragraph summarising the test results in plain language, structured as follows:

1. State the research question and the hypotheses (H₀ and Hₐ).
2. Report the test statistics: t(df) = [value], p = [value].
3. State the decision: reject or fail to reject H₀.
4. Report the 95% CI for the mean.
5. Interpret the finding in the context of the 1854 outbreak: were victims systematically younger or older than the general population? What might this suggest about who was most exposed to the pump water?

---

---

### 🧪 Test Your Knowledge

A public health team claims that the mean age of cholera victims at the Broad Street pump was greater than 35 years. Using the Snow dataset, **Task:** (a) State H₀ and Hₐ formally. (b) Write the R code to run the one-sample t-test against μ₀ = 35. (c) Interpret the output: t, df, p-value, and 95% CI. (d) State your conclusion in plain language for a non-statistical public health audience.

```{dropdown} Show Solution
```r
# (a) Hypotheses
# H₀: μ = 35  (mean age of Broad Street victims is 35 years)
# Hₐ: μ > 35  (mean age is greater than 35 years) — one-tailed

# (b) R code (one-tailed test — alternative = "greater")
broad_victims <- subset(cholera_data, Pump_Used == "Broad St")
result <- t.test(broad_victims$Age, mu = 35, alternative = "greater")
result

# (c) Reading the output:
# t         = test statistic (positive = sample mean > 35)
# df        = n - 1
# p-value   = compare to 0.05 (one-tailed)
# conf.int  = one-sided lower bound for the mean

# (d) Plain-language conclusion example:
# "The data provide [sufficient / insufficient] evidence to conclude that 
#  the mean age of Broad Street pump victims exceeded 35 years 
#  (t([df]) = [t], p = [p-value])." 
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Target population** | The complete group about which a researcher wishes to draw conclusions. The population to which the findings should generalise. |
| **Sampling frame** | The practical list or enumeration from which the sample is actually drawn. Should match the target population as closely as possible. |
| **Probability sampling** | Any sampling design in which every individual in the sampling frame has a known, non-zero probability of selection. Required for valid inferential statistics. |
| **Simple random sampling** | A probability design in which every individual has an equal chance of selection, typically using a random number generator. |
| **Stratified random sampling** | A probability design in which the population is divided into mutually exclusive strata before random sampling is conducted within each stratum. Improves representation of important subgroups. |
| **Systematic sampling** | A probability design in which every k-th individual is selected from a sorted sampling frame, with a random starting point. |
| **Cluster sampling** | A probability design in which naturally occurring groups (clusters) are randomly selected and all individuals within selected clusters are surveyed. Practical for geographically dispersed populations. |
| **Convenience sampling** | A non-probability design using whoever is available. Prone to selection bias; p-values and confidence intervals have limited generalisability. |
| **Coverage error** | The gap between the target population and the sampling frame — individuals who belong to the target population but cannot be selected because they are absent from the frame. | | The default assumption that no real effect or difference exists; the position of scepticism that must be overcome by evidence. |
| **Alternative Hypothesis (Hₐ)** | The researcher's claim that a real, measurable effect or difference exists in the population. Supported indirectly when H₀ is rejected. |
| **p-value** | The probability of observing a test statistic at least as extreme as the one computed, assuming H₀ is true. A small p-value is evidence against H₀. |
| **Significance threshold (α)** | The pre-specified probability below which H₀ is rejected. Conventionally set at 0.05 in health sciences, corresponding to a 5% false positive rate. |
| **Type I error (α)** | Rejecting H₀ when it is actually true — a false positive. Its probability is controlled by the choice of α. |
| **Type II error (β)** | Failing to reject H₀ when it is actually false — a false negative. Its probability is reduced by increasing statistical power. |
| **Statistical power (1 − β)** | The probability that a study correctly rejects H₀ when it is false — the probability of detecting a real effect. The conventional minimum standard is 0.80 (80%). |
| **Power analysis** | A pre-study calculation to determine the minimum sample size required to achieve a target level of statistical power, given the significance threshold, expected effect size, and data variability. |
| **One-sample t-test** | A hypothesis test that compares the mean of a single sample against a known or pre-specified population value. Produces a t-statistic, degrees of freedom, and p-value. |
| **Two-tailed test** | A hypothesis test that considers differences in both directions — the alternative hypothesis states only that the mean is different (not specifically higher or lower). Standard for most clinical research. |

---

## Review Questions

1. A public health team is investigating a gastrointestinal illness outbreak affecting inpatients across 12 wards in a 800-bed hospital. They wish to interview a sample of 120 patients. (a) Describe how they would conduct a stratified random sample using ward as the stratifying variable, including how many patients would be selected per ward. (b) Explain why stratified sampling is preferable to simple random sampling in this context. (c) Identify one form of coverage error likely to affect their sampling frame.

2. Write the Null and Alternative Hypotheses, using correct statistical notation, for the following public health question: *"Does a new oral rehydration therapy reduce the seven-day mortality rate of cholera patients compared to the standard treatment?"*

3. A study comparing two groups returns p = 0.048. Write a precisely worded one-sentence conclusion. Then explain, with specific reference to the definition of the p-value, why this result crosses the conventional threshold for statistical significance.

4. A large randomised controlled trial of a new antihypertensive drug finds p = 0.003. The mean reduction in systolic blood pressure is 1.1 mmHg. Explain why this finding may be statistically significant but clinically unimportant. What additional information would you need to decide whether to recommend the drug?

5. Explain the difference between a Type I error and a Type II error using a public health example of your own that does not involve the Broad Street pump. For each error, state the consequence and explain how it is controlled.

6. A researcher conducts a trial with n = 15 patients and reports that a new antibiotic showed no statistically significant effect on recovery time (p = 0.12). They conclude that the drug does not work. What alternative explanation should they consider before drawing this conclusion? What should they calculate to evaluate this explanation?

7. Four independent clinical studies all investigate the same new vaccine. Study A has n = 20, Study B has n = 100, Study C has n = 500, and Study D has n = 2,000. If the vaccine provides a small but genuine protective effect:
   - (a) Which study is most likely to detect the effect?
   - (b) Which study is most likely to commit a Type II error?
   - (c) If Study D reports p = 0.03 with a risk reduction of 0.8 percentage points, does this finding warrant recommending universal vaccination? Justify your answer with reference to the distinction between statistical and clinical significance.

---

```{admonition} Key Takeaways
:class: tip

- **Sampling:** Simple random, stratified, systematic, and cluster are probability methods — required for valid inference. Convenience sampling cannot guarantee this.
- **H₀ / Hₐ:** The null hypothesis asserts no effect. The alternative asserts the effect of interest. We test against H₀; we never prove Hₐ.
- **P-value:** The probability of observing a result at least this extreme *if H₀ were true*. It is NOT the probability H₀ is true.
- **α = 0.05:** The pre-set threshold. If p < α, reject H₀. The threshold is set *before* seeing the data.
- **Type I error (α):** Rejecting H₀ when it is true (false positive). **Type II error (β):** Failing to reject H₀ when it is false (false negative).
- **Power = 1 − β.** Increased by: larger n, larger effect size, larger α, lower σ.
- **One-sample t-statistic:** t = (x̄ − μ₀) / (s / √n), with df = n − 1.
```

## Chapter Summary

Hypothesis testing is the formal procedure by which biostatistics transforms a scientific question into a defensible decision. Its structure — a null hypothesis presuming no effect, a test statistic, a p-value compared against a pre-specified threshold — is designed to impose intellectual rigour on what would otherwise be a subjective judgement.

The p-value is the most frequently misinterpreted concept in health sciences. It is a conditional probability — the probability of the data given H₀ — not a probability about the truth of H₀ itself. It is not a measure of clinical importance. And a result that fails to reach p < 0.05 is not a proof that no effect exists.

The two error types — Type I (false positive, controlled by α) and Type II (false negative, reduced by increasing power) — are in direct tension. The correct balance between them depends on the clinical consequences of each error in the specific context of the study.

Statistical power is the probability of detecting a real effect, and ensuring adequate power before a study begins is both a methodological and an ethical requirement. Under-powered studies generate unreliable null findings and waste the resources and goodwill of participants.

In Chapter 6, we extend hypothesis testing to the comparison of two groups — the most common analytical structure in clinical research — using the independent samples and paired samples t-tests.

---

*Next: **Chapter 6 — A Tale of Two Groups:** Comparing Means with T-Tests*

---

> Part II — Making Decisions Under Uncertainty
