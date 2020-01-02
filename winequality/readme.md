
![wine](https://static.vinepair.com/wp-content/uploads/2018/01/blackwine-internal.jpg)  

# Content : 
#### 1. [Understanding the wine data columns](#understanding)
#### 2. [Perform basic data check](#perform)
#### 3. [Perform relation analysis by graphical approach](#relation)
#### 4. [Cleaning the data](#cleaning) 
#### 5. [Checking the relations after cleaning](#checking)
#### 6. [Conclusion](#conclusion)

<a id='understanding'></a>
## **Understanding the wine data columns**
The dataset is related to red and white variants of the Portuguese **"Vinho Verde"** wine

1. **fixed acidity**  
most acids involved with wine or fixed or nonvolatile (do not evaporate readily)


2. **volatile acidity**  
the amount of acetic acid in wine, which at too high of levels can lead to an unpleasant, vinegar taste


3. **citric acid**  
found in small quantities, citric acid can add 'freshness' and flavor to wines


4. **residual sugar**  
the amount of sugar remaining after fermentation stops, it's rare to find wines with less than 1 gram/liter and wines with greater than 45 grams/liter are considered sweet


5. **chlorides**
the amount of salt in the wine


6. **free sulfur dioxide**  
the free form of SO2 exists in equilibrium between molecular SO2 (as a dissolved gas) and bisul-fite ion; it prevents microbial growth and the oxidation of wine  


7. **total sulfur dioxide**  
amount of free and bound forms of S02; in low concentrations, SO2 is mostly undetectable in wine, but at free SO2 concentrations over 50 ppm, SO2 becomes evident in the nose and taste of wine


8. **density**  
the density of water is close to that of water depending on the percent alcohol and sugar con-tent


9. **pH**  
describes how acidic or basic a wine is on a scale from 0 (very acidic) to 14 (very basic); most wines are between 3-4 on the pH scale


10. **sulphates**  
a wine additive which can contribute to sulfur dioxide gas (S02) levels, wich acts as an antimicro-bial and antioxidant


11. **alcohol**  
the percent alcohol content of the wine


13. **quality**  
output variable (based on sensory data, score between 0 and 10)

---

<a id='perform'></a>
## **Perform basic data check**  
Import all libraries
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

```python
# download the file from the source.
winequality_df = pd.read_csv('https://raw.githubusercontent.com/insaid2018/Term-1/master/Data/Projects/winequality.csv')
winequality_df.head(3)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/1.JPG)  


We will add our own definition of quality of wine based on quality index.
```python
# Good   = [8,9]
# Medium = [5,6,7]
# Poor   = [3,4]

winequality_df['overall'] = winequality_df['quality'].apply(lambda x : 'Poor' if x < 5 else 'Medium' if x < 8 else 'Good' )
winequality_df.overall = winequality_df.overall.astype('category')
```

Let's see the stats of the data.  
```python
winequality_df.describe()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/2.JPG)  

Check if there is any NULL values.  
```python
winequality_df.isnull().sum()

# No null values as output.
```

![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/3.JPG)  

check the datatype
```python
winequality_df.info()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/4.JPG)  

<a id='relation'></a>
## **Perform relation analysis by graphical approach**  
Let's analyse the relationship with pairplot  
Make PairPlots with PairGrid function to have more control.
```Python
g = sns.PairGrid(winequality_df)
g = g.map_upper(sns.kdeplot, cmap = 'YlOrBr', shade = True, shade_lowest = False)     #KDE
g = g.map_diag(plt.hist, color = '#1c9446')                                           #Histogram
g = g.map_lower(sns.scatterplot, color='#e68e12', edgecolor = 'k')                    #Scatter

g.fig.tight_layout()
plt.subplots_adjust(top=0.9, hspace=0.9)
g.fig.suptitle('Variable Relationship')
plt.show()
```   
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/5.png)  

Check the strength of the correlation among the variables. 
```python
plt.figure(figsize=(12,8))
ax = sns.heatmap(winequality_df.corr(), cmap='Greens', annot=True)
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/6.png)  

Most important variables   
```python
winequality_df.corr()[['quality']].sort_values(by='quality', ascending = False)

# alcohol, density, volatile acidity, chlorides influence the quality of wine in the order. 
```   
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/7.JPG)  

