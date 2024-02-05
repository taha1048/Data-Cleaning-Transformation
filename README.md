# Data-Cleaning-Transformation



## This project aims to enhance the validity and usability of the dataset, preparing it for various applications such as machine learning, data visualization, and analysis.

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


### **1- Dropping unwanted columns:**
   - useless columns or columns containing high missing values percentage 'county' (100%)
   
         df.drop(['id', 'url', 'region_url', 'county', 'image_url', 'description'],
         axis = 1, inplace = True)


### 2- Price & Odometer
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

| Column        | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|---------------|---------------|----------------|---------------------|
| manufacturer  | 42            | 17646          | 4.13%               |
| model         | 29667         | 5277           | 1.24%               |

These ones are crucial for the dataset as many features depend on them, but the problem is all the missing values in the 'manufacturer' aren't totally random.

There are specific 'models' don't have any manufacturer at all, so
1. i deleted rows where they both are missing

         # droping cars with no manufacturer and model
         df.dropna(subset = ['manufacturer', 'model'], how = 'all', inplace = True)

2. then filled the remaining missing values in the 'manufacturer' column with a new category 'unknown'

         # filling manufacturer with'unknown'
         df['manufacturer'] = df['manufacturer'].fillna('unknown')
3. dropped the null values in the model column
   
         df.dropna(subset = 'model', inplace = True)

we also got some typos in the 'model' column so i used some regular expressions to filter them out.
      
      # dropping strange inputs
      df = df[df['model'].str.contains('[♿🔥]') == False]
      df = df[df['model'].str.contains('^[,-./]') == False]
      df = df[df['model'].str.contains('^[\W\d]+$') == False]
      df = df[df['model'].str.contains('[@%$*#%*!=]') == False]

---

### 5- Title Status, Transmission, and Fuel

| Column        | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|--------------|---------------|----------------|---------------------|
| Transmission | 3             | 2556           | 0.60%               |
| Title Status | 6             | 8242           | 1.93%               |
| Fuel         | 5             | 3013           | 0.71%               |

these columns have common characteristics :
1. low missing values percentage 
2. dominating values, as the following:
   - 79% of 'transmission' is 'automatic'
   - 97% of 'title_status' is 'clean'
   - 84% of 'fuel' is 'gas'
     

so, there won't be any bias by filling them with the mode value

      # filling title_status with mode value
      df['title_status'] = df['title_status'].fillna('clean')
      
      # filling transmission with mode value
      df['transmission'].fillna(df['transmission'].mode().iloc[0], inplace = True)
      
      # filling fuel with mode value
      df['fuel'].fillna(df['fuel'].mode().iloc[0], inplace = True)

---

### 6- Type, Cylinders, Drive, Size

| Column     | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|------------|-------------------------|--------------------------|---------------------------|
| Type       | 13                      | 92,858                   | 21.75%                    |
| Cylinders  | 8                       | 177,678                  | 41.62%                    |
| Drive      | 3                       | 130,567                  | 30.59%                    |
| Size       | 4                       | 306,361                  | 71.77%                    |


these columns are totally the opposite, they have
1. high missing values percentage
2. no obvious dominating values

since the dataset contains 426,880 rows and 29,667 models, i took two different paths:

**1. populated these missing values with mode value per each model.**

```
    # create a list for unique models
      models = df['model'].unique()

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

```  

         # filling nulls with the mode value per each 'model'
         for i in range(min(len(model_type), len(typ))):
             
             df.loc[(df['type'].isnull()) & (df['model'] == model_type[i]), 'type'] = typ[i]



**2. I used the 'bfill' method to fill them after sorting the data by manufacturer, model, and then the column itself.**

```
columns = ['cylinders', 'type', 'drive', 'size']

for column in columns:
    df.sort_values(by = ['manufacturer', 'model', column], inplace = True)
    df[column].fillna(method = 'bfill', inplace = True)
```

Example:

the missing values (NaN) will take the previous value based on each 'model' in every 'manufacurer'

