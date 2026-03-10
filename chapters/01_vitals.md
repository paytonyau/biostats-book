# A Little Bit of Everything in Biostatistics for Health Science Students

---

# Chapter 1: The Vitals of Data
## *Understanding Types of Data and Levels of Measurement*

> **Course:** HS 4510 Biostatistics | **Unit:** 1 | **Part:** I — Describing the World in Numbers
>
> **Dataset used throughout this chapter:** Dr John Snow's 1854 Broad Street cholera outbreak — `Snow.deaths` from the R package `HistData`.

---

```{admonition} Learning Objectives
:class: note

By the end of this chapter, you will be able to:

- Distinguish between **categorical** and **continuous** data
- Identify and apply the four levels of measurement: nominal, ordinal, interval, and ratio
- Classify every variable in the Snow 1854 dataset at the correct level of measurement
- Set up a dataset in PSPP and load `Snow.deaths` in R
```


---

## Why This Chapter Comes First

Before a single mean is calculated, before a single p-value is interpreted, a clinician, an epidemiologist, or a public health analyst must answer one deceptively simple question:

**What kind of data do I actually have?**

This question is not procedural housekeeping. It is the foundation on which every subsequent decision rests. The statistical tests you may use in Chapters 5 through 8 — t-tests, ANOVA, regression — are each built for a specific type of data. Apply the wrong test to the wrong data type and the software will still produce a number. That number will simply be wrong, and in a clinical or public health setting, a wrong number can harm or kill people.

In 1854, Dr John Snow recorded every cholera death in the Soho district of London and noted the address of each victim. His dataset was not glamorous. It contained street names, pump locations, and death counts. Yet from that simple, carefully categorised data, he identified the contaminated Broad Street water pump and ended an outbreak that had already killed over 600 people in ten days. He did not succeed because he had sophisticated software. He succeeded because he understood exactly what his data represented and what it could legitimately be used to claim.

We begin there.

---

## Section 1: The Two Families of Data

All data in health sciences belongs to one of two fundamental families: **categorical** or **continuous**. This is the first and most important classification you will make when you open any dataset.

### 1.1 Categorical Data

Categorical data places each observation into a named group or category. The categories may have labels, codes, or names, but the values themselves do not represent a measurable quantity along a scale. You cannot perform arithmetic on them in a meaningful way.

**Key diagnostic question:** *Can I calculate a meaningful average of this variable?*

If the answer is no, the variable is categorical.

**Examples from the Snow dataset:**

- **Pump_Used** — which pump a household drew water from (`Broad St`, `Rupert St`, `Marlborough Mews`). There is no sensible arithmetic average of a list of pump names.
- **Gender** — recorded as `Male` or `Female`. The average of `Male` and `Female` is not a useful number.
- **Neighbourhood** — the sub-district of Soho in which a death occurred.

Within the categorical family, there is a crucial distinction between two sub-types.

---

### 1.2 Nominal Data

**Nominal** data is categorical data in which the categories have no natural order or ranking. The word comes from the Latin *nomen*, meaning name. The categories are simply labels for groups that are different from one another, but not better, worse, higher, or lower.

| Property | Nominal data |
|---|---|
| Categories exist | ✓ Yes |
| Categories have a natural rank or order | ✗ No |
| Differences between categories are measurable | ✗ No |
| A meaningful zero exists | ✗ No |

**Examples:**
- Blood type: A, B, AB, O. Type AB is not "more blood" than type O. There is no ranking.
- Cause of death: `Cholera`, `Typhoid`, `Smallpox`. These are names for different conditions, not positions on a scale.
- Pump used by a household: `Broad St` vs `Rupert St`. There is no numerical relationship between the two pump labels.

> **A common error to avoid:** Nominal categories are often stored as numbers in a dataset for coding convenience — for example, Male = 1, Female = 2. The numbers are still nominal. Calculating the average of 1 and 2 and reporting it as 1.5 is meaningless. The software does not know this. You must know it.

---

### 1.3 Ordinal Data

