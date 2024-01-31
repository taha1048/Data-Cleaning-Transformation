# Data-Cleaning-Transformation



## This project aims to enhance the validity and usability of a used cars dataset, preparing it for various applications such as machine learning, data visualization, and analysis.

**The project is organized into three main phases (with provided notebooks):**
1. Exploratory data analysis (EDA). 
2. Data cleaning. 
3. Application of machine learning algorithms for imputing some missing values.

**The detailed cleaning process involves:**
1. Handling outliers in numeric columns
2. Removing/Filling missing values with different techniques
3. Fixing typos and incosistent entries
4. Deleting useless rows & columns

## First let's have a closer look at the data and we will check each column individually later.


**Overview of the Dataset :**

The dataset contains information about used cars from 1900 to 2021, with a total of 426,880 entries (rows) and 26 columns.

Here's a table with column names, definitions, and data types:


| Column name       | Definition                                | Data type     |
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

## Data cleaning process


## **1- Dropping unwanted columns:**
   - useless columns or columns containing high missing values percentage['county', 'size']
   
         df.drop(['id', 'url', 'region_url', 'county', 'size','image_url', 'description'],
         axis = 1, inplace = True)


## 2- Price & Odometer
These columns are kinda related, as the higher the odometer is, the lower the price should be.

here's some details about them.

| Column        | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|---------------|---------------|----------------|---------------------|
| price         | 15,655        | 0              | 0.0%                |
| odometer      | 104,870       | 4,400          | 1.03%               |






I found high skewness in them due to some outliers.

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/b4cf9c36-d155-4e11-9c96-0bf72477bc7c)




With an overall shape like this:

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/0b867985-5737-46fd-9ec8-8b4d1a4db6d4)


I categorized the values in each column, and here is what I've got:

1. **Price Ranges:**
   - Zero Prices : 32,895 rows
   - 0 < Prices < 100 : 3,327 rows
   - 100 <= Prices < 1000 : 10,093 rows
   - 1000 <= Prices <= 10,000 : 129,922 rows
   - 10,000 < Prices <= 100,000 : 249,988 rows
   - Prices > 100,000 : 655 rows

2. **Odometer Ranges:**
   - Odometer <= 10 : 5,343 rows
   - Odometer <= 100 : 6,974 rows
   - Odometer <= 1,000 : 10,928 rows
   - Odometer <= 10,000 : 29,761 rows
   - Odometer <= 100,000 : 247,141 rows
   - Odometer <= 1,000,000 : 421,904 rows
   - Odometer <= 10,000,000 : 422,480 rows

I opted for these ranges for better data interpretation and less skewness:
- Price between (1,000 & 100,000)
- Odometer less than 1,000,000

Here is the result:

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/b845fdbd-5db2-4b62-97b2-dfa1f74470c6)


after that i filled the missing values in the 'odometer' with the median value as it contained small percentage of missing values (1.03%).

      # filling odometer with the median value
      df['odometer'] = df['odometer'].fillna(df['odometer'].median())
      

--- 

### 3- Date columns ['year', 'posted_date']

| Column        | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|---------------|---------------|----------------|---------------------|
| year          | 114           | 1205           | 0.28%               |
| posting_date  | 381536        | 68             | 0.02%               |


By extracting the year from the 'posted_date' [ 2021-04-26T21:20:19-0500],

it turned out that all entries were made in (2021), but we have models entered as (2022 edition). 

like this :

| row number | year | posting_year |
|-------|------|--------------|
| 9738  | 2022.0 | 2021.0 |
| 32148 | 2022.0 | 2021.0 |
| 43183 | 2022.0 | 2021.0 |
| 65611 | 2022.0 | 2021.0 |
| 65612 | 2022.0 | 2021.0 |

so i dropped:
- the 'posted_date' column as i'm only interested in years
- rows where the 'year' = 2022
- missing years from the data as it represented almost 0 percent of the column (0.28%).
  
       # dropping year 2022 and null years
       df = df[df['year'] != 2022]
       # dropping null years
       df.dropna(subset = 'year', inplace = True)
       # dropping 'posted_date'
       df.drop('posted_date', axis = 1, inplace = True)

