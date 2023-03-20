# 2nd team - "Algorithms and Big Data in Chemistry and Materials" course 
### Team members: Valentina Bocharova and Alexander Makarov (group A4130)
## Hometask №1 : Data collecting and cleansing
The first step of the project is cleaning and collecting more data (at least 1000 descriptors). The dataframe we chose is '2.csv', then it was renamed to **'data_2.csv'**. The initial number of rows - 5000. All code is in notebook **'Homework_1.ipynb'**.
It was decided first to inspect the dataframe we got.
There are 11 columns in the table. Let's have a look at missing values distribution:
|Name of the column|Type|Missing value rate|
|:-----|:-----:|-----:|
|DOI|object|0.0000|
|Date|object|0.0842|
Journal |object|0.0842|
Title |object|0.0842|
Name |object|0.0010|
measurement_error|float64|0.0000|
measurement_wavelength |object|0.8858|
measurement_method |object|0.0000|
normalised_name |object|0.4116|
raw_value |object|0.0000|
specifier |object|0.0000|
Our mission is to handle missing values, check and clean existing values.
____
#### Now we can easily look at the distribution of missing values in the given dataset, for example: in the column 'measurement_wavelength' 88,58% of data is missing, in 'normalised_name' - 41,16%
___
## Fixing DOI column
### use DOI PATTERN
```python
'(DOI_pattern = re.compile(r'^10\.\d{4,9}\/[-._;()\/:A-Z0-9]+(?=_)'))'
```
___
## After we split the DPI, we got rid of the duplicates
check if there are any duplicates in the df
>***We got 4975 original lines***

## We are left with empty lines that need to be filled.

|Name of the column|Missing value|
|:-----|-----:|
|DOI|                       0.000000|
|Date|                      0.083467|
|Journal|                   0.083467|
|Title|                     0.083467|
|Name  |                    0.001001|
|measurement_error|         0.000000|
|measurement_wavelength|    0.885709|
|measurement_method     |   0.000000|
|normalised_name         |  0.411729|
|raw_value                | 0.000000|
|specifier                 |0.000000|
|dtype: | float64

### Here we can see, that the most missing data we have is in the columns ***'measurement_wavelength', 'normalised_name'*** Firstly let's look at the other column - 'Title'. For some reason the name of the articles were imported not as the sentances but as the continuous sequence of large letters. Also there is missing values in the ***'Date' and 'Journal'*** columns which we should also fill in. Now when we know that all the rows are unique and valid, let's collect this data from the articles with the function.
___
### which retrieves publication metadata in JSON format using DOI. In this case, we request publication metadata
>with this function we get the missing data, and output the value up to 0.000

|Name of the column|Missing value|
|:-----|-----:|
|DOI|                       0.000000|
|Date|                      0.000000|
|Journal|                   0.000000|
|Title|                     0.000000|
|Name  |                    0.001001|
|measurement_error|         0.000000|
|measurement_wavelength|    0.885709|
|measurement_method     |   0.000000|
|normalised_name         |  0.411729|
|raw_value                | 0.000000|
|specifier                 |0.000000|
|dtype: | float64
____