**Ordinal** data is categorical data in which the categories *do* have a natural, meaningful rank or order. However, the *distances* between ranks are not equal and cannot be assumed to be so.

| Property | Ordinal data |
|---|---|
| Categories exist | ✓ Yes |
| Categories have a natural rank or order | ✓ Yes |
| Distances between ranks are equal and measurable | ✗ No |
| A meaningful zero exists | ✗ No |

**Examples:**
- Pain severity: `Mild`, `Moderate`, `Severe`. "Severe" is worse than "Moderate", but we cannot say it is precisely twice as bad.
- Socioeconomic status: `Low`, `Middle`, `High income`. There is a rank, but the gap between Low and Middle is not the same as the gap between Middle and High.
- Glasgow Coma Scale scores: ranked from 3 to 15, but the clinical difference between a score of 3 and 4 is not identical to the difference between 12 and 13.

> **The critical insight:** With ordinal data, you know the *direction* of a difference but not its *size*. A patient who rates their pain as 8 out of 10 is in more pain than a patient who rates it 4 out of 10, but is not necessarily in twice as much pain.

---

### 1.4 Continuous Data

Continuous data records a measurement along a genuine numerical scale. The values represent actual quantities, arithmetic operations on them produce meaningful results, and the distance between any two values is real and interpretable.

**Key diagnostic question:** *Does this number represent a real measurement on a scale where the gaps between values are equal and meaningful?*

If the answer is yes, the variable is continuous.

**Examples from the Snow dataset:**

- **Age** — the age of each victim in years. The difference between age 20 and age 30 is exactly the same as the difference between age 60 and age 70. The mean age is a meaningful statistic.
- **Deaths** — the count of deaths recorded at a particular address or grid square.
- **Distance_Metres** — the distance from a household to the nearest pump.

Within the continuous family, there is again a crucial distinction.

---

### 1.5 Interval Data

**Interval** data is continuous data measured on a scale where the *distances between values are equal and meaningful*, but where **there is no true zero point**. Zero does not mean the complete absence of the thing being measured — it is simply an arbitrary reference point on the scale.

| Property | Interval data |
|---|---|
| Ordered categories | ✓ Yes |
| Equal distances between values | ✓ Yes |
| True zero (zero = complete absence) | ✗ No |
| Ratios are meaningful | ✗ No |

**The clearest example:** Temperature in degrees Celsius.

0 °C does not mean *no temperature*. It is the freezing point of water — an arbitrary reference. This means that whilst you can say 30 °C is 10 degrees warmer than 20 °C, you cannot say 30 °C is *twice as hot* as 15 °C. The ratio is meaningless because the zero is not a genuine absence.

In health sciences, interval data appears in calendar years (the year 0 CE is not the beginning of time), standardised test scores, and some psychological scales.

---

### 1.6 Ratio Data

**Ratio** data is continuous data measured on a scale where equal distances exist *and* there is a **true zero point** — a zero that genuinely means the complete absence of the quantity being measured.

| Property | Ratio data |
|---|---|
| Ordered categories | ✓ Yes |
| Equal distances between values | ✓ Yes |
| True zero (zero = complete absence) | ✓ Yes |
| Ratios are meaningful | ✓ Yes |

**Examples:**
- **Age in years:** 0 years means the person has not yet lived a single year. A person aged 40 has lived twice as long as a person aged 20. Both the difference and the ratio are meaningful.
- **Weight in kilograms:** 0 kg means the object has no mass. 80 kg is twice the mass of 40 kg.
- **Distance in metres:** 0 m means no distance. 200 m is twice as far as 100 m.
- **Death count:** 0 deaths means no one died. 10 deaths is twice as many as 5 deaths.

> **Why does this matter clinically?** A patient's blood pressure of 0 mmHg means no blood pressure is measurable — cardiac arrest. A serum creatinine of 0 means no creatinine is detectable. These are ratio variables, and the zero is medically meaningful. Treating them as interval data would distort clinical interpretation.

