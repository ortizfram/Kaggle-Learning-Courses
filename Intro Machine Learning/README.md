# 1.1 How Models Work?
Your cousin has made millions of dollars speculating on real estate. He's offered to become business partners with you because of your interest in data science. He'll supply the money, and you'll supply models that predict how much various houses are worth.

You ask your cousin how he's predicted real estate values in the past, and he says it is just intuition. But more questioning reveals that he's identified price patterns from houses he has seen in the past, and he uses those patterns to make predictions for new houses he is considering.

Machine learning works the same way. We'll start with a model called the Decision Tree. There are fancier models that give more accurate predictions. But ***decision trees*** are easy to understand, and they are the basic building block for some of the best models in data science
<img src="https://user-images.githubusercontent.com/51888893/217024834-f79255d1-2e68-4d74-a52a-3887a0c5075a.png" width=500px>

***df is divided into 2 categories:***
- more than 2 bedrooms -YES -NO

This step of capturing patterns from data is called ***fitting or training the model***

#### how to improve decision tree
You can capture more factors using a tree that has more "splits." These are called "deeper" trees. A decision tree that also considers the total size of each house's lot might look like this:

***"deeper trees"***

<img src="https://user-images.githubusercontent.com/51888893/217027388-6f42f47c-8c69-4734-9084-93472ce1a5e3.png" width=500px>

***the bottom in the prediction is called ("leaf")***

---
## 1.2 Basic Data Exploration
```py
import pandas as pd
```
We load and explore the data with the following commands:
```py
      # save filepath to variable for easier access
      melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
      # read the data and store data in DataFrame titled melbourne_data
      melbourne_data = pd.read_csv(melbourne_file_path) 
      # print a summary of the data in Melbourne data
      melbourne_data.describe()
```
## 1.3 basic-data-exploration exercise
see exercise [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Intro%20Machine%20Learning/exercise-explore-your-data.ipynb)

# 2.0 Your First Machine Learning Model

## 2.1Selecting Data for Modeling

```py
melbourne_data.columns
```
```py
# The Melbourne data has some missing values (some houses for which some variables weren't recorded.)
# We'll learn to handle missing values in a later tutorial.  
# Your Iowa data doesn't have missing values in the columns you use. 
# So we will take the simplest option for now, and drop houses from our data. 
# Don't worry about this much for now, though the code is:

# dropna drops missing values (think of na as "not available")
melbourne_data = melbourne_data.dropna(axis=0)
```
## 2.3 Selecting The Prediction Target
You can pull out a variable with dot-notation.

We'll use the dot notation to select the column we want to predict, which is called the ***prediction target.*** By convention, the prediction target ***is called y.***
```py
y = melbourne_data.Price
```
## 2.4 Choosing "Features"
 In our case, those would be the columns used to determine the home price. Sometimes, you will use all columns except the target as features. ***Other times you'll be better off with fewer features.***
```py
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
```
choose some, almost all cases would be all cols, execpt the target one
```py
# by convention, feature is called X
X = melbourne_data[melbourne_features]
```
```py
# Visually checking your data. You'll frequently find surprises in the dataset that deserve further inspection.
X.describe()
X.head()
```
## 2.5 Building Your Model
scikit-learn 

The steps to building and using a model are:
- Define: What type of model will it be? A decision tree? Some other type of model? Some other parameters of the model type are specified too.
- Fit: Capture patterns from provided data. This is the heart of modeling.
- Predict: Just what it sounds like
- Evaluate: Determine how accurate the model's predictions are.
```py
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model
melbourne_model.fit(X, y)
```
```py
print("Making predictions for the following 5 houses:")
print(X.head())
print("The predictions are")
print(melbourne_model.predict(X.head()))
```
## 2.6 first machine learning model execise
see execise [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Intro%20Machine%20Learning/exercise-your-first-machine-learning-model.ipynb)

# 3.0 Model Validation
the relevant measure of model quality is predictive accuracy. In other words, ***will the model's predictions be close to what actually happens.***

If you compare predicted and actual home values for 10,000 houses, you'll likely find mix of good and bad predictions. Looking through a list of 10,000 predicted and actual values would be pointless. We need to summarize this into a single metric.

There are many metrics for summarizing model quality, but we'll start with one called ***Mean Absolute Error (also called MAE)***. Let's break down this metric starting with the last word, error.

***The prediction error for each house is:***
```py
error=actual−predicted
```
With the ****MAE metric**** , we take the absolute value of each error. This converts each error to a positive number. We then take the average of those absolute errors. This is our measure of model quality. In plain English, it can be said as

> On average, our predictions are off by about X.

Once we have a model, here is how we calculate the mean absolute error:
```py
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
```
## 3.1 validation for MAE (train_test_split)
```py
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
# Define model
melbourne_model = DecisionTreeRegressor(random_state = 0)
# Fit model
melbourne_model.fit(train_X, train_y)

# get predicted prices on validation data
val_predictions = melbourne_model.predict(val_X)
print(mean_absolute_error(val_y, val_predictions))
```
## 3.2 Exercise: Model Validation
see it [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Intro%20Machine%20Learning/exercise-model-validation.ipynb)
