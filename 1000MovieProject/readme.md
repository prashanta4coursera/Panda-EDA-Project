![](https://seeklogo.com/images/M/movie-time-cinema-logo-8B5BE91828-seeklogo.com.png)

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
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/1.JPG)

###### Step 3 : Look into the data
```python
movie_df = raw_movie.copy()
movie_df.describe()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/2.JPG)

```python
movie_df.info()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/3.JPG)

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
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/6.JPG)

Q2. Which movies earned the maximum revenues ?
Answer : Top 10 movies by revenue
```python
max_revenue.sort_values(by = ['Revenue (Millions)','Title'], inplace = True, ascending = False)
max_revenue[:10]
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/7.JPG)

Q3. Which years have maximum movies from the list?
Answer :
```python
year_movie = pd.DataFrame(movie_df['Year'].dt.year.value_counts(), index = None)
year_movie.reset_index(inplace=True)
year_movie.rename(columns = {'index':'Year','Year':'No of Movies'}, inplace = True)
year_movie.sort_values(by=['Year'],inplace = True)
year_movie.set_index('Year', inplace = True)

sns.barplot(data = year_movie, x = year_movie.index.values, y = 'No of Movies')
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/8.JPG)

**Creating List of all the Actors in the 1000 movie database**

```python

movie_actor = movie_df.Actors.copy()

movie_actor = movie_actor.str.replace(', ',',')
movie_actor = list(movie_actor.str.split(','))

movie_actor_list = list(set(x for y in movie_actor for x in y))
movie_actor_list
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/9.JPG)

**Creating list of all Genre in the 1000 movie list**
```python
genre_list = movie_df.Genre.copy()
genre_list = list(genre_list.str.split(','))

genre_list_flat = list(set(x for y in genre_list for x in y))

genre_list_flat
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/10.JPG)

**Genre based revenues table**
```python
genre_revenue = []
genre_movies = []

for x in genre_list_flat : 
  genre_revenue.append(movie_df[movie_df.Genre.str.contains(x)]['Revenue (Millions)'].sum())
  genre_movies.append(movie_df[movie_df.Genre.str.contains(x)]['Revenue (Millions)'].count())

d = {'Genre':genre_list_flat,'Revenue (Millions)': genre_revenue,'No of Movies':genre_movies}
genre_df = pd.DataFrame(d).sort_values(by = ['Revenue (Millions)','Genre'], ascending = [False,True]).reset_index(drop = True)
genre_df['Revenue (Millions)/ Movie'] = genre_df['Revenue (Millions)'] / genre_df['No of Movies']
genre_df.round({'Revenue (Millions)/ Movie':2})
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/11.JPG)

**Top 20 movies from each Genre**
```python
top_movie = []
for x in genre_list_flat : 
  top_movie.append(movie_df.loc[movie_df['Genre'].str.contains(x)]['Title'][0:1].values)

new_list = list(x for y in top_movie for x in y)

d = {'Genre': genre_list_flat,'Top Movie': new_list}
pd.DataFrame(d)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/12.JPG)

**Top 20 actors based on no of movies acted**
```python
noofmovies = []
for x in movie_actor_list : 
  noofmovies.append(movie_df['Actors'].str.contains(x).sum())
actor_movie_dict = {'Actor':movie_actor_list, 'No of Movies':noofmovies}
pd.DataFrame(actor_movie_dict).sort_values(by=['No of Movies','Actor'],ascending = [False,True]).reset_index(drop = True)[:20]
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/13.JPG)

**Top 20 actors based on total revenues earned**
```python
for x in movie_actor_list : 
  actor_revenue.append(movies[movies.Actors.str.contains(x)]['Revenue (Millions)'].sum())

actor_revenue_dict = {'Actor':movie_actor_list,'Revenue':actor_revenue}
df_actor_revenue = pd.DataFrame(actor_revenue_dict).sort_values(by=['Revenue','Actor'],ascending = [False,True])
df_actor_revenue.reset_index(drop = True)[:20]
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/14.JPG)

**Revenues earned year on year by these 1000 movies**
```python
yearly_revenue = movies.groupby(movies['Year'].dt.year)[['Revenue (Millions)']].sum().reset_index()
sns.barplot(x = 'Year', y = 'Revenue (Millions)',data = yearly_revenue)
plt.show()
```
!![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/15.JPG)

**No of movies for each rating**
```python
rating_df = movie_df[['Title','Rating']].copy()
bins = np.arange(10)
bins
rating_df['Rate_Bins'] = pd.cut(rating_df.Rating, bins = bins, right = False)
rating_df_bins = rating_df.groupby('Rate_Bins',as_index=False).count()
rating_df_bins.rename(columns = {'Rate_Bins':'Ratings','Rating': 'No of Movies'}, inplace = True)
sns.barplot(x = 'Ratings',y = 'No of Movies',data = rating_df_bins)
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/16.JPG)

**Revenues earned for each movie ratings available**
```python

rating_rev_df = movies[['Rating','Revenue (Millions)']].copy()
rating_rev_df['Rate_Bins'] = pd.cut(rating_rev_df.Rating, bins = bins)
rating_rev = rating_rev_df.groupby('Rate_Bins',as_index = False)[['Revenue (Millions)']].sum()
rating_rev.columns = ['Ratings','Revenue (Millions)']
sns.barplot(x = 'Ratings', y = 'Revenue (Millions)', data = rating_rev)
plt.plot()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/17.png)

**Check for relations if any?
Heatmap
Pairplot**
```python
sns.heatmap(movies.corr(),annot = True)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/18.png)

```python
sns.pairplot(movies, diag_kind = 'kde')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/19.png)

**Relations :**

1. Metascore vs Rating
2. Revenue vs Votes
3. Votes vs Rating
```python
sns.jointplot(x = 'Metascore', y = 'Rating', data = movies, kind = 'reg')
sns.jointplot(x = 'Votes', y = 'Revenue (Millions)', data = movies, kind = 'reg')
sns.jointplot(x = 'Rating', y = 'Votes', data = movies, kind = 'reg')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/20.png)
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/21.png)
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/1000MovieProject/images/22.png)

**Conclusion**

Metascore vs Rating
Metascore is directly related to the ratings.

**Revenue vs Votes**
As the number of votes increases the revenue also increases. This maybe because the movie is popular and watched by people many times. But, the relation is still weak.

**Votes vs Rating**
As the no of votes increases, the rating also increases, though the relation is quite weaker here.