---

## Section 2: The Four Levels of Measurement — Quick Reference

The following table provides a single reference for all four levels. When you encounter a new variable, work through the rows from the top: if the answer is "no" at any row, you have found the correct level.

| Level | Order? | Equal distances? | True zero? | Example |
|---|---|---|---|---|
| **Nominal** | No | No | No | Blood type, Pump_Used, Gender |
| **Ordinal** | Yes | No | No | Pain scale, Glasgow Coma Scale |
| **Interval** | Yes | Yes | No | Temperature (°C), Calendar year |
| **Ratio** | Yes | Yes | Yes | Age, Weight, Height, Death count |

> **The diagnostic shortcut:** If you can multiply or divide two values and the result makes clinical sense (e.g. "this patient is twice as heavy"), the variable is ratio. If you can only add or subtract meaningfully, it is interval. If the numbers are ranks with unequal gaps, it is ordinal. If the values are simply names, it is nominal.

---

## Section 3: Why Getting This Wrong Causes Real Harm

The following scenarios illustrate what happens when a researcher assigns the wrong level of measurement to a variable.

**Scenario A — Treating nominal data as ratio:**
A researcher codes ethnicity as White = 1, Black = 2, Asian = 3, Other = 4. She calculates the mean ethnicity of the study sample and reports it as 2.3. This number is completely meaningless — there is no such thing as average ethnicity on a numerical scale — but no software will stop her from computing it. The result will be reported, cited, and built upon, producing a chain of meaningless findings.

**Scenario B — Treating ordinal data as interval:**
A clinical trial measures patient satisfaction on a five-point Likert scale: Strongly Disagree = 1, Disagree = 2, Neutral = 3, Agree = 4, Strongly Agree = 5. The researcher assumes the gaps between points are equal and calculates a mean of 3.7. This assumption is almost certainly wrong — the psychological distance between "Neutral" and "Agree" may be very different from the distance between "Disagree" and "Neutral". The mean is a distortion.

**Scenario C — The Snow dataset:**
Suppose a researcher replaces the text label `Broad St` with the number 1 and `Rupert St` with the number 2 and then calculates the average pump number across all victims. The result (say, 1.6) has no meaning whatsoever. Yet this operation is arithmetically possible and the software will execute it without complaint.

The level of measurement is therefore not an administrative label. It determines which mathematical operations are permissible and which will produce nonsense.

---

## Section 4: Applying This to the Snow Dataset

The 1854 cholera dataset we shall use throughout this course contains the following variables. Take a moment to classify each one before reading the answer.

| Variable | Description | Level of Measurement | Reasoning |
|---|---|---|---|
| `case_id` | Unique identifier for each recorded death | Nominal | An ID number is a label, not a quantity. Case 47 is not "more" than case 12. |
| `x` | Grid east–west coordinate of the death location | Interval | Grid coordinates have equal spacing but no true zero; a coordinate of 0 does not mean "no location". |
| `y` | Grid north–south coordinate | Interval | Same reasoning as `x`. |
| `Pump_Used` | Name of the pump nearest to or used by the household | Nominal | Pump names are unordered categories. |
| `Age` | Age of the victim in years | Ratio | 0 years = no years lived. Age 40 is twice as long a life as age 20. |
| `Deaths` | Number of deaths at this address | Ratio | 0 deaths = no deaths. 10 deaths is twice as many as 5 deaths. |
| `Distance_Metres` | Distance from the household to the nearest pump | Ratio | 0 m = no distance. 200 m is twice as far as 100 m. |

---

## 🔬 Lab Manual — Chapter 1

### Objective
Set up the cholera outbreak dataset in PSPP by assigning the correct variable type and measurement level for each column. Load the same dataset in R and perform a first inspection using `head()` and `summary()`.

---

### Option A — PSPP

PSPP stores variable metadata in a dedicated **Variable View** tab. Every column in your dataset has properties — name, type, and measurement level — that you must set before any analysis.

**Step-by-step instructions:**

