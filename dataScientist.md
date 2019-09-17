- Data Pre-processing Wrangling
  - basic data handling with data selection
  
        import numpy as np
        import pandas as pd
        
        data = pd.read_csv("filename")
        data.head(6)
        
        #select one column
        data['Column name 1']
        # select multiple columns
        df = data[['Column1', 'Column2']]
        # select rows
        data[3:10]
        
        # conditional selection
        data[data['Column1']<500]
  - drop column and row
        
        # check type of data
        type(data)
        # convert to dataframe
        df = pd.DataFrame(data)
        # remove first row
        df.drop(df.index[0], inplace=True)
        # remove first two rows
        df.drop(df.index[:2], inplace=True)
        # drop column by name
        df.drop(['Column1'], axis=1, inplace=True)
  - subsetting and indexing
        
        # select all entries from index 2 onwards
        data[2:]
        # select entries at even index locations
        data[::2]
        # specify the ranges of rows and columns
        data.iloc[0:8, 0:7]
        # select all columns for rows of index values 0 and 10
        data.loc[[0,10], :]
        # isolate rows and columns
        data[['Column1', 'Column2']][4:7]
        # isolate by row names
        data[data.Countries == "Italy"]
        # isolate by two row labels
        data.loc[data['Countries'].isin(['Italy', 'Germany'])]
        # select rows with multiple conditions
        data[(data.Countries == 'India') & (data.Degree == 'Vulnerable')]
        # change column name
        df = data[:] # copy data
        df.rename(columns={'old name' : 'new name'}, inplace = True)
        
        
