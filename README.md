# 🏅 Olympic Medal Predictor

Predicts how many medals a country's team will win at the Summer Olympics from two signals:
**how many athletes it sends** and **how many medals it won last time**.

## Data

`teams.csv` holds one row per country per Games, 1964 to 2016 (2,014 rows after dropping
missing values). The notebook uses these columns:

| Column | Meaning |
| --- | --- |
| `team`, `country` | Country code and name |
| `year` | Olympic year |
| `athletes` | Athletes the country sent |
| `age` | Average athlete age |
| `prev_medals` | Medals won at the previous Games |
| `medals` | Medals won (the target) |

Medals correlate strongly with team size (**r = 0.84**) and previous medals (**r = 0.92**),
and barely at all with average age (r = 0.02), so the model uses the first two.

## Model

1. Load and select columns, drop rows with missing values
2. Explore correlations and plot medals against athletes and age (seaborn)
3. Split by time so the model never sees the future: train on **1964 to 2008**, test on **2012 and 2016**
4. Fit a `LinearRegression` on `athletes` and `prev_medals`
5. Clip negative predictions to 0 and round to whole medals
6. Score with mean absolute error, then break the error down per country

## Results

| Metric | Value |
| --- | --- |
| Mean absolute error (test, 2012 and 2016) | **3.3 medals** |
| Average medals per team | 11.0 (std 33.6) |

The error is small for big, consistent teams and proportionally larger for countries that win
only a handful of medals, which the per-team error ratio at the end of the notebook shows.

## Run it

```bash
pip install pandas scikit-learn seaborn jupyter
jupyter notebook AVP.ipynb
```

## Ideas for next steps

- Add more features (events entered, host-country flag, `prev_3_medals`)
- Try tree-based models and compare MAE
- Report error separately for small and large teams
