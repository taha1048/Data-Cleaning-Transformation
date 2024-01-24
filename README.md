# Data-Cleaning-Transformation
I started by exploring data types and missing values in the dataset
first, numeric columns ['price' , 'odometer']:
i found high skewness in them (right) due to some outliers


![download](https://github.com/taha1048/Data-Cleaning-Transformation/assets/139405748/4d7f72f0-07dd-454e-a0b0-cb253985422d)





high percentage of missing values in :
['condition', 'cylinders', 'VIN', 'drive', 'type', 'paint_color']

high skewness in : 
['price', 'odometer', 'year']

dominating values in categorical columns like:
fuel(gas 85%)
title_status(clean 96%)
transmission(automatic 80%)
in addition to a low number of missing values

many outliers in the 'year' column and also 
outdated years (starting from 1900)
all the data were input ('posing_date') in 2021
some inconsistencies with the 'year' as it's after the posting year

some outliers in the 'price':
Zero prices:  32895 
0 < prices < 100 :  3327 
100 <= prices < 1000 :  10093 
1000 <= prices <= 10000 :  129922 
10000 < prices <= 100000 :  249988 
prices > 100000:  655
