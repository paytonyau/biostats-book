# Preface

## Why We Wrote This Book

The question we hear most often from health science students encountering biostatistics for the first time is not "what does this formula mean?" It is "why does this matter?"

This book is our answer to that question.

We wrote it because the standard approach to teaching biostatistics — abstract formulas first, clinical context later, if at all — produces graduates who can pass an exam but cannot interpret a confidence interval on a drug trial report. We wrote it because health science students deserve a textbook that treats them as future clinicians and public health practitioners, not as apprentice mathematicians.

Most importantly, we wrote it because the dataset at the heart of this book earns its place there. The Framingham Heart Study did not just produce important findings — it invented the methodology of risk factor epidemiology. Every cardiovascular risk score used in clinical practice today (QRISK, Framingham Risk Score, SCORE2) descends from the work done in this study. Using Framingham data to teach biostatistics to health science students is not an arbitrary pedagogical choice. It is teaching the methods through the study that created them.

## How This Book Is Different

**One dataset, nine chapters.** Every statistical method in this book is demonstrated on the same 500 participants. Students do not need to learn a new clinical context in each chapter. They learn one context deeply — and apply progressively more powerful analytical tools to it.

**Plain English before formulas.** Every key concept is introduced with a plain-language statement before the formal definition. The formula follows the intuition, not the other way around.

**Honest about limitations.** Where a method has assumptions that matter, we say so. Where a common misinterpretation exists, we name it explicitly. Where a statistical test is technically incorrect for the data (as OLS regression is for censored survival times), we explain why and show the correct alternative.

**Epidemiology built in, not bolted on.** Incidence rates, relative risk, odds ratios, Kaplan-Meier curves, and the log-rank test are not appendices. They are woven through the chapters where they belong, taught using the variables in the dataset that require them.

## A Note on the Teaching Dataset

The Framingham teaching subset used in this book (n = 500) is derived from data provided by the NHLBI for educational use. It has been processed to ensure participant anonymity. It is appropriate for teaching and illustrative analysis only — it should not be used for research publication or clinical decision-making.

The dataset is available as `framingham_teaching.csv` in the book's data folder and can be loaded in R with a single line of code.

## For Instructors

Each chapter is self-contained and can be taught independently, though the book is designed to be read sequentially. Chapters 1–3 assume no prior statistical knowledge. Chapters 4–6 build on descriptive statistics and introduce inferential reasoning. Chapters 7–8 extend to multiple groups and relationships. Chapter 9 consolidates all methods and introduces survival analysis.

Lab exercises are provided in both PSPP (free, open-source) and R. Either tool is sufficient for all labs in this book.

*Payton Yau*
*Nottingham Trent University / University of the People*

*Suhirthakumar Puvanendran*
*Birkbeck, University of London / University of West London*
