# Appendix B — Glossary

All terms defined in the Key Terms tables throughout the book are consolidated here in alphabetical order. The chapter where each term is first introduced is noted in brackets.

Use this page as a quick lookup when you encounter an unfamiliar term, or as a self-test tool: cover the Definition column and test your recall of each term before an examination.

---

## A

**Alternative Hypothesis (Hₐ)** [Ch. 5]
The research hypothesis — the claim the investigator is trying to find evidence for. States that there *is* an effect, a difference, or an association. It is accepted only when the data provide sufficient evidence to reject H₀.

**ANOVA (Analysis of Variance)** [Ch. 7]
A hypothesis test that compares the means of three or more independent groups on a continuous outcome variable. Decomposes total variance into between-group variance (signal) and within-group variance (noise), producing the F-statistic.

---

## B

**Between-group variance** [Ch. 7]
The component of total variance in ANOVA attributable to differences between group means. Represents the signal — the systematic effect of group membership on the outcome.

**Binomial distribution** [Ch. 4]
A discrete probability distribution that models the number of successes in a fixed number of independent trials (n), each with the same probability of success (p). Used for binary outcomes such as infected / not infected.

**Bonferroni correction** [Ch. 7]
A method for controlling the family-wise Type I error rate when running k simultaneous hypothesis tests. Divides the significance threshold by k: α_corrected = α / k. Produces conservative (wide) adjusted p-values.

---

## C

**Categorical data** [Ch. 1]
Data that places each observation into a named group or category rather than measuring it on a numerical scale. Includes nominal and ordinal variables. Cannot be meaningfully averaged.

**Censored observation** [Ch. 8]
A survival analysis term for a participant whose event of interest has not occurred by the end of the study period. Their survival time is known to be at least a certain length, but the exact time of the event remains unknown.

**Central Limit Theorem (CLT)** [Ch. 4]
The mathematical theorem stating that the sampling distribution of the sample mean (x̄) approaches a Normal distribution as sample size increases, regardless of the population's underlying distribution. Justifies Normal-based inference for n ≥ 30.

**Chi-Square statistic (χ²)** [Ch. 7]
A test statistic measuring the overall discrepancy between observed and expected cell frequencies in a contingency table. Computed as $\chi^2 = \sum(O - E)^2/E$. Used in the Chi-Square Test for Independence.

**Chi-Square Test for Independence** [Ch. 7]
A hypothesis test that assesses whether two categorical variables are statistically independent, comparing the pattern of observed frequencies against the pattern expected under H₀ of no association.

**Cluster sampling** [Ch. 5]
A probability sampling design in which naturally occurring groups (clusters) are randomly selected, and all individuals within selected clusters are surveyed. Practical for geographically dispersed populations; requires larger samples due to intra-cluster correlation.

**Coefficient of Determination (R²)** [Ch. 8]
The proportion of variance in the outcome variable (y) explained by the predictor variable (x) in a linear regression model. Ranges from 0 to 1. Calculated as $R^2 = r^2$. Multiply by 100 to express as a percentage.

**Confidence Interval (CI)** [Ch. 3]
A range of values constructed from a sample that, over many repeated samples, would contain the true population parameter the stated percentage of the time (e.g., 95%). Formula for a 95% CI: $\bar{x} \pm 1.96 \times SE$.

**Confounding variable** [Ch. 8]
A variable associated with both the predictor and the outcome that creates a spurious or distorted apparent relationship between them. Confounding is the primary reason correlation does not imply causation.

**Contingency table** [Ch. 7]
A grid displaying the joint frequency distribution of two categorical variables, where each row represents one category of Variable 1, each column represents one category of Variable 2, and each cell contains the count of observations falling into that combination.

**Continuous data** [Ch. 1]
Data that measures a genuine numerical quantity on a scale where the distances between values are real and arithmetic operations (addition, subtraction, averaging) produce meaningful results. Includes interval and ratio variables.

**Convenience sampling** [Ch. 5]
A non-probability sampling design that uses whoever is available and willing at the time of data collection. Prone to selection bias; p-values and confidence intervals from convenience samples have limited generalisability.

**Coverage error** [Ch. 5]
The gap between the target population and the sampling frame — individuals who belong to the target population but cannot be selected because they are absent from the frame.

**Cramér's V** [Ch. 7]
An effect size measure for the Chi-Square Test for Independence. Ranges from 0 (no association) to 1 (perfect association). Interpretation scale: < 0.10 negligible; 0.10–0.30 small; 0.30–0.50 moderate; > 0.50 large.

---

## D

