## Pandas Series

### 1.Creating Series:

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

### 2.Series Operations

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

### 3.built-in functions

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

### 4.Series alignment

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



### 1.DataFrame construction

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

### 2.DataFrame retrieve values

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

### 3.DataFrame : change values

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

### 4.DataFrame Methods

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
df.corr() # correlation matrix of COLUMNS with each other by default
df.cov() # covariance matrix
ref_arr = pd.Series([0.01, 0.02, 0.03, 0.04], index=['0811', '0812', '0813'])
df.corrwith(ref_arr) # -> array of each column's correlation
```

5.DataFrame alignment

```python
# missing values will be NaN
df1 + df3

# if don't want NaN
df1.add(df3, fill_value=0)

# gives TRUE or FALSE results
df1 > df2
(df1>0) & (df2>0)

(df1>0) | (df2>0)

# Series and Dataframe operations:

se = pd.Series({'David':100, 'Jade':20, 'Eldon':50})
df1 * se
# by default it matches the index of series on columns; otherwise NaN

se2 = pd.Series({'0811':100, '0812':20, '0813':50})
df1.mul(se2, axis = 0)  # multiply by row

```

### 5.Missing Data (NaN) 的处理

```python
# numpy represents missing data by np.nan 
nan = np.nan

pd.isnull(nan) # -> True
se.isnull() # every element missing or not
se.count() # number of non-NaN values

# getting RID of missing values?
se[se.notnull()] # too complex
se.dropna() # easier way

# dataframe dropna: by default drops any row containing NaN

df.dropna() # default
df.dropna(how = 'all') # drop rows with ALL NaNs, preserve others 

se.dropna(thresh = 3) # drop rows with <3 normal values
se.dropna(axis=1, thresh = 3) # drop COLUMNS with <3 normal values



# filling missing data
se.fillna(0) # 用0填充
df.fillna({'David':0.02, 'Eldon':0.05}) #每个column用不同填充,如果没有写该column则不填充

# Pandas的built in functions会自动忽略NaN,这点和numpy不同

df.mean() # 含有NaN的column会自动忽略
df.fillna(0).sum()/df.count()  #和上面的方法等价

np.mean(df.values,axis=1) # numpy不会自动忽略NaN,所以含有NaN的会变成NaN。 axis=1 得到每行的平均，axis=0得到每列的平均值

df.rank() #也会自动忽略NaN的部分，默认是每一column给出rank

```

### 6.Reindexing Sorting

```python
# re-order existing index / adding new values
se.reindex(['Eldon','Jade','John','David'])  # 新的index会自动加入，数据NaN

# exclude entries
se.drop('David')

#Dataframe的reindexing类似,可以重排序并加入新的index
new_index = ['0815','0816','0817','0818']
df.reindex(new_index) ##
new_columns = ['David','Jade','Jack','Eldon']
df.reindex(new_index, columns = new_columns )

# Dataframe drop: 默认row, 如果drop column需要多加一个
se.sort_index() # 按照index排序series
se.sort_index(ascending = False ) # 按照descending顺序

df.sort_index()
df.sort_index(axis=1, ascending = False) #columns的名字按照descending排序
df.sort_values(['David','Jade']) #如果有多个column，可以依次按照这个排序
```

### 7.Plotting

```python
df = pd.DataFrame(index=['0811','0812','0813','0814'])
df['X'] = [1, 2, 3, 4]
df['Y'] = [2, 3, 4, 5]
df['Z'] = [0.5, 0.2, 0.4, 0.1]

df['X'].plot(title='X plot') # 画一条线
df.plot(title='DataFrame Line Plot', style='-*') # 画每一个column的一条线]

df.plot(kind='bar', title='DataFrame Line Plot', style='-*') # 画bar plot

# 画heatmap，常用于画correlation matrix
import seaborn as sns
sns.heatmap(df.corr(),annot=True)
```

### 8.Categorical Data

```python
# sector information
se = pd.Series({'AAPL':'Tech', 'XOM':'Energy', 'MSFT':'Tech', 'GS':'Financials'})
se.unique()

se.value_counts()
mask = se.isin(['Financials','Tech']) #产生一个mask数组，boolean表明每个index是否属于这个范畴或者不属于
se[mask]
pd.get_dummies(se) # 产生一个dataframe，可用作多元回归 indicator variables

```

### 9.MultiIndexing

当有好几张类似的表时，同时操纵它们不太方便。可以先存成一个dictionary，然后用 pd.concat( ) 收纳成一个multi-indexing的dataframe

```python
multi.loc['pe'] # 通过最外层的index，返回相应的dataframe
multi_se = multi['SPY'] # 如果用列直接调用列，得到的是一个 multi-index series.
multi_se['pe'] #得到一个series
multi_se.unstack(level=0) #可以把它变回去一个dataframe
multi_se.unstack(level=0).stack() #可以再revert回去成一个multi-index series


```

multi也支持 mean(), sum()等等methods，其返回的结果均较为intuitive。

## Other Pandas Topics

### Pandas File I/O:

```python
# 转换为csv

