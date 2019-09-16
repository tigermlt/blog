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