**Degrees of freedom (df)** [Ch. 5]
The number of independent pieces of information available to estimate a parameter. For a one-sample t-test: df = n − 1. For an independent samples t-test: df ≈ n₁ + n₂ − 2 (or the Welch approximation). For Chi-Square: df = (rows − 1)(columns − 1).

---

## E

**Effect size** [Ch. 5, 6, 7, 8]
A standardised measure of the magnitude of a statistical result, independent of sample size. Examples: Cohen's d (t-tests), η² (ANOVA), Cramér's V (Chi-Square), r (correlation), R² (regression). Reports *how big* the effect is, not merely whether it is statistically significant.

**Expected count (E)** [Ch. 7]
The cell frequency predicted under the Null Hypothesis of independence in a Chi-Square test. Calculated as E = (Row total × Column total) / Grand total. All expected counts must be ≥ 5 for the Chi-Square approximation to be valid.

**Experimental design** [Ch. 1]
A study design (such as a Randomized Controlled Trial) in which researchers actively intervene and control the assignment of treatments to test efficacy and establish cause-and-effect.

**Extrapolation** [Ch. 8]
Using a regression model to make predictions for values of x outside the range of the original data. Unreliable because the linear relationship observed within the data range may not extend beyond it.

---

## F

**F-statistic** [Ch. 7]
The test statistic produced by ANOVA. Defined as the ratio of between-group variance (MS_Between) to within-group variance (MS_Within). Values substantially greater than 1 indicate group separation. Reported as F(df_between, df_within).

**Family-wise error rate (FWER)** [Ch. 7]
The probability of making at least one Type I error across a family of k simultaneous hypothesis tests. Quantified as $FWER = 1 - (1 - \alpha)^k$.

**Fisher's Exact Test** [Ch. 7]
An exact alternative to the Chi-Square test used when any expected cell count falls below 5. Computes the exact probability of the observed table without large-sample assumptions. Valid regardless of table size.

---

## H

**Homoscedasticity** [Ch. 8]
The regression assumption that the variance of residuals is constant across all fitted values. Checked using the Residuals vs Fitted diagnostic plot — a flat horizontal band indicates homoscedasticity.

**Hypothesis testing** [Ch. 5]
A formal statistical framework for making decisions about population parameters based on sample data. Sets up a Null Hypothesis (H₀) and an Alternative Hypothesis (Hₐ), computes a test statistic, and uses its p-value to decide whether to reject H₀.

---

## I

**Independent Samples T-Test** [Ch. 6]
A hypothesis test that compares the means of two separate, unrelated groups on a continuous outcome variable. Requires approximately Normal distributions within each group and checks equal variances with Levene's Test.

**Inferential statistics** [Ch. 3]
Statistical methods that use sample data to draw conclusions (inferences) about a larger population. Require that the sample was obtained through a probability sampling design to be formally valid.

**Interval data** [Ch. 1]
A continuous measurement scale with equal, meaningful distances between values but no true zero point. Ratios between values are not meaningful. Example: temperature in degrees Celsius.

---

## K

**Kaplan-Meier estimator** [Ch. 8]
A non-parametric method used to estimate the survival probability over time. It correctly accounts for censored observations by recalculating the probability of survival each time an event occurs.

---

## L

