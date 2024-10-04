# Exno-03 Recommendation Systems

**AIM:**

To Implement Recommendation Systems using the suitable data sets.

**ALGORITHM:**

STEP 1: Load the necessary Datasets.

STEP 2: Include the necessary python library.

STEP 3: Use Fuzzy library for handling text data.

STEP 4: Perform Data Preprocessing Steps.

STEP 5: Standardize column names for merging.

STEP 6: Apply fuzzy matching to find similar text data between datasets.

STEP 7: Perform Data transformation between datasets.

STEP 8: Define a recommendation score using the features of the datasets.

STEP 9: Sort the data by recommendation score.

STEP 10: Export the results to a CSV file.


**CODING & OUTPUT**
```python
import pandas as pd
import numpy as np
!pip install pandas numpy scikit-learn fuzzywuzzy python-Levenshtein

from fuzzywuzzy import process
from sklearn.preprocessing import MinMaxScaler
df = pd.read_csv('emobile.csv')
df.head()
```
![image](https://github.com/user-attachments/assets/19c41877-332a-47ed-8b89-28e5dc5d280d)

```python
dx = pd.read_csv('maxmobile.csv')
dx.drop_duplicates(inplace=True)
df.drop_duplicates(inplace=True)
df.columns =['Product_id','Product_Name','Rating','Review_Count_ds1']
dx.columns=['Product_id','Product_Name','Rating','Reivew_Count_ds2']
def match_products(name,choices,limit=1):
    results = process.extract(name,choices,limit=limit)
    return results[0][0] if results else None
dx['Match_Product']= dx['Product_Name'].apply(lambda x:match_products(x,df['Product_Name'].tolist()))
merged_df = pd.merge(df,dx,left_on='Product_Name',right_on='Match_Product',how='inner',suffixes=('_ds1','_ds2'))
merged_df
```
![image](https://github.com/user-attachments/assets/8146ba6d-4dcc-4a16-89f5-5d6e99fc8906)
```python
x=merged_df['Review_Count_ds1']+merged_df['Reivew_Count_ds2']
merged_df['Combined_Rating']= (merged_df['Rating_ds1']*merged_df['Review_Count_ds1']+merged_df['Rating_ds2']*merged_df['Reivew_Count_ds2'])/x
merged_df['Recomendaation_Score']=0.7*merged_df['Combined_Rating']+0.3*merged_df['Rating_ds1']
merged_df.head()
```
![image](https://github.com/user-attachments/assets/6ad6c2fb-9dc5-4803-a89f-58ccae02bf2d)
```python
best_products = merged_df.sort_values(by='Recomendaation_Score',ascending=False)
best_products.columns
print(best_products[['Match_Product','Combined_Rating','Recomendaation_Score']])
```
![image](https://github.com/user-attachments/assets/1d0ae62d-421f-4b9e-9bf7-1d23f6f440bd)




**RESULT**


The above experiment excuted successfull
