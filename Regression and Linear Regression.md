
# Regression
Predicting a number based on other information. e.g. predicting house price (a number) based on size, location, etc..

# Linear Regression
Simplest type of prediction model, think of it as finding the 'line of best fit' through your data points, it assumes there's a straight-line relationship  between your input and output

## Linear Regression Example
IN this example, we will use linear regression to predict a quantitative measure of diabetes diseases progression based on age, sex, BMI, blood pressure, and six blood serum measurements s1 to s6 in sklearn's diabetes dataset.

Importing the following:
```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import sklearn.datasets import load_diabetes, load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neural_network import MLPClassifier
from sklearn.metrics import mean_squared_error, mean_absolute_error, mean_absolute_percentage_err, accuracy_score, recall_score, precision_score, f1_score, confusion_matrix
from sklearn import linear_model
```

```python
diabetes = load_diabetes()
print(f"Number of data samples: {diabetes.target.shape[0]}")
print(f"Number of potential feature variables: {diabetes.data.shape[1]}")
print(f"Feature variable numbers: {diabetes.feature_names}")
```
```md
Number of data samples: 442
Number of potential feature variables: 10
Feature variable names: ['age', 'sex', '']
```