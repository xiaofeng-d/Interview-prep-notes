## Pandas Series

1.Creating Series:

```python
import pandas as pd

#empty series
ser = pd.Series()

#create from numpy arr (note: index will be 0,1,2,3,4)
data = np.array(['g','e','e','k','s'])
se = pd.Series(data)
#change index
se = pd.Series(data,index=[10,11,12,13,14])

#create from Python dictionary
dict = {'A':5.0, 'B':0.2, 'C':0.5, 'D':0.25}
se = pd.Series(dict)

#create from Python list
list = ['g','e','e','k','s']
se = pd.Series(list)

```

2.Series Operations

```python
# Scalar Arithmetic
avg = 0.5
se - 0.5 

# numpy functions
np.sign(sr)

# map function: ELEMENT-WISE OPERATIONS
def threshold(x):
	if x > 0.5:
		return x
  else:
    return 0.5
se.map(threshold)

se.map(lambda x: if x > 0.5 else 0.5)
```

3.built-in functions

```python
# average
se.mean()

# min
se.min()

#standard deviation
se.std()

# summary stats
se.describe()

# ascending order rank of items (index + order #)
se.rank() 
```

4.Series alignment

```python
# adding values of corresponding indices
se1 = pd.Series({'A':1, 'B':2})
se2 = pd.Series({'A':0,  'C':3})
se3 = pd.Series({'A':0, 'B':4})
se1 + se2 # resulting in NAN values

se1.add(se2, fill_value = 0 ) #replace NAN with 0


# boolean alignment
se1 > se2  # this will end in error
se1 > se3  # element wise 
bool1 = (se1 > 0 )
bool2 = (se3 > 0)
bool1 & bool2
bool1 | bool2
bool1 & bool2[['A','B']] #first pick ['A','B'] from bool2 and form a sub-array, and then do &

#NOTE: & and | work for arrays not of the same indices; if missing indices just assmued to be FALSE
```

## Pandas DataFrame



1.DataFrame construction

```python
# construct with index array, columns array and data matrix
index = ['0811','0812','0813']
columns = ['David','Eldon','Jade','Cage']
data = [[0.5, 0.3, 0.2, 0.1],
        [0.6, 0.4, 0.5, 1.0],
        [0.1, 0.5, 2.0, 3.0]]
df = pd.DataFrame(data, index = index, columns = columns)

# construct from DICTIONARY with EQUAL LENGTH lists/ Numpy Arrays
data = {'David':[0.5,0.6,0.1],
        'Eldon':[0.6,0.4,1.0],
        'Jade':[0.2,0.5,2.0],
        'Cage':[0.1,1.0,3.0]
}
index = ['0811','0812','0813']
df = pd.DataFrame(data, index = index)

# construct from DICTIONARY of pd.Series
index = ['0811','0812','0813']
data = {}
data['David'] = pd.Series([0.5,0.6,0.1],index=index)
data['Eldon'] = pd.Series([0.6,0.4,1.0],index=index)
df = pd.DataFrame(data)

# Adding columns to pd.DataFrame (like dictionary)
df = pd.DataFrame()
index = ['0811','0812','0813']
df['David'] = pd.Series([0.5,0.6,0.1],index=index)
df['Eldon'] = pd.Series([0.6,0.4,1.0],index=index)
```

2.DataFrame retrieve values

```python
# get data
df.values  # Return 2-d array
# get columns
df.columns # Return Index object
# get rows
df.index #return Index object

# get a column, return a Series
df['David']
# to get row, need .loc, returns a Series
df.loc['0811'] # returns a row

# can do normal series indexing with those:
df['David']['0811']
df.loc['0811']['David']

# Easier to use loc method directly
df.loc['0811','David'] #similar to 2d numpy array

# Multiple rows & columns -> Subset of df
df.loc[['0811','0812'],['David','Eldon']]

# slicing with loc
df.loc['0811':'0813','David']
df.loc['0811':'0813','David':'Jade'] # returns a dataframe

# boolean logic within loc
df.loc[df['David']>0, : ]
df.loc[df['David']>0, ['David','Jade']]

# df['David']>0 returns a series boolean, specifying which rows are selected; ['David','Jade'] specifies the columns

# column boolean selects rows
# can also select columns
df.loc[:, df.loc['0811']>0]
# can also not use loc for this, easier way to write
df[df['David']>0]

# df < 0 returns a boolean dataframe
df[df<0] # returns NaN for selected False values

```

3.DataFrame : change values

```python
# set column to scalar   
df['David'] = 0
df.loc[df['David']<0,'David']=0

# set column to list, has to be SAME DIMENSION! 
df['David'] = [0.05, 0.2, 1.0]

# set column using series -> automatically aligns
jade = pd.Series({'0811':3, '0812':5})
# DOSEN't have to be same dimension; missing values -> NAN
df['Jade'] = jade

# can set subset of df as 2-d list
df.loc[['0811','0812'],['David','Jade']] = [[0.01, 0.02],[-0.02,0.03]]

# create a new column
df['NEWCOLUMN'] = [0.5,0.2,0.3]

# create a new row
df.loc['0814'] = [-0.01, 0.015, 0.03, 0.05, -0.01]
```

4.DataFrame Methods

```python
# arithmetic
base = 0.5
df - 0.5
# boolean element-wise
df > 0
# numpy universal functions: element-wise
np.abs(df)

# applymap, same as Series 'map'

def threshold(x):
  if np.abs(x) > 0.005:
    return x
  else:
    return 0
df.applymap(threshold)

# apply: apply function to each column of dataframe

def max_min(se):
  return se.max() - se.min()
max_min(df['David']) # -> get a number

# apply function to each column
df.apply(max_min) # -> will get a number for each column. end result is a series

# apply function to each row
df.apply(max_min, axis=1)

## if the function returns a SERIES (not number), then applying this to each column will end up being a dataframe
def max_min(se):
  return pd.Series([se.min(),se.max()],index=['min','max'])
df.apply(max_min) # -> get a dataframe

# df.mean() average for each column 
df.mean()
df.mean(axis=1) # average for each row/read

# use these built-in methods to make code concise and readale

df.rank(axis=1) 
df.cumsum() #cumulative sums ALONG the COLUMN

df.describe() # returns a new DATAFRAME

# built-in correlation and covariance
df.corr() # correlation of COLUMNS with each other by default

```

