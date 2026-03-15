# Appendix A — Data Dictionary

## Dataset: Framingham Heart Study Teaching Subset

**Source:** Derived from the Framingham Heart Study teaching dataset provided by the National Heart, Lung, and Blood Institute (NHLBI/NIH) for educational use.
**Study:** Framingham Heart Study (Contract N01-HC-25195), Framingham, Massachusetts, USA.
**Teaching subset:** n = 500 participants, baseline examination (Period 1), complete cases.
**File:** `framingham_teaching.csv` (in `data/` folder of the book repository)

> **Important:** This is a teaching dataset. Specific methods were employed to ensure participant anonymity. It is not appropriate for research publication or clinical decision-making.

**Load in R:**
```r
fram_data <- read.csv("data/framingham_teaching.csv")
```

---

## Variable Dictionary

| Variable | Type | Level | Values / Units | Description |
|---|---|---|---|---|
| `RANDID` | Integer | Nominal | 1001–1500 | Participant ID — a label, not a quantity |
| `SEX` | Integer → Factor | Nominal | 1=Male, 2=Female | Sex at examination |
| `EDUC` | Integer → Ordered Factor | **Ordinal** | 1=0-11yrs, 2=HS/GED, 3=Some college, 4=College grad | Education level — ranked, unequal gaps |
| `AGE` | Integer | Ratio | 32–70 years | Age at baseline examination |
| `CURSMOKE` | Integer → Factor | Nominal | 0=Non-smoker, 1=Current smoker | Smoking status at examination |
| `CIGPDAY` | Integer | Ratio | 0–60 cigarettes/day | Cigarettes smoked per day (0 for non-smokers) |
| `TOTCHOL` | Integer | Ratio | mg/dL | Serum total cholesterol |
| `SYSBP` | Decimal | Ratio | mmHg | Systolic blood pressure |
| `DIABP` | Decimal | Ratio | mmHg | Diastolic blood pressure |
| `BPMEDS` | Integer → Factor | Nominal | 0=No, 1=Yes | Currently taking anti-hypertensive medication |
| `DIABETES` | Integer → Factor | Nominal | 0=No, 1=Yes | Diabetes diagnosis at examination |
| `BMI` | Decimal | Ratio | kg/m² | Body mass index |
| `HEARTRTE` | Integer | Ratio | beats/min | Ventricular rate (heart rate) |
| `GLUCOSE` | Integer | Ratio | mg/dL | Casual serum glucose |
| `PREVCHD` | Integer → Factor | Nominal | 0=No, 1=Yes | Prevalent coronary heart disease at baseline |
| `PREVHYP` | Integer → Factor | Nominal | 0=No, 1=Yes | Prevalent hypertension at baseline |
| `ANYCHD` | Integer → Factor | Nominal | 0=No event, 1=CHD event | Incident CHD during follow-up (any: MI, angina, CHD death) |
| `TIMECHD` | Integer | Ratio | Days | Days from baseline to first CHD event or end of follow-up |
| `DEATH` | Integer → Factor | Nominal | 0=Survived, 1=Died | All-cause mortality during follow-up |
| `TIMEDTH` | Integer | Ratio | Days | Days from baseline to death or end of follow-up |
| `STROKE` | Integer → Factor | Nominal | 0=No, 1=Stroke | Incident stroke during follow-up |
| `TIMESTRK` | Integer | Ratio | Days | Days from baseline to stroke or end of follow-up |

---

## Notes on Variable Use

**Survival variables (`TIMEDTH + DEATH`, `TIMECHD + ANYCHD`):**
`TIMEDTH` and `TIMECHD` are **censored survival data**. Participants who did not have the event by end of follow-up have their time recorded but the true event time is unknown (censored). These variables must be analysed using Kaplan-Meier + log-rank test (Chapter 8) — *not* with OLS regression or standard descriptive statistics alone.

**`EDUC` (ordinal):**
The four education levels are ranked but have unequal gaps. Level 1 → Level 2 (completing secondary school) is not the same as Level 3 → Level 4 (completing a college degree). Always convert to an ordered factor in R (`ordered=TRUE`). Use median or mode for summaries, not mean.

**Binary variables (`CURSMOKE`, `DIABETES`, `ANYCHD`, etc.):**
Stored as 0/1 integers but are **nominal**. Convert to factors in R before analysis. The mean of a 0/1 variable gives a proportion (e.g., mean ANYCHD = 0.31 means 31% had a CHD event), but should not be interpreted as a scale mean.

---

## R Code: Complete Data Preparation

```r
# Load
fram_data <- read.csv("data/framingham_teaching.csv")

# Convert all categorical variables to labelled factors
fram_data$SEX      <- factor(fram_data$SEX,      levels=c(1,2),    labels=c("Male","Female"))
fram_data$EDUC     <- factor(fram_data$EDUC,     levels=1:4, ordered=TRUE,
  labels=c("0-11yrs","HS_GED","Some_college","College_grad"))
fram_data$CURSMOKE <- factor(fram_data$CURSMOKE, levels=c(0,1),    labels=c("Non-smoker","Smoker"))
fram_data$BPMEDS   <- factor(fram_data$BPMEDS,   levels=c(0,1),    labels=c("No","Yes"))
fram_data$DIABETES <- factor(fram_data$DIABETES, levels=c(0,1),    labels=c("No","Yes"))
fram_data$PREVCHD  <- factor(fram_data$PREVCHD,  levels=c(0,1),    labels=c("No","Yes"))
fram_data$PREVHYP  <- factor(fram_data$PREVHYP,  levels=c(0,1),    labels=c("No","Yes"))
fram_data$ANYCHD   <- factor(fram_data$ANYCHD,   levels=c(0,1),    labels=c("No_CHD","CHD_event"))
fram_data$DEATH    <- factor(fram_data$DEATH,    levels=c(0,1),    labels=c("Survived","Died"))
fram_data$STROKE   <- factor(fram_data$STROKE,   levels=c(0,1),    labels=c("No","Yes"))

# Verify
str(fram_data)
summary(fram_data)
```

---

## Dataset Use by Chapter

| Chapter | Variables used | Primary analysis |
|---|---|---|
| 1 | All 22 | Variable classification; factor conversion |
| 2 | `SYSBP`, `TOTCHOL`, `AGE`, `BMI`, `CIGPDAY` | Descriptive statistics |
| 3 | `SYSBP`, `TOTCHOL`, `AGE` | SE and 95% CI |
| 4 | `SYSBP`, `TOTCHOL`, `CIGPDAY`, `BMI`, `TIMECHD` | Normality; distributions; incidence chart |
| 5 | `TOTCHOL`, `SYSBP`, `AGE` | One-sample t-test vs reference |
| 6 | `SYSBP`, `TOTCHOL` by `CURSMOKE`; hypothetical pre/post | Independent and paired t-test |
| 7 | `TOTCHOL` by `EDUC` (ANOVA); `CURSMOKE` × `ANYCHD` (Chi-Square); RR/RD | ANOVA, Chi-Square, proportions |
| 8 | `AGE` → `SYSBP` (regression); `TIMEDTH + DEATH` by `CURSMOKE` | Correlation, regression, KM + log-rank |
| 9 | All | Review and cheat sheets |