| row number  | Manufacturer | Model              | Drive |
|---|--------------|--------------------|-------|
| 1 | Acura        | 2002 rsx type s    | fwd   |
| 2 | Acura        | 3.0cl               | fwd   |
| 3 | Acura        | 3.0cl               | fwd   |
| 4 | Acura        | 3.0cl               | NaN   |
| 5 | Acura        | 3.2 cl type s      | fwd   |
| 6 | Acura        | 3.2 cl type s      | NaN   |
| 7 | Acura        | 3.2 cl type s      | NaN   |
| 8 | Acura        | 3.2 cl type s      | NaN   |
| 9 | Acura        | 3.2 cl type s      | NaN   |
|10 | Acura        | 3.2 tl              | fwd   |


Then i decided to go for the second one after comparing the intial distribution with the results as it was closer to the original data.

Example:

Values' percentage in the 'Cylinders' column

|            Values             | Original Data (with missing values) | Imputation Method 1 | Imputation Method 2 |
|-------------------------|------------------------------------|----------------------|----------------------|
| 6 cylinders             | 38.0                               | 33.0                 | 36.0                 |
| 4 cylinders             | 31.0                               | 29.0                 | 35.0                 |
| 8 cylinders             | 29.0                               | 27.0                 | 26.0                 |
| 5 cylinders             | 1.0                                | -                    | 1.0                  |
| 10 cylinders            | 1.0                                | 1.0                  | 1.0                  |
| other                   | 1.0                                | 0.0                  | 1.0                  |
| 3 cylinders             | 0.0                                | 0.0                  | 1.0                  |
| 12 cylinders            | 0.0                                | 0.0                  | 0.0                  |

---


### 7- VIN & paint_color
1. The VIN (Vehicle Identification Number) itself isn't important, but its' existence, so i converted it to a binary column
   1. 1 for existing
   2. 0 for missing
```
# convert VIN to binary

# replacing exsisting VINs with 1
df.loc[df['VIN'].notnull(), 'VIN'] = 1

# filling nulls with 0
df['VIN'].fillna(0, inplace = True)

```
2. and for the paint_color, i filled it's missing values with 'custom', as it's not a significant feature.

  ```
   df['paint_color'] = df['paint_color'].fillna('custom')

   ```
---

### 8- Lat & Long

| Column | Number Of Unique Values | Number Of Missing Values | Missing Values Percentage |
|--------|-------------------------|--------------------------|---------------------------|
| Lat    | 53,181                  | 6,549                    | 1.53%                     |
| Long   | 53,772                  | 6,549                    | 1.53%                     |

we only have some missing values here so i populated them with median value based the the region column
      
      # populating lat & long with the median value per each 'region'
      regions = df['region'].unique()
      
      # Fill missing lat and long based on median values for each region
      for region in regions:
          median_lat = df[df['region'] == region]['lat'].median()
          median_long = df[df['region'] == region]['long'].median()
          df.loc[(df['region'] == region) & (df['lat'].isnull()), 'lat'] = median_lat
          df.loc[(df['region'] == region) & (df['long'].isnull()), 'long'] = median_long

---

## Now with the pre-final results 
the new shape of the dataset is 
- 359,724 rows (almost 85% of the original shape)
- 19 columns

| Column        | Percentage of Missing Values |
|---------------|-------------------------------|
| region        | 0.0%                          |
| price         | 0.0%                          |
| year          | 0.0%                          |
| manufacturer  | 0.0%                          |
| model         | 0.0%                          |
| condition     | 36.7%                         |
| cylinders     | 0.0%                          |
| fuel          | 0.0%                          |
| odometer      | 0.0%                          |
| title_status  | 0.0%                          |
| transmission  | 0.0%                          |
| VIN           | 0.0%                          |
| drive         | 0.0%                          |
| size          | 0.0%                          |
| type          | 0.0%                          |
| paint_color   | 0.0%                          |
| state         | 0.0%                          |
| lat           | 0.0%                          |
| long          | 0.0%                          |







