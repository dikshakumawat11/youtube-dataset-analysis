# Youtube dataset analysis

- ### Read the youtube dataset
```python
import pandas as pd
import numpy as np

data=pd.read_csv(r" Path to youtube_dataset.csv", encoding='cp1252')
data.head()  
```
- ### Function to calculate the distributuion of channel type from the top 1000 records
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

# A file named as "youtube_subset" will be exported to the same directory. Index set to false means removing the index column of a dataframe while exporting the file
subset_data_frame.to_csv("youtube_subset.csv", index=False) 
```

- ### Read the Top 1000 records file created in Step 4
```python
# Replace the text inside the read_csv with the path of the file created in the above step
first_1k_data = pd.read_csv(r"Path to the file youtube_subset.csv saved in above step") 
```

- ### Create a Sql connection and perform the following steps:

    - ### #Create a database connection
        ```python
        from sqlalchemy import create_engine
        # Replace user_name, database_password and host_name as your's mysql connection
        sqlEngine = create_engine('mysql+pymysql://user_name:database_password@host_name') 
        dbConnection = sqlEngine.connect()
        ```
        - ### Create a new database
        ```python
        with sqlEngine.connect() as conn:
            conn.execute("commit")
            conn.execute(f"CREATE DATABASE database_name")  # Replace database_name with the actual database name you need to create
        ```
        - ### Create a new table and import the csv file generated in Step 2 into it
        ```python
        # Replace table_name and database_name with the table name you want to create and use the same database name as used in the create statement
        first_1k_data.to_sql('table_name',con=sqlEngine, schema = 'database_name',index=False,if_exists='append')
        ```
        - ### Read data from the table created in previous step and display it
        ```python
        # Replace table_name and database_name with the table name and database name used in the steps above
        frame = pd.read_sql("select * from database_name.table_name", dbConnection); 
        pd.set_option('display.expand_frame_repr', False)