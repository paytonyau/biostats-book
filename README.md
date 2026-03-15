# A Little Bit of Everything in Biostatistics for Health Science Students

**An open, applied biostatistics textbook for health science undergraduates, epidemiology students, and self-directed learners in public health.**

Written by **Payton Yau**, Lecturer in Computational Biology, Nottingham Trent University and **Suhirthakumar Puvanendran**, Lecturer in Bioinformatics, Birkbeck, University of London.

---

## What This Book Covers

This textbook teaches biostatistics through one real historical dataset — Framingham Heart Study's record — from the first chapter to the last. You never have to re-learn a new dataset to learn a new method.

| Part | Chapters | Topics |
|---|---|---|
| Part I — The Anatomy of Population Data | 1–3 | Data types, descriptive statistics, confidence intervals |
| Part II — Making Decisions Under Uncertainty | 4–6 | Probability, hypothesis testing, t-tests |
| Part III — Complex Connections | 7–8 | ANOVA, proportions, chi-square, correlation, regression |
| Part IV — Consolidation | 9 | Formula reference, test selection guide, PSPP and R cheat sheets |

Every chapter includes:
- Step-by-step **Lab Manual** instructions for both **PSPP** (free, point-and-click) and **R / RStudio**
- **Test Your Knowledge** exercises with hidden solutions
- **Key Takeaways** summary boxes
- A **Glossary** and **Data Dictionary** in the appendices

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

> 🌐 **Live book:** `https://yourusername.github.io/biostats-book`
> *(Update this URL once deployed — see Publishing section below)*

---

## Build Locally

**Requirements:** Python 3.8 or later.

```bash
# 1. Install Jupyter Book
pip install jupyter-book

# 2. Clone this repository
git clone https://github.com/yourusername/biostats-book.git
cd biostats-book

# 3. Build the HTML book
jupyter-book build biostats-book/

# 4. Open in your browser
open biostats-book/_build/html/index.html
```

---

## Publish to GitHub Pages

```bash
pip install ghp-import
ghp-import -n -p -f biostats-book/_build/html
```

Your book will be live at `https://yourusername.github.io/biostats-book`.
Update `repository_url` in `_config.yml` to match your actual GitHub URL.

---

## Repository Structure

```
biostats-book/
├── _config.yml              # Book settings
├── _toc.yml                 # Navigation structure
├── index.md                 # Landing page
├── frontmatter/             # Foreword, Preface, About the Author, etc.
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
│   ├── data_dictionary.md   # Codebook for framingham_teaching.csv dataset
│   └── glossary.md          # All key terms, A–Z
└── backmatter/
    ├── references.md
    └── contribute.md
```

---

## Licence

This textbook is released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) licence. You are free to share and adapt the material for any purpose, provided appropriate credit is given.

**Citation:**
> Yau, P. T. O. (*year*). *A Little Bit of Everything in Biostatistics for Health Science Students*. GitHub. `https://github.com/yourusername/biostats-book`

---

## Contributing

Found a typo, broken code, or a concept that could be explained better? See [How to Contribute](biostats-book/backmatter/contribute.md) or open a GitHub Issue.
