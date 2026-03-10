# Preface: Why Numbers Need Health Professionals

If you are reading this, you are training to become a nurse, a public health officer, an epidemiologist, a biomedical scientist, or a health services administrator. You entered this field to save lives, treat illness, and improve the health of communities. You almost certainly did not enter this field because you love calculus.

Yet Biostatistics is a required component of nearly every health science curriculum. And too often, it is taught as though the audience is a room full of mathematics students — drowning clinicians and public health practitioners in Greek symbols, abstract probability puzzles, and formulas derived from coin flips and card decks that bear no visible relationship to the work of caring for patients or managing disease outbreaks.

This book exists to close that gap.

---

## The Purpose of This Book

The purpose of this guide is not to turn you into a statistician. The purpose is to turn you into an **evidence-based practitioner** — someone who can read a research paper critically, design a small-scale health study rigorously, run a dataset through software correctly, and translate the numerical output into a decision that affects real people.

Throughout these chapters, we do not work with abstract textbook problems. Instead, we work as an epidemiological investigation team examining one of the most consequential public health datasets in history: **Dr John Snow's record of the 1854 Broad Street cholera outbreak in London**. Every concept — from data types and descriptive statistics in Chapter 1, to confidence intervals in Chapter 3, to regression and prediction in Chapter 8 — is taught through this single, historically documented case. You never have to re-learn a new dataset to learn a new formula.

The questions we answer are real questions:

- Is the cluster of deaths near the Broad Street pump statistically significant, or could it be chance?
- How confident can we be in our estimate of the mean age of victims?
- Does distance from the pump predict household mortality, and by how much?
- Were the brewery workers who drank beer instead of water genuinely protected, or is that apparent effect a statistical artefact?

These are the kinds of questions that determine whether a pump handle gets removed, whether a hospital ward gets quarantined, or whether a vaccine programme gets funded. Biostatistics is not an academic exercise. It is a decision-making tool. This book teaches it as one.

---

## Who This Book Is For

This book was written for students enrolled in **HS 4510 Biostatistics** at the University of the People, and for any health science undergraduate who needs a practical, applied introduction to the subject. No prior statistical knowledge is assumed. Familiarity with basic algebra — the ability to substitute numbers into a formula and compute the result — is sufficient preparation for everything in these pages.

If you have previously attempted a biostatistics course and found it inaccessible, this book is especially for you. The emphasis throughout is on *meaning*: what does this number represent? What decision does this result support? What would it mean for a patient, a community, or a public health policy?

---

## What This Book Does Differently

**One dataset, nine chapters.** Most biostatistics textbooks introduce a new dataset with every new concept, forcing students to spend cognitive energy on unfamiliar data rather than unfamiliar statistics. This book uses the Snow cholera dataset throughout. By Chapter 5, you know this dataset well enough to focus entirely on what the hypothesis test is doing — not on what the variables mean.

**Software first, theory second.** Every chapter includes a parallel Lab Manual section with step-by-step instructions for both **PSPP** (a free, open-source, point-and-click alternative to SPSS) and **R / RStudio** (the industry standard for epidemiological research). You do not need to wait until you have mastered the mathematics to run your first analysis. You run it in Week 1, alongside reading about data types.

**Interpretation over computation.** Modern health practitioners do not compute t-statistics by hand. Software computes the numbers. What practitioners must do — and what too few courses teach explicitly — is read the output correctly, communicate the findings accurately, and recognise when the result is being misinterpreted. Every chapter in this book includes worked output interpretation tables, correct-versus-incorrect statement comparisons, and plain-language reporting templates.

**The human element.** This book acknowledges something most biostatistics textbooks ignore entirely: numbers are not the whole picture. Chapter 5 covers sampling design, because a perfect analysis of a badly collected sample produces a confidently wrong answer. Chapter 7 covers the comparison of proportions and relative risk, because epidemiology lives in attack rates and risk differences, not only in means. And the course review in Chapter 9 includes an ethics section, because every row in a health dataset represents a human being, and the responsibility that carries does not appear in any formula.

---

## How to Use This Book

Each chapter corresponds to one week of the HS 4510 course. Work through the conceptual sections before opening PSPP or R. Attempt the Review Questions before checking any answers. Use the Key Terms table as a self-test before the chapter examination.

Chapter 9 is different from all the others. It contains no new theory. It is a dedicated examination preparation resource: a complete formula reference, a test-selection decision guide, and full cheat sheets for both PSPP and R. Keep it open during your revision.

---

## A Note on Language

This book uses **British English spelling** throughout — *whilst*, *recognise*, *practise* (verb), *licence* (noun), *colour* — consistent with the international conventions of the University of the People and the majority of the peer-reviewed epidemiology literature.

Statistical notation follows standard conventions: population parameters in Greek letters (μ, σ, ρ), sample statistics in Roman letters (x̄, s, r). Every formula is introduced with a plain-language description before the symbolic notation is presented.

---

When you open a dataset, remember: every row was once a person.

The numbers are tools. You are the practitioner.

---

*— Payton Tung On Yau*