plotting the relationship among the important variables   
```python
g = sns.PairGrid(winequality_df[['alcohol','density','volatile acidity','chlorides']])
g = g.map_upper(sns.kdeplot, cmap='YlGnBu', shade=True, shade_lowest=False)
g = g.map_diag(plt.hist, color='#6cbd5e')
g = g.map_lower(sns.scatterplot, color='#b8e0b1', edgecolor='k')

g.fig.tight_layout()
g.fig.suptitle('Relationship')
plt.subplots_adjust(top=0.9)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/8.png)  

Strong relations :  
1. free sulfur dioxide ~ total sulfur dioxide  
2. alcohol ~ density  

Moderate relations :  
1. total sulfur dioxide ~ residual sugar  
2. density ~ residual sugar  

```python
# Plot JointPlot
# free sulfur dioxide ~ total sulfur dioxide

g = sns.JointGrid(x='free sulfur dioxide', y='total sulfur dioxide', data=winequality_df)
g = g.plot_joint(sns.regplot, color='#eb447e')
g = g.plot_marginals(sns.distplot, color='#fc7f03')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/9.png)  

```python
# Plot JointPlot
# alcohol ~  density

g = sns.JointGrid(x='alcohol', y='density', data=winequality_df)
g = g.plot_joint(sns.regplot, color='#eb447e')
g = g.plot_marginals(sns.distplot, color='#fc7f03')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/10.png) 

```python
# Plot JointPlot
# total sulfur dioxide ~ residual sugar

g = sns.JointGrid(x='total sulfur dioxide', y='residual sugar', data=winequality_df)
g = g.plot_joint(sns.regplot, color='#eb447e')
g = g.plot_marginals(sns.distplot, color='#fc7f03')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/11.png)  

```python
# Plot JointPlot
# density ~ residual sugar

g = sns.JointGrid(x='density', y='residual sugar', data=winequality_df)
g = g.plot_joint(sns.regplot, color='#eb447e')
g = g.plot_marginals(sns.distplot, color='#fc7f03')
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/12.png)  

reshape the dataframe with pd.melt for preparing a facetgrid.  
```python
facet_grid_df = pd.melt(winequality_df, id_vars=['quality','overall'], value_vars=winequality_df.columns[:11], var_name='variable', value_name='value')
facet_grid_df.sort_values(by=['variable','quality'], ascending=[True,True], inplace=True)
facet_grid_df.reset_index(drop=True, inplace=True)
facet_grid_df.quality = facet_grid_df.quality.astype('str')
facet_grid_df.quality = facet_grid_df.quality.astype('category')
facet_grid_df.head(3)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/13.JPG)  

Distribution of various varibales across the wine quality : FacetGrid  
```python
# FacetGrid

g = sns.FacetGrid(facet_grid_df, col='variable', col_wrap=4, hue='overall', palette={'Poor':'#F7E3DE', 'Medium':'#E99494', 'Good':'#DB0000'}, 
                  sharey=False, sharex=False, height=3, aspect=1.3, legend_out=True, hue_order=['Poor','Medium','Good'])
g.map(sns.violinplot, 'quality', 'value', order=['3', '4', '5', '6', '7', '8', '9'])

g.fig.tight_layout()
plt.subplots_adjust(top=0.8, hspace=0.8)
g.fig.suptitle('Wine Quality based on variables')
legend = ['Poor','Medium','Good']
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/14.png)  

Column bar sugesting the variation of the quality of wine with vriation of variable quantity.  
```python
# Plot bar graph for the median values.

d = facet_grid_df.groupby(['variable','overall'], as_index=False)[['value']].median()

g = sns.FacetGrid(d, col='variable', col_wrap=4, sharey=False, sharex=False, height=3, aspect=1, legend_out=True, 
                  hue='overall', hue_order=['Poor','Medium','Good'],palette='RdPu')
g.map(sns.barplot, 'overall', 'value', order=['Poor','Medium','Good'])

g.fig.tight_layout()
plt.subplots_adjust(top=0.8, hspace=0.8)
g.fig.suptitle('Median values of wine quality types')
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/15.png)  

<a id='cleaning'></a>
## Cleaning the data  
We want to get rid of the extreme outliers.  
How we do it ?   

