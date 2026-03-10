# Appendix A — Data Dictionary

## The Master Dataset: Snow.deaths (1854 Broad Street Cholera Outbreak)

This appendix defines every variable used in this book. All worked examples, lab exercises, and Review Questions draw from this dataset. Before running any code in PSPP or R, read this table in full.

**Source:** `Snow.deaths` from the R package `HistData` (Friendly, 2022), based on Dr John Snow's original 1854 field records.

**Unit of observation:** One row = one household address in the Soho district of London where at least one cholera death was recorded during the August–September 1854 outbreak.

**Total rows in base dataset:** 578 address records.

---

## Core Variables (Present in `Snow.deaths`)

| Variable Name | Data Type | Level of Measurement | Definition | Valid Values | Notes |
|---|---|---|---|---|---|
| `x` | Numeric | Interval | Horizontal grid coordinate of the household address on Snow's original map | Continuous decimal | Not interpretable as a real-world distance unit; used only for spatial mapping |
| `y` | Numeric | Interval | Vertical grid coordinate of the household address on Snow's original map | Continuous decimal | Paired with `x` to reconstruct Snow's dot map |
| `deaths` | Integer | Ratio | Number of cholera deaths recorded at this address during the 1854 outbreak | 0 and above (integer) | A value of 0 indicates an address with no deaths; true zero is meaningful |

---

## Extended Variables (Added in Chapter Labs)

The following variables are added to the base dataset in lab exercises throughout the book. They are constructed from the base coordinates or from historical supplementary records.

| Variable Name | Added in Chapter | Data Type | Level of Measurement | Definition | Valid Values |
|---|---|---|---|---|---|
| `Age` | Ch. 2 | Numeric | Ratio | Age of the primary victim at the address in years at time of death | Positive real numbers; 0 = infant |
| `Pump_Used` | Ch. 1 | Character / Factor | Nominal | Name of the water pump from which the household primarily drew water | `"Broad St"`, `"Rupert St"`, `"Marlborough Mews"` |
| `Distance_Metres` | Ch. 3 | Numeric | Ratio | Estimated straight-line walking distance in metres from the household address to the Broad Street pump | Positive real numbers |
| `Household_Deaths` | Ch. 8 | Integer | Ratio | Total number of cholera deaths at this household address (same as `deaths` in the base dataset, renamed for clarity) | 0 and above (integer) |
| `Severity` | Ch. 7 | Character / Factor | Ordinal | Recorded severity classification of illness at the household | `"Mild"`, `"Moderate"`, `"Severe"` |
| `Near_Pump` | Ch. 7 | Integer (dummy) | Nominal | Binary indicator: 1 if `Distance_Metres` ≤ 100m, 0 otherwise | `0`, `1` |
| `Death_Any` | Ch. 7 | Integer (dummy) | Nominal | Binary indicator: 1 if `Household_Deaths` > 0, 0 otherwise | `0`, `1` |
| `Interview_Notes` | Ch. 9 | Character | Nominal | Text field representing notes from qualitative household interviews (simulated for the mixed-methods lab) | Free text |
| `Boils_Water_Dummy` | Ch. 9 | Integer (dummy) | Nominal | Binary indicator derived from `Interview_Notes`: 1 if household reported boiling water, 0 otherwise | `0`, `1` |
| `Neighbourhood` | Ch. 7 | Character / Factor | Nominal | Sub-district of Soho in which the address is located | `"Soho"`, `"Mayfair"`, `"Holborn"` (extended dataset only) |

---

## Loading the Dataset in R

```r
# Install the package (run once only)
# install.packages("HistData")

# Load and assign
library(HistData)
cholera_data <- Snow.deaths

# Inspect
head(cholera_data)      # First 6 rows: columns x, y, deaths
str(cholera_data)       # Variable types
summary(cholera_data)   # Min, max, mean per column
dim(cholera_data)       # 578 rows × 3 columns (base dataset)
names(cholera_data)     # Column names
```

---

## Loading the Dataset in PSPP

The extended dataset used throughout the labs is saved as `snow_1854_outbreak.sav`. To load it:

1. Open PSPP.
2. Go to **File → Open → Data**.
3. Navigate to the folder where you saved `snow_1854_outbreak.sav` and click **Open**.
4. In **Variable View**, confirm that:
   - `Age` and `Distance_Metres` are set to **Scale** (continuous)
   - `Pump_Used` and `Severity` are set to **Nominal** and **Ordinal** respectively
   - `Near_Pump`, `Death_Any`, and `Boils_Water_Dummy` are set to **Nominal**
5. In **Data View**, scroll through several rows to confirm values loaded correctly.

---

## Common Coding Errors to Avoid

| Error | Consequence | How to detect |
|---|---|---|
| `Pump_Used` stored as integers (1/2/3) without converting to a factor | Software treats pump codes as a continuous variable; calculates a meaningless "mean pump number" | Run `str(cholera_data)` and check that `Pump_Used` shows `Factor` not `int` or `num` |
| `Age` stored as a character string (e.g., `"34"` instead of `34`) | `mean()` and `sd()` return `NA`; no descriptive statistics can be computed | Run `str(cholera_data)` and check that `Age` shows `num` or `int` |
| `Severity` treated as nominal instead of ordinal | Analysis does not respect the rank order (Mild < Moderate < Severe); ordered comparisons are invalid | In PSPP Variable View, set Measure to **Ordinal**. In R: `factor(Severity, ordered=TRUE, levels=c("Mild","Moderate","Severe"))` |
| `Near_Pump` or `Death_Any` used as continuous predictors | Regression treats binary codes as if intermediate values (0.5, 0.3) were meaningful | Always convert binary dummies to `factor` before using as grouping variables in t-tests or ANOVA |

---

## A Note on Data Provenance

The `Snow.deaths` dataset in `HistData` is a reconstruction from Snow's original published map and records, compiled by Michael Friendly (2022). It accurately represents the spatial locations of cholera deaths but some derived variables — particularly `Age` and `Severity` — are added for pedagogical purposes in this textbook and do not represent Snow's original field data. They are constructed to be plausible and internally consistent with the historical record. Any analysis using these derived variables should be understood as a teaching exercise, not a historical claim.
