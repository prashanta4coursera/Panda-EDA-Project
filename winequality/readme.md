
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
download the file from the source.
winequality_df = pd.read_csv('https://raw.githubusercontent.com/insaid2018/Term-1/master/Data/Projects/winequality.csv')
winequality_df.head(3)
```
|fixed acidity|	volatile acidity|	citric acid|	residual sugar|	chlorides|	free sulfur dioxide|	total sulfur dioxide|	density|	pH| sulphates	|alcohol	|quality|  
|---|---|---|---|---|---|---|---|---|---|---|  
|1|2|3|4|5|6|7|8|9|10|11|12|





