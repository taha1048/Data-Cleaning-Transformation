# Data-Cleaning-Transformation
in this project i meant to make this dataset more valid for different usages (ML, data visualization, data analysis,...etc)

### Overview of the Used Cars Dataset

The dataset contains information about used cars from 1900 to 2021, with a total of 426,880 entries and 26 columns.

#### Data Columns and Definitions:

| Column        | Non-Null Count | Dtype  | Description                                      |
|---------------|----------------|--------|--------------------------------------------------|
| id            | 426,880        | int64  | Unique identifier for each car listing           |
| url           | 426,880        | object | URL of the car listing                           |
| region        | 426,880        | object | Geographic region where the car is located       |
| region_url    | 426,880        | object | URL of the region                                |
| price         | 426,880        | int64  | Price of the used car (in currency)              |
| year          | 425,675        | float64| Year of the car's manufacturing                  |
| manufacturer  | 409,234        | object | Manufacturer or brand of the car                 |
| model         | 421,603        | object | Model of the car                                 |
| condition     | 252,776        | object | Condition of the car (e.g., new, used, like new)|
| cylinders     | 249,202        | object | Number of cylinders in the car's engine          |
| fuel          | 423,867        | object | Fuel type (e.g., gas, diesel)                    |
| odometer      | 422,480        | float64| Odometer reading of the car (in miles)           |
| title_status  | 418,638        | object | Title status of the car (e.g., clean, salvage)   |
| transmission  | 424,324        | object | Type of transmission (e.g., automatic, manual)  |
| VIN           | 265,838        | object | Vehicle Identification Number                   |
| drive         | 296,313        | object | Drive type (e.g., 4wd, fwd)                       |
| size          | 120,519        | object | Size of the car (e.g., compact, full-size)       |
| type          | 334,022        | object | Type of car body (e.g., sedan, SUV)              |
| paint_color   | 296,677        | object | Exterior paint color                            |
| image_url     | 426,812        | object | URL of the car's image                           |
| description   | 426,810        | object | Description of the car listing                   |
| county        | 0              | float64| No non-null values for the 'county' column      |
| state         | 426,880        | object | State where the car is located                   |
| lat           | 420,331        | float64| Latitude of the car's location                   |
| long          | 420,331        | float64| Longitude of the car's location                  |
| posting_date  | 426,812        | object | Date and time when the listing was posted        |



































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




### Second on the list would be date columns ['year', 'posted_date']

by extracting the posted_year from 'posted_date'[ 2021-04-26T21:20:19-0500],

it turned out that all entries were made in 2021, but we have models entered as 2022 edition. 

| index | year | posting_year |
|-------|------|--------------|
| 9738  | 2022.0 | 2021.0 |
| 32148 | 2022.0 | 2021.0 |
| 43183 | 2022.0 | 2021.0 |
| 65611 | 2022.0 | 2021.0 |
| 65612 | 2022.0 | 2021.0 |

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

### Next is string or text columns ['manufacturer', 'model', 'region']



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


