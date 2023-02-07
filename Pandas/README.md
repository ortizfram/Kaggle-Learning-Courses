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
# 2.0 Indexing, Selecting & Assigning
## 2.1 access a col
```py
reviews.country
# or
reviews['country']

# access first in col 
reviews['country'][0]
```
## 2.2 Indexing in pandas
- `iloc` for numerical position in the data
```py
# fist val
reviews.iloc[0]

# every record from col 0
reviews.iloc[:, 0]

# from 0 to 3 in 1st col
reviews.iloc[:3, 0]

#  select just the second and third entries,
reviews.iloc[1:3, 0]

#  pass a list:
reviews.iloc[[0, 1, 2], 0]

# last five elements of the dataset.
reviews.iloc[-5:]
```
- `loc` for label-based selection
```py
# first record in country
reviews.loc[0, 'country']

# select every record from these cols
reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
```
## 2.3 Manipulating the index
```py
# set a column as index
reviews.set_index("title")
```
## 2.4 conditionals
```py
# return records of this condition
reviews.loc[reviews.country == 'Italy']

# more than 1 condition 
reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]

# if 1 of both condition meets, return
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
```
```py
# isin : if exists return
reviews.loc[reviews.country.isin(['Italy', 'France'])]
```
```py
# isnull, notnull
reviews.loc[reviews.price.notnull()]
```
## 2.5 Assigning data
```py
reviews['critic'] = 'everyone'
reviews['critic']
```
```py
# iterable variables
reviews['index_backwards'] = range(len(reviews), 0, -1)
reviews['index_backwards']
```
## 2.6 Exercise: Indexing, Selecting & Assigning
see it [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Pandas/exercise-indexing-selecting-assigning.ipynb)

# 3.0 Summary Functions and Maps
Extract insights from your data.
```py
reviews.describe()
reviews.taster_name.describe()
```
```py
reviews.points.mean()
```
```py
reviews.taster_name.unique()
```
```py
reviews.taster_name.value_counts()
```
To see a list of unique values and how often they occur in the dataset `value_counts()`
-  high-level ****summary of the attributes**** for ****numerical data****
## 3.1 Maps
 creating new representations from existing data, or for transforming data from the format it is in now to the format that we want it to be in later
 `.map()`
```py
review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
```
`apply()` is the equivalent method if we want to transform a whole DataFrame by calling a custom method on each row.
```py
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```
- both create new serie or df , and does not apply in the original one
## 3.2 Exercise: Summary Functions and Maps
see exercise [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Pandas/exercise-summary-functions-and-maps.ipynb)
# 4.0 Grouping and Sorting
Scale up your level of insight. The more complex the dataset, the more this matters
```py
# same as unique.value_counts but with group by
reviews.groupby('points').points.count()
```
```py
reviews.groupby('points').price.min()
```
```py
# selecting the name of the first wine reviewed from each winery in the dataset:
reviews.groupby('winery').apply(lambda df: df.title.iloc[0])
```
```py
#  the best wine by country and province:
reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])
```
Another groupby() method worth mentioning is `agg()`, which lets you run a bunch of different functions on your DataFrame simultaneously. For example, we can generate a simple statistical summary of the dataset as follows:
```py
reviews.groupby(['country']).price.agg([len, min, max])
```
## 4.1 Multi-indexes
```py
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
countries_reviewed
```
```py
countries_reviewed.reset_index()
```
## 4.2 sorting 
```py
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
```
```py
countries_reviewed.sort_values(by='len', ascending=False)
```
```py
countries_reviewed.sort_index()
```
```py
# sort by more than one column at a time:
countries_reviewed.sort_values(by=['country', 'len'])
```
## 4.3 Exercise: Grouping and Sorting
see exercise [here](https://github.com/ortizfram/Kaggle-Learning-Courses/blob/main/Pandas/exercise-grouping-and-sorting.ipynb)
# 5.0 Exercise: Data Types and Missing Values
see exercise [here]()
