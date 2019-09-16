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
        
