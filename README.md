# Data-Cleaning-Transformation





### Overview of the Used Cars Dataset

The dataset contains information about used cars from 1900 to 2021, with a total of 426,880 entries and 26 columns.

**"Dataset Characteristics: Unique Values, Missing Data, and Definitions"**
Certainly! Here's a table with column names, definitions, and data types:

**Overview of the Used Cars Dataset**

| Column        | Definition                                | Dtype     |
|---------------|-------------------------------------------|-----------|
| id            | Unique identifier for each listing        | int64     |
| url           | URL of the listing                       | object    |
| region        | Geographical region                      | object    |
| region_url    | URL corresponding to the region           | object    |
| price         | Price of the car                         | int64     |
| year          | Manufacturing year of the car             | float64   |
| manufacturer  | Manufacturer or brand of the car          | object    |
| model         | Model of the car                         | object    |
| condition     | Condition of the car (e.g., good, fair)  | object    |
| cylinders     | Number of cylinders in the engine         | object    |
| fuel          | Type of fuel used by the car              | object    |
| odometer      | Odometer reading in miles                | float64   |
| title_status  | Title status of the car (e.g., clean)    | object    |
| transmission  | Transmission type (e.g., automatic)      | object    |
| VIN           | Vehicle Identification Number            | object    |
| drive         | Drive type (e.g., 4wd)                   | object    |
| size          | Size of the car (e.g., compact)          | object    |
| type          | Type of car (e.g., sedan)                | object    |
| paint_color   | Exterior color of the car                | object    |
| image_url     | URL of the car's image                   | object    |
| description   | Description of the car                   | object    |
| county        | County information (null for all entries)| float64   |
| state         | State where the car is located           | object    |
| lat           | Latitude of the car's location            | float64   |
| long          | Longitude of the car's location           | float64   |
| posting_date  | Date when the listing was posted         | object    |




| Column        | Definition                                        | Unique Values | Missing Values | Missing Percentage |
|---------------|---------------------------------------------------|---------------|----------------|---------------------|
| id            | Unique identifier for each car listing             | 426,880       | 0              | 0.0%                |
| url           | URL of the car listing                             | 426,880       | 0              | 0.0%                |
| region        | Geographic region where the car is located        | 404           | 0              | 0.0%                |
| region_url    | URL of the region                                  | 413           | 0              | 0.0%                |
| price         | Price of the used car (in currency)                | 15,655        | 0              | 0.0%                |
| year          | Year of the car's manufacturing                   | 114           | 1,205          | 0.28%               |
| manufacturer  | Manufacturer or brand of the car                  | 42            | 17,646         | 4.13%               |
| model         | Model of the car                                  | 29,667        | 5,277          | 1.24%               |
| condition     | Condition of the car (e.g., new, used, like new)  | 6             | 174,104        | 40.79%              |
| cylinders     | Number of cylinders in the car's engine           | 8             | 177,678        | 41.62%              |
| fuel          | Fuel type (e.g., gas, diesel)                     | 5             | 3,013          | 0.71%               |
| odometer      | Odometer reading of the car (in miles)            | 104,870       | 4,400          | 1.03%               |
| title_status  | Title status of the car (e.g., clean, salvage)    | 6             | 8,242          | 1.93%               |
| transmission  | Type of transmission (e.g., automatic, manual)   | 3             | 2,556          | 0.60%               |
| VIN           | Vehicle Identification Number                    | 118,264       | 161,042        | 37.73%              |
| drive         | Drive type (e.g., 4wd, fwd)                        | 3             | 130,567        | 30.59%              |
| size          | Size of the car (e.g., compact, full-size)        | 4             | 306,361        | 71.77%              |
| type          | Type of car body (e.g., sedan, SUV)               | 13            | 92,858         | 21.75%              |
| paint_color   | Exterior paint color                             | 12            | 130,203        | 30.50%              |
| image_url     | URL of the car's image                            | 241,899       | 68             | 0.02%               |
| description   | Description of the car listing                    | 360,911       | 70             | 0.02%               |
| county        | No non-null values for the 'county' column        | 0             | 426,880        | 100.0%              |
| state         | State where the car is located                    | 51            | 0              | 0.0%                |
| lat           | Latitude of the car's location                    | 53,181        | 6,549          | 1.53%               |
| long          | Longitude of the car's location                   | 53,772        | 6,549          | 1.53%               |
| posting_date  | Date and time when the listing was posted         | 381,536       | 68             | 0.02%               |



## **1- Dropping unwanted columns:**
   - useless columns or columns containing high missing values percentage['county', 'size']
   
         df.drop(['id', 'url', 'region_url', 'county', 'size','image_url', 'description'],
         axis = 1, inplace = True)


## 2- Numeric columns ['price', 'odometer', 'lat', 'long']:

| Column        | Unique Values | Missing Values | Missing Percentage |
|---------------|---------------|----------------|---------------------|
| price         | 15,655        | 0              | 0.0%                |
| odometer      | 104,870       | 4,400          | 1.03%               |
| lat           | 53,181        | 6,549          | 1.53%               |
| long          | 53,772        | 6,549          | 1.53%               |


I found high skewness in the ['price', 'odometer'] due to some outliers.

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


after that i filled the missing values in the 'odometer' with the median.

      # filling odometer with the median value
      df['odometer'] = df['odometer'].fillna(df['odometer'].median())
      
and for 'lat' & 'long', i populated the nulls with median value per each region

      # populating lat & long with the median value per each 'region'
      regions = df['region'].unique()
      
      # Fill missing lat and long based on median values for each region
      for region in regions:
          median_lat = df[df['region'] == region]['lat'].median()
          median_long = df[df['region'] == region]['long'].median()
          df.loc[(df['region'] == region) & (df['lat'].isnull()), 'lat'] = median_lat
          df.loc[(df['region'] == region) & (df['long'].isnull()), 'long'] = median_long

### 3- Date columns ['year', 'posted_date']

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

it contained some outlier, but i found knid of a relationship between these earlier years and the 'title_status' so i kept them.


![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/73c9cd32-8363-4f4b-bb7a-c0d3b2de0796)

# Handling missing values

### 1- Categorical columns with dominating values

| Column        | Value       | Proportion (%) |
|---------------|-------------|-----------------|
| transmission  | automatic   | 79.0            |
| transmission  | other       | 15.0            |
| transmission  | manual      | 6.0             |
| title_status  | clean       | 97.0            |
| title_status  | rebuilt     | 2.0             |
| title_status  | salvage     | 1.0             |
| fuel          | gas         | 84.0            |
| fuel          | other       | 7.0             |
| fuel          | diesel      | 7.0             |
| fuel          | hybrid      | 1.0             |
| fuel          | electric    | 0.0             |

these columns have dominatig values:
fuel(gas 85%)
title_status(clean 96%)
transmission(automatic 80%)
in addition to a low number of missing values
so i filled the missing values with the mode per each column.

      # filling title_status with mode value
      df['title_status'] = df['title_status'].fillna('clean')
      
      # filling transmission with mode value
      df['transmission'].fillna(df['transmission'].mode().iloc[0], inplace = True)
      
      # filling fuel with mode value
      df['fuel'].fillna(df['fuel'].mode().iloc[0], inplace = True)


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


