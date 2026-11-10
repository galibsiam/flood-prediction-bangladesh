# Flood prediction in Bangladesh using ensemble machine learning

This repository holds the code, data and report for our undergraduate thesis at Daffodil International University (B.Sc. in Computer Science and Engineering, January 2024).

We used 74 years of monthly weather data (1949 to 2022) from 33 Bangladesh Meteorological Department stations to predict whether a flood happened in a given month at a given station. We built four ensemble models with bagging and stacking, using combinations of Decision Tree, Random Forest, XGBoost, AdaBoost, SVM and Logistic Regression.

## Authors

- Galib Md. Bin Hasan Siam
- Rifa Akila

Supervisor: Shah Md Tanvir Siddiquee, Assistant Professor, Department of CSE, DIU
Co-supervisor: Md Atik Asif Khan Akash, Lecturer, Department of CSE, DIU

## Repository contents

```
notebook/
  Prediction_of_Flood_Using_Ensemble_Machine_Learning_Methods_in_Bangladesh.ipynb
thesis/
  Flood_Prediction_Thesis_DIU_2024.pdf
data/
  FloodPrediction_1949-2022_Final_Dataset.csv
  README.md
requirements.txt
```

## Data

Weather records for 1949 to 2013 come from the Bangladesh Meteorological Department through a public dataset used in earlier work (Gauhar et al., 2021). We added weather data for 2014 to 2022 from the same department. For flood labels, we went through annual flood reports, newspapers, research papers and NGO reports and marked the months and stations where a flood was recorded.

The final file has 24,108 rows and 19 columns. Column details are in [data/README.md](data/README.md).

## Method

1. Dropped the `Sl` and `Station_Names` columns and filled the few missing weather values with a KNN imputer (k = 2). Months with no flood record were set to 0.
2. Split the data at random into 90% training and 10% test.
3. Scaled the features with `StandardScaler`.
4. Chose six features with sequential forward selection and backward elimination, using a decision tree: `Month`, `Rainfall`, `Station_Number`, `LATITUDE`, `LONGITUDE` and `Period`.
5. Trained four ensemble models:
   - Bagging model 1: hard voting over XGBoost, AdaBoost and SVM, bagged 50 times
   - Bagging model 2: hard voting over Decision Tree, Random Forest and XGBoost, bagged 50 times
   - Stacking model 1: Decision Tree, Random Forest and XGBoost, with Logistic Regression as the final estimator
   - Stacking model 2: XGBoost, AdaBoost and SVM, with a Decision Tree as the final estimator
6. Evaluated each model with accuracy, precision, recall, F1, ROC-AUC, Matthews correlation coefficient and Brier score.

## Results

These are the numbers reported in the thesis, on the 10% test split.

| Model | Accuracy (%) | Precision | Recall | F1 | ROC-AUC | MCC | Brier |
|---|---|---|---|---|---|---|---|
| Bagging model 1 | 95.44 | 0.92 | 0.91 | 0.86 | 0.91 | 0.83 | 0.046 |
| Bagging model 2 | 97.22 | 0.92 | 0.91 | 0.92 | 0.95 | 0.90 | 0.028 |
| Stacking model 1 | 96.89 | 0.92 | 0.89 | 0.91 | 0.94 | 0.89 | 0.031 |
| Stacking model 2 | 94.90 | 0.84 | 0.86 | 0.85 | 0.91 | 0.82 | 0.051 |

Bagging model 2 gave the best result.

## Known issues

We found a few problems after submitting the thesis. The code is left as it was submitted, so keep these in mind when reading the results:

- The random split puts rows from the same month at different stations into both training and test data. Because `Period` and `Station_Number` are used as features, this probably makes the scores look better than they would on future years.
- The test set is scaled with its own `fit_transform` instead of the scaler fitted on training data.
- The backward elimination result is never used; the cell copies the forward selection result instead.
- ROC-AUC and Brier score are computed from predicted labels, not probabilities.
- The precision and recall printed for Bagging model 1 come from variables that are not defined in the notebook. Its confusion matrix gives precision 0.87 and recall 0.85.
- The thesis table lists rainfall in centimetres. The values are in millimetres.

## Running the notebook

The notebook was written for Google Colab and reads the CSV from Google Drive. To run it:

1. Open the notebook in Colab.
2. Upload `data/FloodPrediction_1949-2022_Final_Dataset.csv` to your Drive, or change the path in the `pd.read_csv` cell to wherever the file is.
3. Run all cells.

It uses the `base_estimator` argument of `BaggingClassifier`, which scikit-learn removed in version 1.4, so install the versions in `requirements.txt`:

```
pip install -r requirements.txt
```

## Citation

Siam, G. M. B. H., and Akila, R. (2024). Prediction of Flood Using Ensemble Machine Learning Methods in Bangladesh. B.Sc. thesis, Department of Computer Science and Engineering, Daffodil International University, Dhaka.