here is the 'year' distribution 

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/e39e51e7-88b8-485b-bb36-2dfdab840442)


it contained some outlier (from 1900 to 1990), but these earlier years affect some old cars with (parts only & missing) status.



![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/815595b6-b090-4a35-a415-76e9f1b8cd5f)

also these outliers represent 6% of the whole dataset so i kept them.

---




### 4- Manufacturer & Model


| Column        | Unique Values | Missing Values | Missing Percentage |
|---------------|---------------|----------------|---------------------|
| manufacturer  | 42            | 17646          | 4.13%               |
| model         | 29667         | 5277           | 1.24%               |

i deleted rows where they both are missing

      # droping cars with no manufacturer and model
      df.dropna(subset = ['manufacturer', 'model'], how = 'all', inplace = True)

then filled the remaining missing value with a new category 'unknown'

      # filling manufacturer and model 'unknown'
      df['manufacturer'] = df['manufacturer'].fillna('unknown')
      df['model'] = df['model'].fillna('unknown')
we also got some typos in the 'model' column so i used some regular expressions to find and fill them
with 'unknown' if the row contains a 'manufacurer' and drop them if not.
      
      # creating a mask for the condition
      mask = ((df['model'].str.contains('^[\W\d]+$') == True) & (df['manufacturer'] == 'unknown'))
      mask2 = ((df['model'].str.contains('[@%$*#%*!=]') == True) &  (df['manufacturer'] == 'unknown'))
      
      # dropping typos with null manufacturer (unknown)
      df = df[~mask]
      df = df[~mask2]
      
      # filling the remaining with 'unknown'
      df.loc[df['model'].str.contains('^[\W\d]+$') == True, 'model'] = 'unknown'
      df.loc[df['model'].str.contains('[@%$*#%*!=]') == True, 'model'] = 'unknown'

      # dropping strange inputs
      df = df[df['model'].str.contains('[♿🔥]') == False]
      df = df[df['model'].str.contains('^[,-./]') == False]


### 5- Categorical columns with dominating values


| Column       | Unique Values | Missing Values | Missing Percentage |
|--------------|---------------|----------------|---------------------|
| Transmission | 3             | 2556           | 0.60%               |
| Title Status | 6             | 8242           | 1.93%               |
| Fuel         | 5             | 3013           | 0.71%               |

these column have low null percentage and dominating values as the following: 

| Column        | Value       | Proportion (%) |
|---------------|-------------|-----------------|
| transmission  | automatic   | 79.0            |
| title_status  | clean       | 97.0            |
| fuel          | gas         | 84.0            |

there will be no bias by filling them with the mode value

      # filling title_status with mode value
      df['title_status'] = df['title_status'].fillna('clean')
      
      # filling transmission with mode value
      df['transmission'].fillna(df['transmission'].mode().iloc[0], inplace = True)
      
      # filling fuel with mode value
      df['fuel'].fillna(df['fuel'].mode().iloc[0], inplace = True)

### Categorical columns related to the 'model'

| Column    | Unique Values | Missing Values | Missing Percentage |
|-----------|---------------|----------------|---------------------|
| Type      | 13            | 92858          | 21.75%              |
| Cylinders | 8             | 177678         | 41.62%              |
| Drive     | 3             | 130567         | 30.59%              |

these columns have high null percentage and with no significant dominating values
and sense the dataset contains 426,880 row and 29667 model, i populated these missing values with mode 
value per each model.

      # empty lists to save the result
       typ = []
      model_type = []
      models_null_type = []
      
      for model in models:
          # type mode value
          mode_value = df[df['model'] == model]['type'].mode()
          
          if not mode_value.empty:
              
              # assining values to the lists
              model_type.append(model)
              typ.append(mode_value.iloc[0])
          else:
              # models with no mode value
              models_null_type.append(model)
              
      # displaying the length of each list        
      print(len(model_type), len(typ), len(models_null_type)) 






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


