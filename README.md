# NFL Game Outcome Prediction

## Project Overview

This project builds machine learning models to predict **NFL game outcomes and point differentials** using team performance statistics derived from play-by-play data.

The goal is to determine whether **recent team performance (last 10 games)** can be used to accurately predict:

* Game winners (classification)
* Margin of victory (regression)

---

## Data Source

* Data collected using the nfl_data_py library
* Includes play-by-play data across multiple NFL seasons
* Aggregated into **team-level game statistics**

---

## Feature Engineering

### Step 1: Play-by-Play → Team-Game Data

Each game is aggregated into:

* Offensive efficiency (EPA, success rate)
* Defensive efficiency (EPA allowed)
* Turnovers and takeaways
* Points scored and allowed

---

### Step 2: Rolling Averages

For each team, rolling averages over the **last 10 games** were computed:

* `off_epa_per_play_r10`
* `def_epa_allowed_per_play_r10`
* `point_diff_r10`
* `win_r10`

👉 Uses `.shift(1)` to ensure only **past games** are included (no data leakage)

---

### Step 3: Matchup Features

Game-level features are constructed by comparing teams:

```python
diff_feature = home_stat - away_stat
```

Examples:

* `diff_off_epa_per_play_r10`
* `diff_point_diff_r10`
* `diff_def_takeaways_r10`

👉 This directly encodes **relative team strength**

---

## Models

### Classification Models

* Logistic Regression
* Random Forest

**Target:**

```python
home_win = 1 (home team wins), 0 otherwise
```

---

### Regression Model

* Predicts **point differential**
* Output: expected margin of victory

---

## Evaluation Metrics

* Accuracy
* ROC-AUC (probability ranking performance)

---

## Results

* Logistic Regression performed **as well as or better than Random Forest**
* Indicates that the engineered features are largely **linear and well-structured**
* Random Forest showed limited improvement and potential overfitting

---

## Visualizations

### Prediction Trends

* Plotted win probability by week for individual teams (e.g., Detroit Lions)
* Shows how model confidence evolves over time

---

### Game-Level Results Table

Built using `great_tables`, including:

* Matchups and scores
* Predicted vs actual winners
* Predicted vs actual point differential
* Win probability
* Correct/incorrect indicator
* Team logos and color styling

---

### Model Comparison Table

* Logistic Regression vs Random Forest
* Accuracy and ROC-AUC comparison

---

## Key Insights

* Rolling performance metrics are effective for predicting NFL games
* **EPA and success rate** are among the most informative features
* Feature engineering (especially **difference features**) is critical
* Simpler models can outperform complex ones when features are well-designed
* The model tends to **overestimate strong teams**

  * Example: Detroit predicted 14 wins vs 9 actual

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/felipeaguilarch/CSE482.git
cd CSE482
```

### 2. Start environment (Docker)

To run docker on your computer use:
docker-compose up --build

Then find (http://127.0.0.1:8888/tree?token=ada19c9b09a91cc58001758877292c41e48a7b61262b0880) put this in your browswer

Anything created in Jupyter notebook will automatically get created in your local folder. When you are done detach from the docker and then push.

Structure:
    data: store databases
    notebooks: Jupyter notebooks
    src: python scripts

```bash
docker compose up
```

### 3. Open JupyterLab

Navigate to:

```
http://localhost:8888
```

### 4. Run notebook

Open and run:

```
notebooks/your_notebook.ipynb
```

---

## Project Structure

```
├── notebooks/
│   └── nfl_prediction.ipynb
├── outputs/
│   └── lions_table.html
├── README.md
```

---

## Key Takeaway

> Good feature engineering + simple models = strong performance

This project demonstrates that **well-designed features can outperform complex models**, especially in structured datasets like sports analytics.

---

## Future Improvements

* Incorporate player-level data (QB stats, injuries)
* Add betting lines / spreads
* Tune classification threshold beyond 0.5
* Expand to playoff predictions

---

## Author

Jake Roll (Modeling & Visualizations), Josh Lamb (Report Management), Felipe Aguilar (Repository Management), Vibu Darshan
Michigan State University
Sports Analytics & Machine Learning
