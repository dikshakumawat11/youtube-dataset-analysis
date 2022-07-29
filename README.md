# Youtube dataset analysis

- ### Use the read_csv function to read the youtube dataset
```python
import pandas as pd
import numpy as np

data=pd.read_csv(r"Path to youtube_dataset.csv", encoding='cp1252')
data.head()  
```
- ### Calculate the distribution of channel type from the top 1000 records
```python
def subset_data_channeltype_distribution(df, no_of_rows):
  subset = df.head(no_of_rows)
  return subset.groupby('channeltype')['channeltype'].count()

#Function Call
subset_data_channeltype_distribution(data,1000)
```

- ### Insert Top 1000 records inserted into a CSV file
```python
subset_data_frame = data.head(1000)

# A file named as "youtube_subset" will be exported to the same directory. 
# Index set to false means removing the index column of a dataframe while exporting the file
subset_data_frame.to_csv("youtube_subset.csv", index=False) 
```

- ### Read the CSV file created in previous Step
```python
# Substitute the text inside the read_csv with the path of the file created in the above step- hiding the sensitive information as mentioned in the requirements
first_1k_data = pd.read_csv(r"Path to the file youtube_subset.csv saved in above step") 
```

- ### SQL Database connection
```python
from sqlalchemy import create_engine
# Substitute user_name, username_password and host_name as your's mysql connection
sqlEngine = create_engine('mysql+pymysql://user_name:username_password@host_name') 
dbConnection = sqlEngine.connect()
```
- ### Creating New Database
```python
with sqlEngine.connect() as conn:
    conn.execute("commit")
    conn.execute(f"CREATE DATABASE databaseName")  # Substitute database name with the one you wish to use
```
- ### Creating a table within the database created above and importing data
```python
# Substitute tableName and databaseName with the table name you want to create and use the same database name as used in the create database command
first_1k_data.to_sql('tableName',con=sqlEngine, schema = 'databaseName',index=False,if_exists='append')
```
- ### Reading data inserted in previous step to a dataframae
```python
# Substitute tableName and databaseName with the one's created in above steps
frame = pd.read_sql("select * from databaseName.tableName", dbConnection); 
pd.set_option('display.expand_frame_repr', False)