Reshape the dataframe  
Drop rows below 1% and above 99% quantile.  
```python
# Reshape the data with melt function so that facetgrid can be applied on the variables.

corrected_melt = pd.melt(winequality_df, id_vars=['quality','overall'], value_vars=winequality_df.columns[:11], var_name='variable', value_name='value')
corrected_melt.sort_values(by=['variable','quality'], ascending=[True,True], inplace=True)
corrected_melt.reset_index(drop=True, inplace=True)
corrected_melt.quality = facet_grid_df.quality.astype('str')
corrected_melt.quality = facet_grid_df.quality.astype('category')
corrected_melt.head(3)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/16.JPG)  

```python
# Create 1% quantile data
p01 = corrected_melt.groupby(['quality','variable'], as_index=False)[['value']].quantile(.01)
p01.rename(columns={'value':'1%'}, inplace=True)

# Create 99% quantile data
p99 = corrected_melt.groupby(['quality','variable'], as_index=False)[['value']].quantile(.99)
p99.rename(columns={'value':'99%'}, inplace=True)

# Merge both columns
corrected_df = corrected_melt.merge(p01, on=['quality','variable']).merge(p99, on=['quality','variable'])

# A function to create columns for 1% and 99% quantile data
def corrected_value(value,one,ninenine) : 
  if (value >=one) and (value <=ninenine) : 
    return value
  else :  
    return np.NaN

# Apply the function
corrected_df['corrected value'] = corrected_df.apply(lambda x: corrected_value(x['value'], x['1%'], x['99%']), axis=1)
corrected_df.dropna(axis=0, how='any', inplace=True)
```
```python
# Check the record
corrected_df.shape
```
(70221, 7)

```python
#Select only the original column, drop the 1% and 99% columns from the frame.
corrected_df = corrected_df[['quality','overall','variable','value']]
corrected_df.head(3)
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/17.JPG)  

<a id='checking'></a>
## **Checking the relations after cleaning**
```python
# Again a FacetGrid for the violinplot.

g = sns.FacetGrid(corrected_df, col='variable', col_wrap=4, hue='overall', palette={'Poor':'#F7E3DE', 'Medium':'#E99494', 'Good':'#DB0000'}, 
                  sharey=False, sharex=False, height=3, aspect=1.3, legend_out=True, hue_order=['Poor','Medium','Good'])
g.map(sns.violinplot, 'quality', 'value', order=['3', '4', '5', '6', '7', '8', '9'])

g.fig.tight_layout()
plt.subplots_adjust(top=0.8, hspace=0.8)
g.fig.suptitle('Wine Quality based on variables')
legend = ['Poor','Medium','Good']
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/18.png)  

```python
# Plot bar for the median values.

d = corrected_df.groupby(['variable','overall'], as_index=False)[['value']].median()

g = sns.FacetGrid(d, col='variable', col_wrap=4, sharey=False, sharex=False, height=3, aspect=1, legend_out=True, 
                  hue='overall', hue_order=['Poor','Medium','Good'],palette='RdPu')
g.map(sns.barplot, 'overall', 'value', order=['Poor','Medium','Good'])

g.fig.tight_layout()
plt.subplots_adjust(top=0.8, hspace=0.8)
g.fig.suptitle('Median values of wine quality types')
plt.show()
```
![](https://github.com/prashanta4coursera/Panda-EDA-Project/blob/master/winequality/images/19.png)  

## **Conclusion**
1. As alcohol level increase ==> Quality increases
2. As chlorides level decreases ==> Quality increases
3. As citric acid level increases ==> Quality increases
4. As density decreases ==> Quality increases
5. fixed acidity ==> can't say impact on Quality
6. As free sulfur dioxide increases ==> Quality increases
7. pH ==> can't say impact on Quality
8. As residual sugar increases ==> Quality increases
9. sulphates ==> can't say impact on Quality
10. total sulfur dioxide ==> can't say impact on Quality
11. As the volatile acidity decreases ==> Quality increases

But since, only below four contributes towards winequality :
alcohol, density, volatile acidity, chlorides  

####################################################################################
1. Increase in the alcohol qty, increases the quality of the wine.
2. Decrease in the density of the wine, increases the quality of the wine.
3. Decrease in the volatile acidity of the wine, increases the quality of the wine.
4. Decrease in chlorides, increases the quality of the wine.  

####################################################################################









