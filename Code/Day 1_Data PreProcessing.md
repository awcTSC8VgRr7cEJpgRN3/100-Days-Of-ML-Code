# Data PreProcessing
<p align="center">
  <img src="../Info-graphs/Day%201.jpg">
</p>

As shown in the infograph we will break down data preprocessing in 6 essential steps.
Get the dataset from [here](https://github.com/Avik-Jain/100-Days-Of-ML-Code/tree/master/datasets) that is used in this example

## Step 1: Importing the libraries
```Python
import numpy as np
import pandas as pd
```
## Step 2: Importing dataset
```python
dataset = pd.read_csv('Data.csv')
X = dataset.iloc[ : , :-1].values
Y = dataset.iloc[ : , 3].values
```
## Step 3: Handling the missing data [^SimpleImputer]

[^SimpleImputer]: [python 3.x - ImportError: cannnot import name 'Imputer' from 'sklearn.preprocessing' - Stack Overflow](https://stackoverflow.com/questions/59439096/importerror-cannnot-import-name-imputer-from-sklearn-preprocessing)

```python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
imputer = imputer.fit(X[ : , 1:3])
X[ : , 1:3] = imputer.transform(X[ : , 1:3])
```
## Step 4: Encoding categorical data
```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
labelencoder_X = LabelEncoder()
X[ : , 0] = labelencoder_X.fit_transform(X[ : , 0])
```
### Creating a dummy variable [^ColumnTransformer]

[^ColumnTransformer]: [python 3.x - TypeError: __init__() got an unexpected keyword argument 'categorical_features' - Stack Overflow](https://stackoverflow.com/questions/59476165/typeerror-init-got-an-unexpected-keyword-argument-categorical-features)

```python
from sklearn.compose import ColumnTransformer
ct = ColumnTransformer([("Country", OneHotEncoder(), [0])], remainder = 'passthrough')
X = ct.fit_transform(X)
labelencoder_Y = LabelEncoder()
Y =  labelencoder_Y.fit_transform(Y)
```
## Step 5: Splitting the datasets into training sets and Test sets [^model_selection]

[^model_selection]: [python - ImportError: No module named sklearn.cross_validation - Stack Overflow](https://stackoverflow.com/questions/30667525/importerror-no-module-named-sklearn-cross-validation)

> Why didn't sklearn developers put in an alias for backward compatibility? Also, the doc for that older version should indicate this refactor: scikit-learn.org/0.16/modules/generated/….
> – flow2k
> Commented Aug 15, 2019 at 22:23

> Because making code inoperable with API changes is one of the favorite features of python-based data science ;-) (See Pandas decade of version 0.x changes)
> – Jeff Winchell
> Commented Nov 12, 2021 at 14:52

```python
from sklearn.model_selection import train_test_split
X_train, X_test, Y_train, Y_test = train_test_split( X , Y , test_size = 0.2, random_state = 0)
```

## Step 6: Feature Scaling
```python
from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
X_train = sc_X.fit_transform(X_train)
X_test = sc_X.fit_transform(X_test)
```
### Done 😅
