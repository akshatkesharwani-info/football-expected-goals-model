# Football Expected Goals (xG) Model

My own xG model: the chance that a shot becomes a goal, from where and how the shot was taken, compared with StatsBomb's own xG.

## STAR
- **Situation:** Goals are rare, so shot quality is a better way to judge attacking play than the scoreline. xG models turn each shot into a probability.
- **Task:** Build an xG model from open event data and measure how close it gets to a professional model.
- **Action:** Loaded StatsBomb open data for the 2018 World Cup (first 40 of 64 matches, 1,050 shots, 1,011 after removing penalties, 82 goals). Created distance to goal, goal opening angle, header, free kick, corner, first time and under pressure features. Split by match (760 train and 251 test shots, 25 test goals) so shots from one match are never in both. Compared a constant goal rate, Logistic Regression and Gradient Boosting, then checked calibration and compared with StatsBomb xG.
- **Result:** Logistic Regression reached **AUC 0.770** and **log loss 0.278** against 0.328 for the constant baseline. StatsBomb's own xG scored AUC 0.821 on the same shots.

## Results (test set, 251 shots)
| Model | AUC | Log loss | Brier |
|---|---|---|---|
| Same chance for every shot | - | 0.3282 | 0.0903 |
| Logistic Regression | **0.7704** | **0.2778** | 0.0785 |
| Gradient Boosting | 0.7062 | 0.2959 | 0.0796 |
| StatsBomb xG | 0.8207 | 0.2514 | 0.0693 |

## Key insights
- Goal rate falls fast with distance (StatsBomb yards): 41.2% within 6, 17.9% for 6 to 10, 8.3% for 10 to 14, 4.9% for 18 to 25 and 2.4% beyond 25.
- Logistic Regression coefficients (scaled features): distance -0.868, angle +0.318, header -0.402, under pressure -0.101. Distance matters most, headers are worse than foot shots from the same spot.
- The simple Logistic Regression beat Gradient Boosting, because 760 training shots are too few for a more flexible model.
- Calibration is good for low xG shots (0.03 predicted vs 0.032 actual), but the model is too cautious on good chances: for 13 shots it predicted 0.27 and 54% were goals. Total xG was 18.2 against 25 real goals.

## Limitations
- Only 25 goals in the test set, so every number here has a wide margin of error.
- No goalkeeper or defender positions, which is why StatsBomb's model is better.
- Only 40 World Cup matches, so it may not carry over to league football.

## How to run
1. Open `football_expected_goals_model.ipynb` in Google Colab.
2. Runtime > Run all (it downloads about 40 match files from the StatsBomb open data GitHub, this can take a few minutes).
3. The first data cell should print "real data loaded".

## Files
- `football_expected_goals_model.ipynb` - executed notebook with outputs
- `requirements.txt`

## Author
Akshat Kesharwani
- Portfolio: https://akshatkesharwani-info.github.io/
- GitHub: https://github.com/akshatkesharwani-info
