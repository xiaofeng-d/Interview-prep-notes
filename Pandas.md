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