1. Open PSPP. Go to **File → New → Data**.
2. Click the **Variable View** tab at the bottom of the screen (you will see it next to Data View).
3. For each variable, click on the row and set the following properties in the corresponding columns:

| Variable name | Type | Measure |
|---|---|---|
| `case_id` | Numeric | Nominal |
| `x` | Numeric | Scale |
| `y` | Numeric | Scale |
| `Pump_Used` | String | Nominal |
| `Age` | Numeric | Scale |
| `Deaths` | Numeric | Scale |
| `Distance_Metres` | Numeric | Scale |

> **Note on PSPP terminology:** PSPP uses the label **Scale** to refer to both interval and ratio variables — any continuous numerical variable where arithmetic operations are valid. You will use **Nominal** for unordered categories, **Ordinal** for ranked categories, and **Scale** for all interval/ratio measurements.

4. Once all variables are defined, click the **Data View** tab and enter the first five rows of the dataset manually to verify that the columns accept the correct data types.
5. Save the file as `snow_1854_outbreak.sav`.

---

### Option B — R / RStudio

R stores the cholera data in the `HistData` package. The following code loads the data, inspects its structure, and begins classification.

```r
# ---------------------------------------------------------
# Chapter 1 Lab: Loading and Inspecting the Snow Dataset
# Course: HS 4510 Biostatistics
# Dataset: Snow.deaths from the HistData package
# ---------------------------------------------------------

# Step 1: Install the package if you have not done so already.
# Run this line only once — remove the # before install.packages.
# install.packages("HistData")

# Step 2: Load the package into the current session.
library(HistData)

# Step 3: Load the Snow.deaths dataset and assign it a working name.
cholera_data <- Snow.deaths

# Step 4: Inspect the first six rows.
# This tells you what the data looks like and confirms it has loaded correctly.
head(cholera_data)

# Step 5: Get a structural overview of every variable.
# str() shows the data type R has assigned to each column.
str(cholera_data)

# Step 6: Get summary statistics for every variable.
# For numeric columns, summary() returns min, max, mean, and quartiles.
# For character columns, it returns a count of observations.
summary(cholera_data)

# Step 7: Check how many rows (observations) and columns (variables) exist.
nrow(cholera_data)
ncol(cholera_data)
```

**What to look for in the output:**

- `head()` should show columns `x`, `y`, and `deaths`. These are the three core variables in `Snow.deaths`.
- `str()` will show `num` (numeric) or `int` (integer) for each column. Confirm that no column has been read as a factor or character when it should be numeric.
- `summary()` for the `deaths` column will show the minimum, maximum, and mean death count per address. Ask yourself: is `deaths` a ratio variable? What does a value of 0 mean?

> **Extending the dataset for later chapters:** In subsequent labs, we shall add columns including `Age`, `Pump_Used`, and `Distance_Metres` to build a richer analytical dataset. The variable types you assign now establish the correct measurement level for every analysis that follows.

---

---

### 🧪 Test Your Knowledge

The Snow dataset contains a variable `Pump_Used` stored as integers (1 = Broad St, 2 = Rupert St, 3 = Marlborough Mews). A classmate calculates the mean of this column and gets 1.6. **Task:** (a) State the level of measurement that classmate wrongly assumed. (b) State the correct level of measurement and explain why. (c) Write the R code to convert `Pump_Used` to a correctly typed factor.

```{dropdown} Show Solution
```r
# (a) The classmate assumed interval or ratio measurement — treating the
#     integer codes as if the gaps between pump numbers were meaningful.
# (b) Correct level: Nominal. Pump names are unordered categories; 
#     coding them 1/2/3 is a storage convention, not a measurement scale.
# (c) R code to convert:
cholera_data$Pump_Used <- as.factor(cholera_data$Pump_Used)
levels(cholera_data$Pump_Used) <- c("Broad St", "Rupert St", "Marlborough Mews")
str(cholera_data$Pump_Used)   # Should now show Factor w/ 3 levels
```
```

