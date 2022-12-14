# Data-Analytics-Tool-Analytics

## Youtube Dataset Analytics

We will be perform the following operations on the youtube dataset to learn more about sql and python connection:
- Step 1 - Read the youtube dataset and create a dataframe
- Step 2 - Create a  function to calculate the distributuion of channel type from the top 1000 records
- Step 3 - Insert Top 1000 records inserted into a CSV file
- Step 4 - Read the Top 1000 records file created in Step 4
- Step 4 - Create a Sql connection and perform the following steps:
    - Create a new database
    - Create a new table and import the csv file generated in Step 2 into it
    - Read data from the table created in previous step and display it


### Step 1 - Read the youtube dataset and create a dataframe
```python
import pandas as pd
import numpy as np

data=pd.read_csv(r"Enter path to youtube_dataset.csv", encoding='cp1252')
# Display the top 5 records of the dataframe
data.head()  
```


### Step 2 - Function to calculate the distributuion of channel type from the top 1000 records
```python
def subset_data_channeltype_distribution(df, no_of_rows):
  subset = df.head(no_of_rows)
  return subset.groupby('channeltype')['channeltype'].count()

#Calling a function and passing number of rows as a variable to make the function dynamic
subset_data_channeltype_distribution(data,1000)
```

### Step 3 - Insert Top 1000 records inserted into a CSV file
```python
subset_data_frame = data.head(1000)

# Change the file name with the name you want
subset_data_frame.to_csv("youtube_dataset_top_1000_rows.csv", index=False) 
```

### Step 4 - Read the Top 1000 records file created in Step 4
```python
# Enter path to your file created in previous step
first_1k_data = pd.read_csv(r"Enter path to the file youtube_dataset_top_1000_rows.csv saved in above step") 
```

### Step 5 - Create a Sql connection and perform the following steps:

### #Create a database connection
```python
from sqlalchemy import create_engine
# Replace username, password and hostname with your database username, password and host
sqlEngine = create_engine('mysql+pymysql://username:password@hostname') 
dbConnection = sqlEngine.connect()
```
- ### Create a new database
```python
with sqlEngine.connect() as conn:
    conn.execute("commit")
    conn.execute(f"CREATE DATABASE database_name")  # Replace database_name with the name with which you want to create database
```
- ### Create a new table and import the csv file generated in Step 2 into it
```python
# Replace table_name and database_name with the name of  table and the database you want to create
first_1k_data.to_sql('table_name',con=sqlEngine, schema = 'database_name',index=False,if_exists='append')
```
- ### Read data from the table created in previous step and display it
```python
# Replace table_name and database_name with the name of  table and the database you want to create
frame = pd.read_sql("select * from database_name.table_name", dbConnection); 
pd.set_option('display.expand_frame_repr', False)
```




    