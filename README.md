# Movies Correlation in Python

Find what fields are directly correlated withthe gross revenue of a movie. It will be interesting to know what impcts the revenue of a film.


## Data Cleaning
The missing data section sees which field has missing data by percent. The for loop goes through the dataset to see if it has nulls in it

```
for col in df.columns:
    pct_missing = np.meandf.[col].isnull()]
    print('{} - {}%.format(col, pct_missing))
```



## Correlation
