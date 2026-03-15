# A Little Bit of Everything in Biostatistics for Health Science Students
## By Payton Yau and Suhirthakumar Puvanendran | Version 1.0


```{figure} ../images/cover.png
:name: fig-intro-vis
:width: 80%
:align: center

```

**An Applied Open-Source Textbook for Health Science Undergraduates, Epidemiology Students, and Self-Directed Learners in Public Health**


---

## About This Book

Most biostatistics textbooks are written for mathematics students. This book is written for people who want to understand disease — and prevent it.

Every chapter is built around a single, historically influential dataset: a teaching subset of the **Framingham Heart Study**, the longest-running cardiovascular epidemiology study in history. Since 1948, researchers in Framingham, Massachusetts have followed thousands of participants to identify the risk factors for heart disease — high blood pressure, high cholesterol, smoking, diabetes, obesity — concepts so fundamental to modern medicine that we have forgotten they had to be *discovered*.

This book uses that dataset to teach statistics the way it should be taught: through real variables with real clinical meanings, where a regression slope tells you something about survival, and a Chi-Square result tells you something about who is most at risk.

---

## What You Will Learn

By the end of this book you will be able to:

- Classify any health variable at the correct level of measurement
- Calculate and interpret descriptive statistics and confidence intervals
- Select and run the correct statistical test for any research scenario
- Interpret p-values, effect sizes, and confidence intervals correctly
- Produce and read Kaplan-Meier survival curves and log-rank tests
- Use PSPP and R to analyse real epidemiological data

---

## The Dataset

**The Framingham Heart Study Teaching Subset** — 500 participants, 22 variables, baseline examination.

*Data derived from the Framingham Heart Study teaching dataset, provided by the National Heart, Lung, and Blood Institute (NHLBI/NIH) for educational purposes. Original study: Framingham Heart Study (N01-HC-25195). This teaching subset is for educational use only and is not appropriate for research publication.*

```r
# Load the dataset in R
fram_data <- read.csv("data/framingham_teaching.csv")
```

---

## How This Book Is Organised

The book is divided into four parts across nine chapters:

**Part I — Describing the World in Numbers** (Chapters 1–3): What kind of data do we have? How do we summarise it? How confident can we be in our estimates?

**Part II — Testing What We Think We Know** (Chapters 4–6): What are the laws of chance? How do we test a hypothesis? How do we compare two groups?

**Part III — Comparing More Than Two Groups** (Chapters 7–8): How do we compare three or more groups? How do we measure and model relationships?

**Part IV — Putting It All Together** (Chapter 9): How do we select the right test? How do we read and report survival data? What have we learned?

---

## How to Use This Book

Each chapter follows the same structure: a plain-English introduction, worked examples using the Framingham data, a lab manual in both PSPP and R, a Test Your Knowledge exercise with a dropdown solution, key terms, review questions, and a summary.

You do not need a mathematics background. You need curiosity about disease and the patience to work through examples.

---

*A Little Bit of Everything in Biostatistics for Health Science Students* is an open-source textbook. It is free to read, use, and adapt under its open licence. Source files are available at the GitHub repository linked above.
