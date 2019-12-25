This project is an EDA on 1000 movie dataset provided by insaid. The data can be downloaded from [1000 Movie][100Movie]

[100Movie]: https://raw.githubusercontent.com/insaid2018/Term-1/master/Data/Projects/1000%20movies%20data.csv "1000 Movie"

###### Step 1: Import all the libraries

 ```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

###### Step 2 : Read the data
```python
raw_movie = pd.read_csv('https://raw.githubusercontent.com/insaid2018/Term-1/master/Data/Projects/1000%20movies%20data.csv', sep=',', parse_dates=['Year'], index_col = 'Rank')
raw_movie.head(3)
```

###### Step 3 : Look into the data
```python
movie_df = raw_movie.copy()
movie_df.describe()
```
```python
movie_df.info()
```

###### Step 4: Let's remove missing values
```python
movie_df.isnull().sum()
```
```python
movies = movie_df.copy()
movies.dropna(axis = 0, how ='any', inplace = True)
movies.isnull().sum()
movies.shape
```


###### Step 5: Lets Check if there is any duplicate value
```python
movies[movies.duplicated()]
```
###### Step 6: Movie data is OK for EDA, let's perform
Q1 : Which are the longest duration movie ?
Anwer : 
```python
long_movies = movie_df[['Title','Runtime (Minutes)']]
long_movies.sort_values(by = ['Runtime (Minutes)','Title'], ascending = [False,True])[:10]
```


Q2. Which movies earned the maximum revenues ?
Answer : Top 10 movies by revenue
```python
max_revenue.sort_values(by = ['Revenue (Millions)','Title'], inplace = True, ascending = False)
max_revenue[:10]
```
