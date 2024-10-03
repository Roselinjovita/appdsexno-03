# AppDSExno-03

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


**PROGRAM**
```
DEVELOPED BY: ROSELIN MARY JOVITA S
REG NO : 212222230122
```
  
```
import pandas as pd 
import numpy as np
from fuzzywuzzy import process
from sklearn.preprocessing import MinMaxScale
```
<table>
<tr>
<td>
    
```
da=pd.read_csv('emobile.csv')          
da.head()
```
</td>

<td>
  

![Screenshot 2024-10-03 223508](https://github.com/user-attachments/assets/46241842-ead4-4768-b7fc-54fed238567b)
</td>
</tr>
</table>

<table>
<tr>
<td>
  

```
db=pd.read_csv('maxmobile.csv')          
db.head()
```

</td>

<td>

![Screenshot 2024-10-03 223517](https://github.com/user-attachments/assets/3b15ab6f-4483-4150-a59a-faf48b8fbf2a)

</td>
</tr>
</table>

```
da.drop_duplicates(inplace=True)
db.drop_duplicates(inplace=True)
da.columns=['Product_ID','Product_Name','Rating','Review_Count']
db.columns=['Product_ID','Product_Name','Rating','Review_Count']
```

```
def matching(name,choice,l=1):
    results=process.extract(name,choice,limit=l)
    return results[0][0] if results else None
```

```
db['Matched_Product'] = db['Product_Name'].apply(lambda x: matching(x, da['Product_Name'].tolist()))
db.head()
```
![Screenshot 2024-10-03 224318](https://github.com/user-attachments/assets/32229071-4fe2-4d90-a4c0-13bb00f1276e)

```
merged=pd.merge(da,db,left_on='Product_Name',right_on='Matched_Product',
                how='inner',suffixes=('_ds1','_ds2'))
merged.head()

```
![Screenshot 2024-10-03 224530](https://github.com/user-attachments/assets/da5280e0-99d7-49b4-8076-d2104de5cec1)


```
merged.columns
x=merged['Review_Count_ds1']+merged['Review_Count_ds2']
merged['Combined_Rating']=(merged['Rating_ds1']*merged['Review_Count_ds1']+
                           merged['Rating_ds2']*merged['Review_Count_ds2'])/x
merged.head()
```

![Screenshot 2024-10-03 224631](https://github.com/user-attachments/assets/9e624bda-45e4-45d9-b856-451b903fd233)


```
merged['Recommendation_Score']=0.7*merged['Combined_Rating']+0.3*merged['Rating_ds1']
merged.head()
```

![Screenshot 2024-10-03 224809](https://github.com/user-attachments/assets/636ed2b5-a33b-4add-9f53-9bffd385c657)

```
best=merged.sort_values (by='Recommendation_Score', ascending=False)
print(best[['Matched_Product', 'Combined_Rating', 'Recommendation_Score']].head(5)
```
![Screenshot 2024-10-03 224922](https://github.com/user-attachments/assets/4b30d9a9-a190-4490-b170-b2de6e3eba7c)


```
best[['Matched_Product','Combined_Rating',
      'Recommendation_Score']].to_csv('BestRecommended.csv',index=False)
new=pd.read_csv('BestRecommended.csv')
new.head()
```



![Screenshot 2024-10-03 225011](https://github.com/user-attachments/assets/052f6a4e-c8ca-470f-a180-bf257a145811)





**RESULT**
Thus, the Implementation of the Recommendation System for the given dataset is completed successfully.
