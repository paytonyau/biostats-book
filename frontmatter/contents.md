# Table of Contents

---

## Front Matter

- [Foreword](../frontmatter/foreword.md)
- [Preface: Why Numbers Need Health Professionals](../frontmatter/preface.md)
- [Copyright and Citation](../frontmatter/copyright.md)
- [Dedication and Acknowledgments](../frontmatter/acknowledgments.md)
- [About the Author](../frontmatter/about_author.md)

---

## Part I — The Anatomy of Population Data

*Chapters 1–3 establish the foundations: what data is, how to describe it, and how to quantify our uncertainty about it. Every concept is grounded in the Snow 1854 cholera dataset, which is used throughout the entire book.*

---

### Chapter 1 — The Vitals of Data
*Understanding Types of Data and Levels of Measurement*

By the end of this chapter, you will be able to:

- Distinguish between **categorical** and **continuous** data
- Identify and apply the four levels of measurement: nominal, ordinal, interval, and ratio
- Classify every variable in the Snow 1854 dataset at the correct level of measurement
- Set up a dataset in PSPP and load `Snow.deaths` in R

---

### Chapter 2 — The Middle and the Mess
*Measures of Central Tendency and Spread*

By the end of this chapter, you will be able to:

- Calculate and interpret the **mean**, **median**, and **mode** for a health dataset
- Calculate the **range**, **variance** (with Bessel's correction), and **standard deviation**
- Choose the appropriate measure of central tendency based on distribution shape
- Produce descriptive statistics in PSPP and R and interpret the output table

---

### Chapter 3 — The Margin of Error
*Sampling, Standard Error, and Confidence Intervals*

By the end of this chapter, you will be able to:

- Distinguish between a **population parameter** and a **sample statistic**
- Calculate the **Standard Error** of the mean and explain what it measures
- Construct and correctly interpret a **95% Confidence Interval** for a population mean
- Identify and avoid the three most common misinterpretations of a confidence interval

---

## Part II — Making Decisions Under Uncertainty

*Chapters 4–6 introduce probability, formal hypothesis testing, and group comparison. These chapters form the statistical core of the course and are directly assessed in the HS 4510 unit examinations.*

---

### Chapter 4 — The Laws of Chance
*Probability Distributions and the Normal Curve*

By the end of this chapter, you will be able to:

- Describe the properties of the **Normal distribution** and apply the 68–95–99.7 rule
- Convert a raw score to a **Z-score** and use it to find probabilities
- Distinguish between the **Binomial** and **Poisson** distributions and identify when each applies
- State the **Central Limit Theorem** and explain why it justifies using Normal-based inference on real health data

---

### Chapter 5 — The Hypothesis Gamble
*Sampling Methods, Study Design, and Hypothesis Testing*

By the end of this chapter, you will be able to:

- Explain the four main **probability sampling methods** and state why sampling design determines the validity of inference
- State a **Null and Alternative Hypothesis** using correct statistical notation
- Define the **p-value** precisely and identify the four most common misinterpretations
- Distinguish **Type I** and **Type II errors**, define **statistical power**, and identify the four levers that increase it

---

### Chapter 6 — A Tale of Two Groups
*Independent and Paired T-Tests*

By the end of this chapter, you will be able to:

- Select the correct t-test design — independent or paired — from a study description
- Apply **Levene's Test** to check equal variances before an independent t-test
- Run and interpret both t-tests in PSPP and R, including the mean difference and 95% CI
- Explain why pairing increases statistical power and identify when pairing is appropriate

---

## Part III — Complex Connections

*Chapters 7–8 extend from two groups to many groups, from counts to proportions, and from correlation to prediction. These methods are the backbone of outbreak investigation and epidemiological research.*

---

### Chapter 7 — Scaling Up
*ANOVA, Proportions, and Chi-Square*

By the end of this chapter, you will be able to:

- Explain the **multiple comparisons problem** and calculate the family-wise error rate for k tests
- Run a **one-way ANOVA** and follow up a significant result with **Tukey's HSD**
- Compute **Risk Difference** and **Relative Risk** from a 2×2 table and run a **two-proportion z-test**
- Build a contingency table and run a **Chi-Square test**, checking expected-count assumptions

---

### Chapter 8 — Reading the Future
*Correlation and Simple Linear Regression*

By the end of this chapter, you will be able to:

- Interpret **Pearson's r** using magnitude and direction, and test its statistical significance
- State the assumptions of simple linear regression and check them using diagnostic plots
- Interpret the **regression equation** ŷ = β₀ + β₁x, including slope, intercept, and R²
- Make a prediction with a 95% prediction interval and explain the danger of extrapolation

---

## Part IV — Consolidation and Examination Preparation

*Chapter 9 contains no new theory. It is a dedicated revision resource: a complete formula reference, a test-selection decision guide, and full software cheat sheets for both PSPP and R.*

---

### Chapter 9 — Course Review and Examination Preparation
*Formula Reference, Test Selection Guide, and Software Cheat Sheets*

By the end of this chapter, you will be able to:

- Locate any formula from Chapters 1–8 in a single consolidated reference table
- Select the correct statistical test for a given research question using a structured decision guide
- Identify the appropriate effect size measure for each test covered in the course
- Execute any analysis from the course in both PSPP and R using the cheat sheet as a reference

---

## Appendices

---

### Appendix A — Data Dictionary
*The Master Dataset: Snow.deaths (1854 Broad Street Cholera Outbreak)*

A complete codebook for every variable used in this book — the three base variables from `Snow.deaths` and every extended variable added in lab exercises across Chapters 1–8. Includes loading instructions for R and PSPP, a common coding errors table, and a data provenance note.

---

### Appendix B — Glossary
*All Key Terms, Alphabetically*

Every key term introduced across all eight content chapters, consolidated in a single alphabetical reference. 55 entries from *Alternative Hypothesis* to *Z-score*, each with a full definition and the chapter of first appearance. Designed for quick lookup and pre-examination self-testing.

---

## Back Matter

- [References and Further Reading](../backmatter/references.md)
- [How to Contribute](../backmatter/contribute.md)
