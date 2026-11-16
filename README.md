⚽ Football Match Outcome Prediction Using Machine Learning
   
IT3091 Machine Learning | Sports Analytics (Football) | Group 2026-DS-49

A machine learning project that analyzes historical European football matches and predicts whether the home team or away team will win, using pre-match information, recent team form, bookmaker odds, and squad ratings.
🎯 Project Overview & Prediction Objective
The goal is to support football managers, coaches, and sports analysts with data-driven insights for tactical planning, opponent analysis, and team-performance evaluation.

The project initially explored three outcomes (home win, draw, away win). The final task was narrowed to binary classification, excluding drawn matches:
- 1 — Home Win
- 0 — Away Win
Note: This model does not predict draws; its reported performance applies to the filtered binary-outcome dataset.
📊 Dataset & Exploratory Data Analysis
Source: European Soccer Database — Kaggle

The original SQLite database contains football match information along with league, team, player, and player-attribute records.
Dataset detail	Value
Matches in source database	25,979
Matches in initial SQL extraction	19,383
Final processed matches	16,375
Initially extracted features	124
Selected model features	16
Training matches	13,100
Testing matches	3,275


Exploratory analysis included:
- Checking missing values, duplicate records, match dates, and invalid odds.
- Studying home wins, draws, and away wins across seasons and leagues.
- Visualizing goal distributions and unusual scores with plots and boxplots.
- Comparing betting odds and match outcomes through bivariate analysis and correlation plots.
- Inspecting class balance after removing draws.
In the full match-outcome analysis, home wins accounted for 45.87%, draws 25.39%, and away wins 28.74%. In the final binary-outcome dataset, home wins accounted for 61.48% and away wins 38.52%.
🧹 Data Cleaning & Preprocessing
The preparation workflow included:
1. Extracting and joining relevant records from the SQLite database.
2. Checking missing values, duplicate rows, and implausible entries.
3. Handling missing Bet365 odds and incomplete match information.
4. Excluding draws to define the binary prediction target.
5. Encoding categorical identifiers such as league and team names.
6. Sorting records chronologically and splitting 80% for training / 20% for testing, without shuffling.
7. Fitting numerical scaling on the training data to reduce leakage into the test set.
Extreme but valid football scores were retained rather than automatically discarded as errors.
🛠️ Feature Engineering & Data Leakage Prevention
The final features combined three main sources of pre-match information:
Feature group	Examples	Purpose
Recent team form	home_form_scored, away_form_conceded, home_form_goal_diff, away_form_goal_diff	Capture goals scored/conceded and momentum over the previous five matches
Bookmaker odds	B365H, B365D, B365A	Capture pre-match odds for possible outcomes
Squad strength	home_squad_rating, away_squad_rating, squad_rating_diff, squad_rating_ratio	Compare historical player-rating information between the two starting squads


Leakage prevention was a major design consideration:
- Removed post-match information, such as shots, corners, cards, fouls, and possession, from prediction inputs.
- Calculated rolling form using prior matches only (shifting the historical series before computing rolling averages).
- Used a chronological train/test split rather than randomly mixing past and future matches.
- Fit scaling on the training subset rather than the entire dataset.
🤖 Machine Learning Models
Three algorithms were compared and tuned:
1. Logistic Regression
A linear classification baseline for estimating the probability of a home or away win.
2. Random Forest
An ensemble of decision trees designed to learn nonlinear relationships among odds, form, and squad-strength features.
3. XGBoost
A gradient-boosted tree approach evaluated as another nonlinear model for binary classification.

The project used hyperparameter search and compared the models on unseen test data.
📈 Model Comparison & Evaluation
Tuned model	Training accuracy	Test accuracy	Macro F1	Away-win recall	Home-win recall
🏆 Random Forest	73.15%	71.45%	0.68	49.0%	86.0%
XGBoost	73.00%	71.18%	0.68	48.0%	86.0%
Logistic Regression	71.54%	70.06%	0.66	45.63%	86.55%


Evaluation measures: accuracy, macro-averaged F1 score, class-specific recall, and confusion matrices.
🏆 Best Model — Tuned Random Forest
The tuned Random Forest achieved the highest reported test accuracy: 71.45%, with a Macro F1 score of 0.68 on 3,275 held-out matches.

Its recall was stronger for home wins (approximately 86%) than for away wins (approximately 49%). This is an important limitation: overall accuracy alone does not mean the model performs equally well for both outcomes.

Adding historical squad-rating features helped the final models exceed 70% test accuracy in the reported experiments.
🧰 Tools & Technologies
- Python — analysis and machine learning
- Jupyter Notebook / Google Colab — experimentation and documentation
- SQLite / SQL — source database and data extraction
- Pandas & NumPy — data manipulation
- Matplotlib & Seaborn — exploratory visualizations
- scikit-learn — preprocessing, model training, tuning, and evaluation
- XGBoost — boosted-tree classification

🚀 How to Explore the Project
1. Download or clone this repository.
2. Open 2026-DS-49_Final_Notebook.ipynb in Google Colab or Jupyter Notebook.
3. Download the European Soccer Database separately, subject to its usage terms.
4. Update the notebook's database path (the original notebook uses a Google Drive path such as /content/drive/MyDrive/ML_Assignment/database.sqlite).
5. Install the required Python packages and run the notebook cells in order.
Reproducibility note: The notebook may require its original Google Colab/Drive setup and compatible package versions; the database is not included in this repository layout.

📚 References
- European Soccer Database — Hugo Mathien (Kaggle)
- 2026-DS-49_Final_Submission_Report.pdf — methodology, decision logs, evaluation, and conclusions.
⚽ Combining historical football data, thoughtful feature engineering, and machine learning to support data-driven match analysis.