df.to_csv('df_example.csv', index_label='Date') #原来的index没有名字，给index起了一个名字
data = pd.read_csv('df_example.csv') #读入的csv文件有专门的Date列
data.set_index('Date') # 将Date列转换回index列，这样写更简便

# 转换为excel
data = pd.read_excel('df_example.xlsx')

df.to_pickle('df_exmaple.pk')
pd.read_pickle('df_example.pk')

# csv和excel方便在excel中打开，pkl格式适合在python中重新读取使用
```

### Time Series Data

```python
import pandas as pd
import numpy as np
dt = '20230107' # 也可以用不同格式，'2011-01-07'等等
dt = pd.to_datetime(dt)

dt.year
dt.month
dt.minute
dt.second

# 在一周中的第几天 Return the day of the week. It is assumed the week starts on Monday, which is denoted by 0 and ends on Sunday which is denoted by 6
dt.weekday()

dt + pd.tseries.offsets.Day()
dt + pd.tseries.offsets.BDay() # 只加business day

# ro l l forward, backward
print(pd.tseries.offsets.MonthEnd().rollforward(dt)) #下一个月底
print(pd.tseries.offsets.MonthEnd().rollback(dt)) #上一个月底

# subtract two times
dt1 = pd.to_datetime('20230107')
dt2 = pd.to_datetime('20230109')
diff = dt2 - dt1

diff.days

# 把excel的data快速变成timestamp object的方法
date = ['20230102',]

# 在date range里获得
days = pd.date_range(dt1, dt2, freq = 'D')

months = pd.date_range('20230101','20231202', freq='M')

# 'min' for minutes
# 可以用datetime series作为index建立dataframe
names = ['David','Eldon','Jade']
days = pd.date_range('20230101','20230205', freq='min')
df = pd.DataFrame(np.random.randn(len(days),len(names)),index=days, columns = names)

df.loc['2023-01-01 00:00:00']
#can pass in the day only, and retrieve that day
df.loc['2023-01-01]
       
# re-sample: 用一定的时间频率来aggregate！
df.resample('M').sum()
# 5 minute high
       df.resample('5min').max()
# close
       df.resample('5min').last
```

### Groupby

Groupby解决当数据表里的data没有按照统一的时间排序，而是按照出现的次序一个一个记录。

```python
univ = ['AAPL','MSFT','BAC','GS']
sector = {'AAPL':'Tech','MSFT':'Tech','BAC':'Fin','GS':'Fin'}
data=[]
# 制造一个未按时间排序的数据表
for dt in dates:
    for x in univ:
   data.append([x,sector[x],dt,np.random.randn(),np.random.randn()])
data = pd.DataFrame(data,columns=['ticker','sector','date','signal1','signal2'])
data

# 按照ticker计算相应的average
data.groupby('ticker').mean()
# 同时按两个group
data.groupby(['sector','date']).mean()
data.groupby(['sector','date'])[['signal1']].mean()

# 用apply引入customized function
def max_minus_min(x):
    return x.max()-x.min()
data.groupby(['sector','date'])[['signal1','signal2']].apply(max_minus_min)

# 如果apply函数返回的是dataframe，则结果有不同：
# return a dataframe instead of a series 
def demean(x):
    return x - x.mean()

data.groupby(['sector','date'])[['signal1','signal2']].apply(demean)

# 
df = data.groupby(['sector','date'])[['signal1','signal2']].mean()
df.groupby(level=0).mean()
```

### Ticker

写法1: df1 = data.set_index(['date','ticker'])['signal1'].unstack(level=1)

写法2:df2 = data.pivot_table(index='date',columns='ticker',values='signal1')
df2

写法2更为简洁



### Rolling

df.rolling().mean()计算rolling average

```python
df.rolling(252).mean() #注意到252天内的会变成NAN，为了避免，也可以写成
df.rolling(252, min_periods=1).mean() #前面天数不足的会直接求平均，而不会是NaN

```

### quantiles

​		用qcut函数，按照bin的数量划分成几个bucket：
https://pandas.pydata.org/docs/reference/api/pandas.qcut.html

```python
qcut = pd.qcut(df['David'], 10, labels=False) #划分成十个quantile

qcut.value_counts().sort_index()


```

### shift

用途1:计算return的时候，df/df.shift() -1

```

```



## Notes

1.df['product_id']

df['product_id'] 和 df[['product_id']]的区别：前面是series，后面是dataframe。多了一个括号的区别！

2.Pandas中表示Logical Not的符号是~

3.如何用or操作：

```python
import pandas as pd

#create DataFrame
df = pd.DataFrame({'team': ['A', 'A', 'B', 'B', 'B', 'B', 'C', 'C'],
                   'points': [25, 12, 15, 14, 19, 23, 25, 29],
                   'assists': [5, 7, 7, 9, 12, 9, 9, 4],
                   'rebounds': [11, 8, 10, 6, 6, 5, 9, 12]})

#view DataFrame
print(df)
```

```python
#filter rows where points > 20 or assists = 9
df[(df.points > 20) | (df.assists == 9)]
```
