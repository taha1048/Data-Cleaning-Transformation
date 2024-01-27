# Data-Cleaning-Transformation
I started by exploring data types and missing values in the dataset

## first, numeric columns ['price' , 'odometer']:

i found high skewness in them (right) due to some outliers

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/97c4892b-5c16-4278-b34b-bb331a362723)


with an overall shape like this :

![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/b44149cb-ecdd-4270-a608-3661cfb23231)




i tried to evaluate these outliers and here is what i've found

1- price ranges >> 

    Zero prices:  32,895 rows
    
    0 < prices < 100 :  3,327 rows
    
    100 <= prices < 1000 :  10,093 rows 
    
    1000 <= prices <= 10,000 :  129,922 rows
    
    10000 < prices <= 100,000 :  249,988 rows
    
    prices > 100,000:  655 rows


2- odometer ranges >>

    odometer <= 10 :  5343
    
    odometer <= 100 : 6974
    
    odometer <= 1,000 : 10928
    
    odometer <= 10,000 : 29761
    
    odometer <= 100,000 : 247141
    
    odometer <= 1000,000 :  421904
    
    odometer <= 10,000,000 : 422480


so i decided to select these ranges 

    price between(1,000 & 100,000)
    
    odometer less than 1000,000 

making the most sense out of data and also less skewness, here is the result 


![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/ba84a941-12e4-49c7-a6a4-e8682f74fa85)


## Second on the list would be Date columns ['year', 'posted_date']

by extracting the posted_year from 'posted_date'[ 2021-04-26T21:20:19-0500],

it turned out that all entries were made in 2021, but we have models entered as 2022 edition. 

            index   year	posting_year
            9738	2022.0	2021.0
            32148	2022.0	2021.0
            43183	2022.0	2021.0
            65611	2022.0	2021.0
            65612	2022.0	2021.0
            ...	...	...
            410935	2022.0	2021.0
            410936	2022.0	2021.0
            412094	2022.0	2021.0
            413805	2022.0	2021.0
            423091	2022.0	2021.0



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


