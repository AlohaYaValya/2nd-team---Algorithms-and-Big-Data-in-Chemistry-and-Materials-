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

### Import the built-in json module, which allows you to work with data in JSON format.
# Filling data
insert the publication dates of the articles in the 'Date' column for all rows that meet the search conditions.
```python
import json
with open('data_filled.json', 'r') as f:
    data_to_fill = json.load(f)'   
 ```
___
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
___

# Filling in SMILES
When the PubChem API queries a connection name, it returns a JSON response and then the code extracts SMILES from that response. If there is an error in the query, a NaN value is added to the list of SMILES for the corresponding connection.

After retrieving SMILES for each connection, the code adds them to the original dataset (dataframe) and saves the updated dataframe in a new TSV format file.
After running a query in PubChem, run a query in another API, and fill in the missing lines of SMILES with the remaining lines
___
|Name of the column|Missing value|
|:-----|-----:|
|DOI|                       0.000000|
|Date|                      0.000000|
|Journal|                   0.000000|
|Title|                     0.000000|
|Name  |                    0.000000|
|measurement_error|         0.000000|
|measurement_wavelength|    0.885709|
|measurement_method     |   0.000000|
|normalised_name         |  0.411729|
|raw_value                | 0.000000|
|specifier                 |0.000000|
|dtype: | float64
___
# Fixing raw values
```python
raw_value_pattern = r'^\d+(\.\d+)?$'
```
***Percentage of values that match the pattern: 91.44%**
As the result i have a list of patterns: ['X', 'X±X', 'X []', 'X–X', 'X to X', 'X−X', 'X ± X', 'X ± X', 'X (X)', '∼X', '~X', 'X ± X', 'X-X', 'X, X, X', 'X + iX', 'X (EtOH)', 'X, X', 'X +', 'X,X ± X,X', 'X at X μm', '<X', 'X–X (this)', 'X X X X', 'X*', 'X+Xi', 'X; X; X', 'Xe']  write a code for each pattern:
___



# Fixing names
Then we use the PubCham library to get the chemical formula for the compound name
>Executes a normalised_name query and enters the required chemical formula in the Formula column

|Name of the column|Missing value|
|:-----|-----:|
|DOI|                       0.000000|
|Date|                      0.000000|
|Journal|                   0.000000|
|Title|                     0.000000|
|Name  |                    0.000000|
|measurement_error|         0.000000|
|measurement_wavelength|    0.885709|
|measurement_method     |   0.000000|
|normalised_name         |  0.411729|
|raw_value                | 0.000000|
|specifier                 |0.000000|
|Formula                |   0.311558|
|dtype: | float64
_____

# Getting descriptors
create descriptor calculator with all descriptors in PubChem RDkit Morder
___


### Cleansing the dataset
Let's drop a column named 'measurement_wavelength', cause almost 90% of it is missing. It could ne collected only from papers, and it will defenetly take too much time for us to care about it. I know it can be a very important parameter, i guess the target parameter changes a lot with the changing of this feature, but we still need to drop it, cause the prediction could be spoiled by that.
```python
df_ww = df_unique.drop(columns=['measurement_wavelength'])
```
You can notice that the percentage of missing values for columns ('complexity', 'molecular_weight', 'exact_mass' and 'tpsa') is the same. It is assumed that these are the same columns, and that in addition to these basic parameters, other parameters are missing there.
___
## Outlier detection
Outliers are data points that fall significantly outside the range of most of the other data points in a dataset. They can be caused by measurement errors, extreme values, or other factors. Outliers can skew statistical analyses, leading to incorrect conclusions

Visualizing the outliers


____
**let's find the outliers using IQR method**
>For now we have 2941 samples out of initial 5000

___
## Outliers capping
In some cases, it may be appropriate to remove outliers from the dataset. However, in many cases, it is better to cap the outliers by replacing them with the corresponding upper or lower limit, as removing them can result in loss of information and bias in the analysis.

Capping the outliers means that the data will still be retained, but any extreme values that could potentially skew the analysis will be brought back into the range of the rest of the data. This can help to ensure that statistical analyses are more accurate and reliable.



***Now our value distribution looks much better, no outliers***
____
# Data transformation
Let's explore what are the types of values we have in our columns
