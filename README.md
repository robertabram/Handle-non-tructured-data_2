# Handle-non-tructured-data_2

import pandas as pd
import numpy as np

df = pd.read_csv('Badly-structured.csv')
df.drop(1,inplace=True)

for n in range(len(df.columns.values)):
    if 'Unnamed' in df.columns.values[n]:
        df.columns.values[n] = ''

df.columns.values[1] = ''
df.columns = df.iloc[0]
df.drop(0,inplace=True,axis=0)
df.drop(824,inplace=True,axis=0)
df.columns.values[0] = 'Order ID'
df.columns.values[1] = 'Order Date'

df.set_index(['Order ID','Order Date'],inplace=True)
ship_mode = ['First Class','First Class','First Class','Same Day','Same Day','Same Day',
            'Second Class','Second Class','Second Class','Standard Class','Standard Class','Standard Class']
df.columns = pd.MultiIndex.from_arrays([ship_mode,df.columns])
#df.reset_index(inplace=True)

df = df.stack()
df = df.reset_index()
df = df.rename(columns={0:'Segment'})
cols = ['Order ID','Order Date','Segment']
df = df.set_index(cols)
df = df.stack()
df = df.reset_index()
df = df.rename(columns={'level_3':'Ship Mode',0:'Sales'})
df = df[['Ship Mode','Segment','Order ID','Order Date','Sales']]
df.sort_values(by=['Ship Mode','Segment'],inplace=True,ignore_index=True)
df.to_csv('Nice_Structured.csv',index=False)
