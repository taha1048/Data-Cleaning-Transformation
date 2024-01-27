# Data-Cleaning-Transformation
in this project i meant to make this dataset more valid for different usages (ML, data visualization, data analysis,...etc)

## Filtering inconsistent data

## First, Numeric Columns ['price', 'odometer']

I found high skewness in them (right) due to some outliers.

![Skewness Plot](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/97c4892b-5c16-4278-b34b-bb331a362723)

With an overall shape like this:

![Overall Shape Plot](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/b44149cb-ecdd-4270-a608-3661cfb23231)

I tried to evaluate these outliers, and here is what I've found:

1. **Price Ranges:**
   - Zero Prices: 32,895 rows
   - 0 < Prices < 100: 3,327 rows
   - 100 <= Prices < 1000: 10,093 rows
   - 1000 <= Prices <= 10,000: 129,922 rows
   - 10,000 < Prices <= 100,000: 249,988 rows
   - Prices > 100,000: 655 rows

2. **Odometer Ranges:**
   - Odometer <= 10: 5,343 rows
   - Odometer <= 100: 6,974 rows
   - Odometer <= 1,000: 10,928 rows
   - Odometer <= 10,000: 29,761 rows
   - Odometer <= 100,000: 247,141 rows
   - Odometer <= 1,000,000: 421,904 rows
   - Odometer <= 10,000,000: 422,480 rows

I decided to select these ranges for better data interpretation and less skewness:
- Price between (1,000 & 100,000)
- Odometer less than 1,000,000

Here is the result:

![Cleaned Data Plot](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/ba84a941-12e4-49c7-a6a4-e8682f74fa85)


Certainly! Let's refine the second section:

### 2. **Consistent Formatting:**

```markdown
## Second on the List: Date Columns ['year', 'posted_date']

By extracting the posted_year from 'posted_date' [2021-04-26T21:20:19-0500], it turned out that all entries were made in 2021, but we have models entered as the 2022 edition.

```python
# Displaying entries with 'year' as 2022
df[df['year'] == 2022].head()
```

| index | year | posting_year |
|-------|------|--------------|
| 9738  | 2022.0 | 2021.0 |
| 32148 | 2022.0 | 2021.0 |
| 43183 | 2022.0 | 2021.0 |
| 65611 | 2022.0 | 2021.0 |
| 65612 | 2022.0 | 2021.0 |

So, I dropped the 'posted_date' column, as I'm only interested in years. I also deleted rows where the 'year' is 2022 and rows with missing years, as they represented almost 0 percent of the column.

```python
# Dropping 'year' 2022 and null years
df = df[df['year'] != 2022]
# Dropping null years
df.dropna(subset='year', inplace=True)
# Dropping 'posted_date'
df.drop('posted_date', axis=1, inplace=True)
```

The 'year' distribution contained some outliers, but I found a kind of relationship between these low years and the 'title_status' so I kept them.

```python
# Displaying 'year' distribution
df['year'].value_counts().sort_index()
```

![Year Distribution Plot](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/4feec60b-4821-46b6-b8e2-5f46e024c5c5)

It contained some outliers, but I found a kind of relationship between these low years and the 'title_status,' so I kept them.

```python
# Displaying relationship between low 'year' and 'title_status'
df[df['year'] < 1990]['title_status'].value_counts()
```

![Title Status vs. Low Year Plot](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/73c9cd32-8363-4f4b-bb7a-c0d3b2de0796)
```

Feel free to adjust the text as needed. Ensure that the code snippets are formatted correctly, and the images' URLs are correct.





























### Second on the list would be Date columns ['year', 'posted_date']

by extracting the posted_year from 'posted_date'[ 2021-04-26T21:20:19-0500],

it turned out that all entries were made in 2021, but we have models entered as 2022 edition. 

    index   year	posting_year
    9738	2022.0	2021.0
    32148	2022.0	2021.0
    43183	2022.0	2021.0
    65611	2022.0	2021.0
    65612	2022.0	2021.0
    ...	        ...	    ...
    410935	2022.0	2021.0
    410936	2022.0	2021.0
    412094	2022.0	2021.0
    413805	2022.0	2021.0
    423091	2022.0	2021.0

so i dropped the 'posted_date' column as i'm only interested in years, also deleted rows where

the 'year' = 2022, and missing years from the data as it presented almost 0 percent of the column.

    # dropping year 2022 and null years
    df = df[df['year'] != 2022]
    # dropping null years
    df.dropna(subset = 'year', inplace = True)
    # dropping 'posted_date'
    df.drop('posted_date', axis = 1, inplace = True)

here is the 'year' distribution 

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/4feec60b-4821-46b6-b8e2-5f46e024c5c5)

it contained some outlier, but i found knid of a relationship between these low years and the 'title_status' so i kept them.


![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/73c9cd32-8363-4f4b-bb7a-c0d3b2de0796)

### Next is string or text columns ['manufacturer', 'model', 'region'



high percentage of missing values in :
['condition', 'cylinders', 'VIN', 'drive', 'type', 'paint_color']

high skewness in : 
['price', 'odometer', 'year']

dominating values in categorical columns like:
fuel(gas 85%)
title_status(clean 96%)
transmission(automatic 80%)
in addition to a low number of missing values

all the data were input ('posing_date') in 2021
some inconsistencies with the 'year' as it's after the posting year


