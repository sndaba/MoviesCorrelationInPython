# Movies Correlation in Python
![movies](https://github.com/sndaba/MoviesCorrelationInPython/assets/53818579/ba928431-0125-4419-861c-c180acfbf858)


This projects aims to find what fields are directly correlated with the gross revenue of a movie. It will be interesting to know what impcts the revenue of a film.

## Import libraries
The libraries that are going to be used are pandas, numpy for data manipulation and transforming, matplotlib and seaborn for visuals.

```
import pandas as pd
import numpy as np
import seaborn as sns

import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib
plt.style.use('ggplot')
from matplotlib.pyplot import figure

%matplotlib inline
matplotlib.rcParams['figure.figsize'] = (12,8)

pd.options.mode.chained_assignment = None

# Now we need to read in the data
df = pd.read_csv(r'https://raw.githubusercontent.com/sndaba/MoviesCorrelationInPython/main/movies.csv')
```

## Data Cleaning
### 1. Missing Values
The missing data section sees which field has missing data by percent. The for loop goes through the dataset to see if it has nulls in it

```
for col in df.columns:
    pct_missing = np.meandf.[col].isnull()]
    print('{} - {}%.format(col, pct_missing))
```
### 2. Change data type
Changing "gross" and "budget" columns to make them whole numbers

```
df['budget'] = df['budget'].astype['int64']
df['gross'] = df['gross'].astype['int64']
```
### 3. Drop duplicates
Drop any duplicate existing in the dataset

```
df.drop_duplicates()
```
### 4. Sorting by 'gross' column
Order the data to see the highest grossing movies by 'gross' column

```
df.sort_values(by=['gross'], inplace=False, ascending=False)
```

## Finding Correlations
Before starting to find any correlations, the hypothesis is that 'budget', 'company' have a high correlation. These are columns to test with the 'gross' column.

### 1. 'budget' and 'gross' column scatter plot
A scatter plot can be used to show the 'budget' and 'gross' using matplotlib
```
plt.scatter(x=df['budget'], y=df['gross'])
plt.title('Budget vs Gross Earnings')
plt.xlabel('Gross Earnings')
plt.ylabel('Budget for film')
plt.show
```
### 2. Correlation between 'budget' and 'gross'
The scatter plot kind of shows whether the 'gross' and 'budget' are visually correlated. However, its time to determine whether they arecorrelated using the regplot(regression plot) using seaborn.
 
```
sns.regplot(x="gross", y="budget", data=df, scatter_kws={"color":"red"}, line_kws={"color":"blue"})
```
### 3. Correlation Matrix between all numeric columns
Using pearson, kendall and spearman. However, keeping to the default 'pearson'.
```
df.corr(method ='pearson') #pearson, kendall and spearman
```
```
#results show high correlation between 'budget' and 'gross'
```

### 4. Correlation matrix visual 
Using seaborn and matplotlib
```
correlation_matrix = df.corr()

sns.heatmap(correlation_matrix, annot = True)

plt.title("Correlation matrix for Numeric Features")

plt.xlabel("Movie features")

plt.ylabel("Movie features")

plt.show()
```

### 5. Numerise all columns
The 'company' data type is set as an object. To convert this, it is best to change it to category data type. The other coulumns that are already numeric, will be left alone, that is, they wil not be changed.
```
df_numerized = df

for col_name in df_numerized.columns:
    if(df_numerized[col_name].dtype == 'object'):
        df_numerized[col_name]= df_numerized[col_name].astype('category')
        df_numerized[col_name] = df_numerized[col_name].cat.codes
        
df_numerized
```

### 6. Visualisation of the numerised columns
```
correlation_matrix = df_numerized.corr(method='pearson')

sns.heatmap(correlation_matrix, annot = True)

plt.title("Correlation matrix for Movies")

plt.xlabel("Movie features")

plt.ylabel("Movie features")

plt.show()
```

### 7. Correlation of numerised columns
```
df_numerized.corr(method='pearson')
```
### 8. Pairing of correlated columns
A sorted linear pairing of the correlated columns 

```
correlated_mat = df_numerized.corr()
corr_pairs = correlation_mat.unstack()
sorted_pairs = corr_pairs.sort_values() # kind="quicksort")

print(sorted_pairs)
```

```
#the ones that have a high correlation (> 0.5)

strong_pairs = sorted_pairs[abs(sorted_pairs) > 0.5]

print(strong_pairs)
```
