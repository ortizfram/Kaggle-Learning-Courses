Solve short hands-on challenges to perfect your data manipulation skills
# 1.0 Creating, Reading and Writing
## 1.1 Creating data
There are two core objects in pandas: the DataFrame and the Series.
#### DataFrame
 table. It contains an array of individual entries, each of which has a certain value. Each entry corresponds to a row (or record) and a column.

For example, consider the following simple DataFrame:
```py
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```
<img src="https://user-images.githubusercontent.com/51888893/217230365-ae85cf54-a1c9-43c5-9207-92a152bf3d64.png" width=100px>

***DataFrame entries are not limited to integers***
```py
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 'Sue': ['Pretty good.', 'Bland.']})
```
<img src="https://user-images.githubusercontent.com/51888893/217233361-f0c3b88a-ff2d-4e6e-9787-45ab317d7489.png" width=200px>

```py
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```
<img src="https://user-images.githubusercontent.com/51888893/217233665-8a1f3e6b-0c50-4cde-938d-d95c4ef49497.png" width=200px>

#### Series
is a sequence of data values. If a DataFrame is a table, a Series is a list. And in fact you can create one with nothing more than a list:
```py
pd.Series([1, 2, 3, 4, 5])
```

    0    1
    1    2
    2    3
    3    4
    4    5
    dtype: int64
 in essence, a ***single column of a DataFrame***. So you can assign row labels to the Series the same way as before, using an index parameter. However, a ***Series does not have a column name*** , it only has one overall name:
```py
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```
    2015 Sales    30
    2016 Sales    35
    2017 Sales    40
    Name: Product A, dtype: int64
    
## 1.2 CSV into a DataFrame
```py
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
```
check how large the resulting DataFrame is:
```py
wine_reviews.shape
```
    (129971, 14)
So our new DataFrame has 130,000 records split across 14 different columns
```py
wine_reviews.head(3)
```
<img src="https://user-images.githubusercontent.com/51888893/217237004-bc3b3545-1ac2-4375-9c83-610662f45c46.png" width=500px>

 built-in index, which pandas did not pick up on automatically. To make pandas use that column for the index (instead of creating a new one from scratch), we can specify an `index_col`
```py
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()
```
<img src="https://user-images.githubusercontent.com/51888893/217237327-fdc9d8a6-6a7a-485e-a19e-632f15c08ee1.png" width=500px>

## 1.3 Exercise: Creating, Reading and Writing
see exercise [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Pandas/exercise-creating-reading-and-writing.ipynb)