**Levene's Test** [Ch. 6]
A hypothesis test for the equality of variances across two or more groups. Used before an independent samples t-test. p > 0.05 → equal variances assumed (use Student's t-test); p < 0.05 → use Welch's correction.

**Level of measurement** [Ch. 1]
The formal classification of a variable (nominal, ordinal, interval, or ratio) that determines which mathematical operations may legitimately be applied to it and which statistical tests are appropriate.

**Log-rank test** [Ch. 8]
A hypothesis test used to compare the survival curves (generated by the Kaplan-Meier estimator) of two or more independent groups to see if their survival experiences significantly differ.

---

## M

**Mean (x̄)** [Ch. 2]
The arithmetic average of a set of values: x̄ = Σxᵢ / n. The most common measure of central tendency. Sensitive to outliers; best for symmetric distributions.

**Median** [Ch. 2]
The middle value of a sorted dataset. Divides the distribution into two equal halves. Resistant to outliers; best for skewed distributions.

**Mode** [Ch. 2]
The most frequently occurring value in a dataset. The only valid measure of central tendency for nominal data.

**Multiple comparisons problem** [Ch. 7]
The inflation of the family-wise Type I error rate that occurs when performing many simultaneous hypothesis tests. At k = 14 tests with α = 0.05, the probability of at least one false positive exceeds 50%.

---

## N

**Negative Predictive Value (NPV)** [Ch. 9]
The probability that a patient who tests negative for a disease truly does not have the disease. Highly dependent on the prevalence of the condition in the population.

**Nominal data** [Ch. 1]
The lowest level of measurement. Data that places observations into named categories with no natural rank order. Examples: blood type, pump name, diagnosis code. Only mode is a valid central tendency measure.

**Normal distribution** [Ch. 4]
A continuous, symmetric, bell-shaped probability distribution fully defined by its mean (μ) and standard deviation (σ). The Empirical Rule: 68% of values fall within ±1σ, 95% within ±1.96σ, 99.7% within ±3σ.

**Null Hypothesis (H₀)** [Ch. 5]
The default hypothesis of no effect, no difference, or no association. Assumed to be true until the data provide sufficient evidence to reject it.

**Number Needed to Treat / Harm (NNT / NNH)** [Ch. 7]
An epidemiological measure indicating how many people must receive an intervention (or be exposed to a risk factor) to prevent (or cause) one additional outcome event. Calculated as $1 / RD$.

---

## O

**Observational design** [Ch. 1]
A study design (such as a prospective cohort) in which researchers monitor participants over time without intervening. Used to identify natural risk factors and associations.

**Odds Ratio (OR)** [Ch. 7]
A measure of association comparing the odds of an outcome in an exposed group to the odds in an unexposed group. Used primarily in case-control studies where relative risk cannot be calculated.

**Ordinal data** [Ch. 1]
A level of measurement where categories have a natural rank order but the distances between ranks are not equal or measurable. Examples: pain severity scale, disease stage rating. Median is the appropriate central tendency measure.

---

## P

**P-value** [Ch. 5]
The probability of observing a test statistic at least as extreme as the one calculated, *assuming the Null Hypothesis is true*. The p-value is NOT the probability that H₀ is true. It does NOT measure the probability that the result occurred by chance.

**Paired Samples T-Test** [Ch. 6]
A hypothesis test that compares two measurements from the same individuals (before/after, or matched pairs). Analyses the difference scores (d = x₁ − x₂). Increases power by removing between-person variability from the error term.

**Pearson's correlation coefficient (r)** [Ch. 8]
A measure of the strength and direction of the linear relationship between two continuous variables. Ranges from −1 (perfect negative linear relationship) to +1 (perfect positive linear relationship). r = 0 indicates no linear relationship.

**Poisson distribution** [Ch. 4]
A discrete probability distribution that models the count of rare, independent events occurring in a fixed time period or area, with a known average rate λ. Used for disease case counts per week or per district.

**Pooled proportion (p̂)** [Ch. 7]
The overall proportion across both groups combined, used as the best estimate of the common population proportion under H₀ in a two-proportion z-test. Calculated as p̂ = (a + c) / n.

**Population** [Ch. 3]
The complete set of individuals or units about which a researcher wishes to draw conclusions. Population parameters are denoted with Greek letters (μ, σ, ρ).

**Positive Predictive Value (PPV)** [Ch. 9]
The probability that a patient who tests positive for a disease truly has the disease. Dependent on the underlying prevalence of the condition in the screened population.

**Post-hoc test** [Ch. 7]
A follow-up analysis run after a significant ANOVA to identify which specific pairs of groups differ significantly. Must be run only after a significant omnibus F-test.

**Power (1 − β)** [Ch. 5]
The probability of correctly rejecting H₀ when it is false — correctly detecting a real effect. Increased by: larger sample size, larger effect size, higher α, lower population variance. Conventional target: ≥ 0.80.

**Probability sampling** [Ch. 5]
Any sampling design in which every individual in the sampling frame has a known, non-zero probability of selection. Required for the formal validity of inferential statistics.

---

## R

**Ratio data** [Ch. 1]
A continuous measurement scale with equal distances between values *and* a true zero representing complete absence of the quantity. Both differences and ratios between values are meaningful. Examples: age, weight, death count, distance.

**Regression equation** [Ch. 8]
The mathematical expression of the linear relationship between predictor (x) and outcome (y): ŷ = β₀ + β₁x, where β₀ is the y-intercept and β₁ is the slope.

**Relative Risk (RR)** [Ch. 7]
The ratio of the outcome proportion in the exposed group to the outcome proportion in the unexposed group: RR = p₁ / p₂. The primary effect-size measure in cohort studies and outbreak investigations. RR = 1 means no association; RR > 1 means increased risk in the exposed group.

**Residual** [Ch. 8]
The difference between an observed value and the value predicted by the regression model: e = y − ŷ. Residuals should be approximately Normally distributed and have constant variance for a valid regression.

**Risk Difference (RD)** [Ch. 7]
The absolute difference between two proportions: RD = p₁ − p₂. Expresses the excess risk attributable to the exposure in percentage-point terms.

---

## S

**Sample** [Ch. 3]
A subset of individuals drawn from a population to represent it. Sample statistics are denoted with Roman letters (x̄, s, r).

**Sampling frame** [Ch. 5]
The practical list or enumeration from which the sample is actually drawn. Should match the target population as closely as possible.

**Sensitivity (Se)** [Ch. 9]
The proportion of actual positive cases (people who truly have the disease) that are correctly identified as positive by a diagnostic test.

**Shapiro-Wilk test** [Ch. 4]
A formal hypothesis test of Normality. H₀: the data are Normally distributed. p > 0.05 → do not reject Normality (proceed with parametric tests). p < 0.05 → evidence of non-Normality (consider non-parametric alternatives).

**Simple linear regression** [Ch. 8]
A statistical method that models the linear relationship between one continuous predictor variable (x) and one continuous outcome variable (y), producing a regression equation ŷ = β₀ + β₁x.

**Simple random sampling** [Ch. 5]
A probability sampling design in which every individual in the sampling frame has an equal probability of selection, typically using a random number generator.

**Skewness** [Ch. 2]
A measure of the asymmetry of a distribution. Positive (right) skew: long tail on the right; mean > median. Negative (left) skew: long tail on the left; mean < median.

**Spearman's rho (rs)** [Ch. 8]
A non-parametric alternative to Pearson's r that measures the strength of a monotonic (not necessarily linear) relationship between two variables. Appropriate when Normality is violated or the relationship is non-linear.

**Specificity (Sp)** [Ch. 9]
The proportion of actual negative cases (people who truly do not have the disease) that are correctly identified as negative by a diagnostic test.

**Standard Deviation (s)** [Ch. 2]
The square root of the variance: s = √s². Measures spread in the same units as the original variable. The most interpretable measure of variability for a symmetric distribution.

**Standard Error (SE)** [Ch. 3]
The standard deviation of the sampling distribution of the mean: SE = s / √n. Measures how precisely the sample mean estimates the true population mean. Decreases as n increases.

**Stratified random sampling** [Ch. 5]
A probability sampling design in which the population is first divided into mutually exclusive strata, and a random sample is drawn independently from each stratum. Improves representation of key subgroups.

**Systematic sampling** [Ch. 5]
A probability sampling design in which every k-th individual is selected from a sorted sampling frame, where k = population size / desired sample size, with a random starting point.

---

## T

**Target population** [Ch. 5]
The complete group about which a researcher wishes to draw conclusions and to whom the results should generalise.

**True zero** [Ch. 1]
A zero value that represents the complete absence of the quantity being measured, not an arbitrary reference point. The defining property of ratio (vs. interval) measurement.

**Tukey's HSD (Honestly Significant Difference)** [Ch. 7]
The most widely used post-hoc test following a significant one-way ANOVA. Compares all possible group pairs whilst controlling the family-wise Type I error rate at α. Assumes equal group sizes and equal variances.

**Two-proportion z-test** [Ch. 7]
A hypothesis test comparing the outcome proportions from two independent groups, testing H₀: p₁ = p₂. Uses the pooled proportion under H₀ to construct the standard error.

**Type I error (α)** [Ch. 5]
Rejecting H₀ when it is actually true — a false positive. The probability of committing a Type I error equals the significance level α. Set by the researcher before data collection.

**Type II error (β)** [Ch. 5]
Failing to reject H₀ when it is actually false — a false negative. The probability of committing a Type II error. Reduced by increasing sample size, effect size, or α.

---

## V

**Variance (s²)** [Ch. 2]
The average squared deviation of each observation from the sample mean, using Bessel's correction: s² = Σ(xᵢ − x̄)² / (n − 1). Measured in squared units of the original variable.

---

## W

**Welch's T-Test** [Ch. 6]
A modification of the independent samples t-test that does not assume equal group variances. Uses the Satterthwaite approximation to produce a non-integer degrees of freedom. The safer default when equal variances cannot be confirmed.

**Within-group variance** [Ch. 7]
The component of total variance in ANOVA attributable to variability of individual observations around their own group mean — the noise. Reflects natural individual differences unrelated to group membership.

---

## Z

**Z-score** [Ch. 4]
The number of standard deviations a given value lies from the population mean: Z = (x − μ) / σ. A Z-score of 0 equals the mean; Z = +1.96 marks the boundary of the central 95% of a Normal distribution.