## Key Terms

| Term | Definition |
|---|---|
| **Categorical data** | Data that places observations into named groups or categories rather than measuring a quantity on a numerical scale. |
| **Continuous data** | Data that measures a genuine numerical quantity, where the distances between values are real and arithmetic operations produce meaningful results. |
| **Nominal** | The lowest level of measurement. Categories exist but have no natural order or rank. Examples: blood type, pump name, diagnosis code. |
| **Ordinal** | Categories exist and have a natural rank order, but the distances between ranks are not equal or measurable. Examples: pain scale, disease severity rating. |
| **Interval** | A numerical scale with equal distances between values but no true zero. Differences are meaningful; ratios are not. Example: temperature in °C. |
| **Ratio** | A numerical scale with equal distances between values *and* a true zero representing complete absence. Both differences and ratios are meaningful. Examples: age, weight, death count. |
| **Level of measurement** | The formal classification of a variable (nominal, ordinal, interval, or ratio) that determines which mathematical operations may legitimately be applied to it. |
| **True zero** | A zero value that represents the complete absence of the quantity being measured, not an arbitrary reference point. |

---

## Review Questions

1. Define the four levels of measurement. For each level, state one clinical example that is *different* from those used in this chapter.

2. A researcher collects data on patient recovery using the following scale: 1 = Very Poor, 2 = Poor, 3 = Fair, 4 = Good, 5 = Excellent. She then calculates the mean recovery score across 200 patients and reports it as 3.4.
   - (a) What level of measurement has this researcher assumed for the recovery scale?
   - (b) Is this assumption valid? Justify your answer.
   - (c) What would be a more appropriate way to summarise this variable?

3. In the Snow dataset, the variable `Pump_Used` is sometimes stored as a number (1 for Broad St, 2 for Rupert St). A student calculates the mean of this column and reports an average pump number of 1.47. Explain precisely why this result is meaningless and what error in data classification produced it.

4. A hospital records each patient's systolic blood pressure in mmHg. Classify this variable at the correct level of measurement and justify every step of your reasoning using the defining questions for each level.

5. Explain, using a specific example from public health or clinical practice, how assigning the wrong level of measurement to a variable could lead to a harmful or misleading conclusion.

6. Open the `Snow.deaths` dataset in R using the code from the Lab Manual. Run `str(cholera_data)` and examine the output. Based on the structure R reports, which variables would you reclassify, and at what level of measurement? Write the specific R code you would use to convert the `Pump_Used` variable into a nominal factor.

---

```{admonition} Key Takeaways
:class: tip

- **Two data families:** Categorical (named groups) vs. Continuous (measurable quantities).
- **Four levels (lowest → highest):** Nominal → Ordinal → Interval → Ratio.
- **Each level adds one property:** Ordinal adds rank; Interval adds equal gaps; Ratio adds a true zero.
- **The diagnostic question:** *Can I calculate a meaningful average?* No → categorical. Yes → continuous.
- **Consequence of misclassification:** Calculating the mean of a nominal variable (e.g., pump name coded as 1/2/3) produces a meaningless number.
```

## Chapter Summary

The central lesson of this chapter is one that every health science student must internalise before they touch statistical software: **the type of data you have determines everything that follows.**

Nominal data names groups. Ordinal data ranks them. Interval data measures gaps on a scale without a true floor. Ratio data measures gaps on a scale that has a genuine zero. Each successive level adds one property: first order, then equal distances, then a true zero.

In Dr Snow's 1854 dataset, a street name (`Pump_Used`) is nominal, the grid coordinates are interval, and the death count is ratio. Confusing these is not a minor technical slip. It is the difference between a meaningful analysis and an invented one.

In the next chapter, we shall use the ratio variables in this dataset — particularly `Age` and `Deaths` — to calculate the measures of central tendency and spread that describe the shape of the outbreak.

---

*Next: **Chapter 2 — The Middle and the Mess:** Measures of Central Tendency and Spread*

---

> Part I — Describing the World in Numbers
