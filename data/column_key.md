# Column Key — Diabetes Health Indicators Dataset

Source: CDC Behavioral Risk Factor Surveillance System (BRFSS) 2015
Rows: 251,467 | Features: 22 + 1 target (includes Race; 2,213 rows removed for Race = don't know/refused)

---

## Target Variable

| Column | Description |
|--------|-------------|
| `Diabetes_binary` | 0 = No diabetes · 1 = Prediabetes · 2 = Diabetes |

---

## Health Conditions (10 features)

| Column | Description |
|--------|-------------|
| `HighBP` | High blood pressure — 0 = No · 1 = Yes |
| `HighChol` | High cholesterol — 0 = No · 1 = Yes |
| `CholCheck` | Cholesterol check in past 5 years — 0 = No · 1 = Yes |
| `BMI` | Body Mass Index (continuous) |
| `Stroke` | Ever told you had a stroke — 0 = No · 1 = Yes |
| `HeartDiseaseorAttack` | Coronary heart disease (CHD) or myocardial infarction (MI) — 0 = No · 1 = Yes |
| `GenHlth` | General health self-rating (scale 1–5) — 1 = Excellent · 2 = Very good · 3 = Good · 4 = Fair · 5 = Poor |
| `MentHlth` | Days of poor mental health in past 30 days (scale 0–30) |
| `PhysHlth` | Physical illness or injury days in past 30 days (scale 0–30) |
| `DiffWalk` | Serious difficulty walking or climbing stairs — 0 = No · 1 = Yes |

---

## Lifestyle Behaviors (5 features)

| Column | Description |
|--------|-------------|
| `Smoker` | Smoked at least 100 cigarettes in lifetime (5 packs = 100 cigarettes) — 0 = No · 1 = Yes |
| `PhysActivity` | Physical activity in past 30 days (not including job) — 0 = No · 1 = Yes |
| `Fruits` | Consume fruit 1 or more times per day — 0 = No · 1 = Yes |
| `Veggies` | Consume vegetables 1 or more times per day — 0 = No · 1 = Yes |
| `HvyAlcoholConsump` | Heavy alcohol use (adult men ≥14 drinks/week, adult women ≥7 drinks/week) — 0 = No · 1 = Yes |

---

## Demographics (4 features)

| Column | Description |
|--------|-------------|
| `Sex` | 0 = Female · 1 = Male |
| `Race` | Computed race-ethnicity (`_RACE`) — 1 = White, non-Hispanic · 2 = Black, non-Hispanic · 3 = American Indian/Alaskan Native, non-Hispanic · 4 = Asian, non-Hispanic · 5 = Native Hawaiian/Pacific Islander, non-Hispanic · 6 = Other race, non-Hispanic · 7 = Multiracial, non-Hispanic · 8 = Hispanic |
| `Age` | 13-level age category (`_AGEG5YR`) — 1 = 18–24 · 2 = 25–29 · 3 = 30–34 · 4 = 35–39 · 5 = 40–44 · 6 = 45–49 · 7 = 50–54 · 8 = 55–59 · 9 = 60–64 · 10 = 65–69 · 11 = 70–74 · 12 = 75–79 · 13 = 80 or older |
| `Education` | Education level (`EDUCA`) — scale 1–6 · 1 = Never attended school or only kindergarten · 2 = Grades 1–8 (Elementary) · 3 = Grades 9–11 (Some high school) · 4 = Grade 12 or GED (High school graduate) · 5 = College 1–3 years (Some college or technical school) · 6 = College 4+ years (College graduate) |
| `Income` | Annual household income (`INCOME2`) — scale 1–8 · 1 = Less than $10,000 · 2 = $10,000 to less than $15,000 · 3 = $15,000 to less than $20,000 · 4 = $20,000 to less than $25,000 · 5 = $25,000 to less than $35,000 · 6 = $35,000 to less than $50,000 · 7 = $50,000 to less than $75,000 · 8 = $75,000 or more |

---

## Healthcare Access (2 features)

| Column | Description |
|--------|-------------|
| `AnyHealthcare` | Has any health care coverage (insurance, HMO, etc.) — 0 = No · 1 = Yes |
| `NoDocbcCost` | Could not see a doctor in the past 12 months due to cost — 0 = No · 1 = Yes |

---

## Feature Group Summary

| Group | Features |
|-------|----------|
| Health Conditions | HighBP, HighChol, CholCheck, BMI, Stroke, HeartDiseaseorAttack, GenHlth, MentHlth, PhysHlth, DiffWalk |
| Lifestyle Behaviors | Smoker, PhysActivity, Fruits, Veggies, HvyAlcoholConsump |
| Demographics | Sex, Race, Age, Education, Income |
| Healthcare Access | AnyHealthcare, NoDocbcCost |
