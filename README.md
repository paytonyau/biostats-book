# A Little Bit of Everything in Biostatistics for Health Science Students

**An open, applied biostatistics textbook for health science undergraduates, epidemiology students, and self-directed learners in public health.**

---

## What This Book Covers

This textbook teaches biostatistics through two real, historically significant datasets—from the first chapter to the last. Rather than confusing students with hypothetical scenarios or random numbers, every statistical concept is grounded in actual public health data. We use the legendary **Framingham Heart Study** to explore observational epidemiology and cardiovascular risk, and the **Anorexia Clinical Trial** dataset to explore experimental design and psychological interventions. By returning to these same cohorts chapter after chapter, students can focus entirely on mastering the statistics rather than re-learning a new clinical scenario every week.

| Part | Chapters | Topics |
|---|---|---|
| **Part I — Describing the World in Numbers** | 1–3 | Study design, data types, descriptive statistics, confidence intervals |
| **Part II — Testing What We Think We Know** | 4–6 | Probability distributions, hypothesis testing, t-tests (independent & paired) |
| **Part III — Comparing More Than Two Groups** | 7–8 | ANOVA, proportions, chi-square, correlation, regression, survival analysis |
| **Part IV — Putting It All Together** | 9 | Course review, test selection guide, cheat sheets, epidemiological measures |

Every chapter includes:
- Step-by-step **Lab Manual** instructions for both **PSPP** (free, point-and-click) and **R / RStudio**
- **Test Your Knowledge** exercises with hidden solutions
- **Key Takeaways** summary boxes
- A **Glossary** and **Data Dictionaries** in the appendices

---

## Who This Book Is For

- Students taking a biostatistics course as part of a **Bachelor of Health Science** or **Certificate in Epidemiology**
- **Health science undergraduates** at any institution who need a practical, applied introduction to statistics
- **Practising clinicians or public health workers** who want a self-directed refresher
- Anyone who has attempted a biostatistics course and found it inaccessible

No prior statistical knowledge is assumed. Basic algebra is sufficient.

---

## Software Requirements

Both software packages are **free**:

| Software | Purpose | Download |
|---|---|---|
| **R** | Statistical computing | [r-project.org](https://www.r-project.org) |
| **RStudio** | IDE for R (recommended) | [posit.co](https://posit.co/download/rstudio-desktop/) |
| **PSPP** | Free open-source alternative to SPSS | [gnu.org/software/pspp](https://www.gnu.org/software/pspp/) |

---

## Read Online

> 🌐 **Live book:** [https://paytonyau.github.io/biostats-book](https://paytonyau.github.io/biostats-book)

---

## Repository Structure

```text
biostats-book/
├── _config.yml              # Book settings
├── _toc.yml                 # Navigation structure
├── index.md                 # Landing page
├── frontmatter/             # Preface, About the Authors, Copyright, etc.
├── chapters/                # Chapters 1–9
│   ├── 01_vitals.md
│   ├── 02_middle.md
│   ├── 03_margin.md
│   ├── 04_chance.md
│   ├── 05_hypothesis.md
│   ├── 06_twogroups.md
│   ├── 07_scaling.md
│   ├── 08_future.md
│   └── 09_review.md
├── appendices/
│   ├── data_dictionary.md   # Codebooks for Framingham and Anorexia datasets
│   └── glossary.md          # All key terms, A–Z
└── backmatter/
    ├── references.md
    └── contribute.md
```

---

## Licence

This textbook is released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) licence. You are free to share and adapt the material for any purpose, provided appropriate credit is given.

## How to Cite This Book

If you use this book in your teaching, research, or studies, please cite it using the following format:

**Plain Text (APA):**
> Yau, P., Puvanendran, S., & Nualyong, J. (2026). *A Little Bit of Everything in Biostatistics for Health Science Students*. Published online at: https://paytonyau.github.io/biostats-book

**BibTeX:**
```bibtex
@book{yau2026biostatistics,
  title     = {A Little Bit of Everything in Biostatistics for Health Science Students},
  author    = {Yau, Payton and Puvanendran, Suhirthakumar and Nualyong, Jarunee},
  year      = {2026},
  url       = {[https://paytonyau.github.io/biostats-book](https://paytonyau.github.io/biostats-book)},
  note      = {Published online}
}

---

## Contributing

Found a typo, broken code, or a concept that could be explained better? See [How to Contribute](backmatter/contribute.md) or open a GitHub Issue.