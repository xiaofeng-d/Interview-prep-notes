Pandas

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
sr = pd.Series(dict)

#create from Python list
list = ['g','e','e','k','s']
se = pd.Series(list)

```

2.Series Operations

```python
# Scalar Arithmetic
avg = 0.5
sr - 0.5 

# numpy functions
np.sign(sr)

# map function: ELEMENT-WISE OPERATIONS
def threshold(x):
		if x > 0.5:
			return x
    else:
      return 0.5
sr.map(threshold)

sr.map(lambda x: if x > 0.5 else 0.5)
```

