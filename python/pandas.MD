# Pandas tutorial

## Setup

1. Install Continuum's Anaconda to get the best of the python packages [https://www.continuum.io/downloads ]
2. Now you got the pandas with anaconda distribution, your test program should better start with these lines:
	
	```python
		import numpy as np

		from pandas import Series, DataFrame
		import pandas as pd
	```
3. Our simple Tutorial focuses on 3 main components of Pandas package
	- Series Objects
	- DataFrame Objects
	- Index Objects
	- and we'll also discuss Miscellaneous Pandas functions

## Series
### What?
- Series is an array like list of homogenous items
- Every item in a series is automatically indexed or can have a custom index
- Since the items are homogenous, the series object has a common data-type
- Visualize the series like this. You use index to access the data

Auto index:

|**Data**|3|44|78|7|167|
|--------|---|---|---|---|---|
|**Index**|0|1|2|3|4|

Custom index:

|**Data**|3113.45|4456.32|6230|8978.23|12000|
|--------|---|---|---|---|---|
|**Index**|salary-1991|salary-2001|salary-2011|salary-2021|salary-if-alive|


Custom index with "hetergenous" data:

|**Data**|31|4456.23|'hello'|7|'world'|
|--------|---|---|---|---|---|
|**Index**|age|salary|firstname|someint|lastname|

### General Operations
- **Create a Series** object from lists, tuples or dicts
- **Selection in Series** Access, Index or Slice the series
- **Modify the Series**
	- Once series created, you can mutate series objects (add, modify or remove elements)
	- Be careful not to tamper the Series object when it is not intended for
	- If you want to change the Series object, use `inplace=True` operation. Most functions have it.

### Creating Series Object
#### Notes
1. Series object can be created from ***lists, tuples, dicts, numpy arrays*** or other array like objects
2. Every Series has a name. ***name*** property defines the name of the Series object (We use it hardly)
3. Every Series have indexes. ***index*** property refers to the list of the indexes
4. Every Series have values (obviously). ***values*** property refers to the list of the values
5. Since Series is homogenous, it will have a single datatype for all the elements. ***dtype*** gives you the datatype of the elements.

#### Code
Read the code and its comments line-by-line and understand what it does.
Execute in a python shell and check the output


	
```python
	
	import numpy as np
	
	from pandas import Series, DataFrame
	import pandas as pd
	
	# From list
	list_series = Series([3,4,5,99,55])
	print(list_series)			#Prints the series, left side column are indexes, right side column are values

	#Name of the list
	print(list_series.name)			#Nothing gets printed out. Since we never assigned something
	list_series.name = 'Series from List'
	print(list_series.name)			#Now the name gets printed out

	#Data-type of the homogenous Series
	print(list_series.dtype)		#dtype property gives the datatype of Series Object

	#Index of the Series elements
	print(list_series.index)		#index property gives the index
	print(list_series.keys())		#keys() function also gives the indexes
	print(type(list_series.keys()))	#prints the type of the index object

	#Values of the Series elements
	print(list_series.values)		#values property will have all the values of elements
	print(type(list_series.values))	#prints the type of the values object
```

#### Output
```shell
	>>> print(list_series.name)
	None

	>>> print(list_series)
	0     3
	1     4
	2     5
	3    99
	4    55
	dtype: int64

	>>> list_series.name = 'Series from List'
	>>> print(list_series.name)
	Series from List

	>>> print(list_series.dtype)
	int64

	>>> print(list_series.index)
	RangeIndex(start=0, stop=5, step=1)

	>>> print(list_series.keys())
	RangeIndex(start=0, stop=5, step=1)

	>>> print(type(list_series.keys()))
	<class 'pandas.indexes.range.RangeIndex'>

	>>> print(list_series.values)
	[ 3  4  5 99 55]

	>>> print(type(list_series.values))
	<class 'numpy.ndarray'>
```

#### Other Examples
Test out the below different types of Series constructors and see how they behave

1. Series from a tuple
	```python
		
		#Series from a tuple	
		tuple_series = Series((4,55,'hello',23))	#Observe the string in the series
		
		#Wait, is this homogenous?
		print(tuple_series.dtype)					#Yes, of-course. dtype of each element is 'object'

	```
2. Series from a dict
	```python
		
		#Series from a dict	
		dict_series = Series( {'age':21, 'salary':5000, 'name':'world'} )
		#Let's print and see how it looks like
		print(dict_series)		#Observe the keys are in sorted order

		#Oh Wait, what happened to the indexes. Indexes are now strings.
		#Let's confirm
		print(dict_series.index)	

	```
3. Complete Series Constructor - Summary
	```python
		
		#List Series with all options
		complist_series = Series([3,77,23,4],index=['a','z','e','m'],dtype='object',name='BestList')
		print(complist_series)

		#Dict Series with all options
		compdict_series = Series({'a':3,'z':77,'e':23,'m':4},dtype='float64',name='BestDict')
		print(compdict_series)
		
		#1. Series can be constructed with any array like structure
		#2. Series can accept a custom-index
		#3. Series can accept a custom data-type (dtype)
		#4. Series can have a name
		#5. In List Series, the order of the Series is per the order of the list, irrespective of the key
		#6. In Dict Series, the order of the Series is sorted by key

	```

### Data access in Series

#### Simple accessing and Slicing
- Accessing ***an element*** in the series ***returns an element***
- Accessing ***range of elements*** in the series ***return a Series***

```shell
	>>> test_series = Series({'a':3,'z':77,'e':23,'m':4},dtype='float64',name='Test Series')
	>>> test_series
	a     3.0
	e    23.0
	m     4.0
	z    77.0
	Name: Test Series, dtype: float64
```

##### Access an element


1. Access by integer index
	```shell
	#Access an element through integer index - List-like Access
	>>> test_series[2]
	4.0
	```

2. Access by key index
	```shell
	#Access an element through key location index - Dict-like Access
	>>> test_series['m']
	4.0
	```


##### Slice a range which returns a series

1. Slice by integer index
	```shell
	
	#Range of elements from 0 till 1 not including 1
	>>> test_series[0:1]
	a    3.0
	Name: Test Series, dtype: float64

	#Range of elements from 0 till 2 not including 2
	>>> test_series[:2]
	a     3.0
	e    23.0
	Name: Test Series, dtype: float64
	
	#Slice all elements from start to end
	>>> test_series[:]
	a     3.0
	e    23.0
	m     4.0
	z    77.0
	Name: Test Series, dtype: float64
	
	#Slice all elements from start till end-1
	>>> test_series[:-1]
	a     3.0
	e    23.0
	m     4.0
	Name: Test Series, dtype: float64
	
	#Slice specific elements indexed at location 0 & 3
	>>> test_series[[0,3]]
	a     3.0
	z    77.0
	Name: Test Series, dtype: float64
	```
2. Slice by key index	
	```shell
	
	#Slice from key 'e' till end
	>>> test_series['e':]
	e    23.0
	m     4.0
	z    77.0
	Name: Test Series, dtype: float64
	
	#Slice from key 'a' till 'm' including 'm'
	>>> test_series['a':'m']
	a     3.0
	e    23.0
	m     4.0
	Name: Test Series, dtype: float64
	
	#Slice from start till key 'e'
	>>> test_series[:'e']
	a     3.0
	e    23.0
	Name: Test Series, dtype: float64

	#Slice selected elements 'z' and 'a' - See that slicing request order is maintained
	>>> test_series[['z','a']]
	z    77.0
	a     3.0
	Name: Test Series, dtype: float64
```

#### Accessing through index access methods (Preferred)

There are 3 main index access methods to index the data. This may seem irrelevant now, but they can be very useful while dealing Pandas DataFrames (2-dimensional data structure)

- .ix[] to use a key-based index or integer-based index
- .loc[] to use a pure location (key) based index
- .iloc[] to use a pure integer location based index

Consider the following Series
```shell
	>>> sample_series=Series({'a':32,'e':11.5,3:7,'z':99})
	>>> sample_series
	z    99.0
	a    32.0
	3     7.0
	e    11.5
	dtype: float64
```

##### Using ix[]

Using regular indexing we're not able to locate the integer location 3 as there's a key label 3
```shell
	>>> sample_series[0]
	99.0
	>>> sample_series[1]
	32.0
	>>> sample_series[2]
	7.0
	>>> sample_series[3]
	7.0
```

Using ix[ ], this is solved, see the sample_series.ix[3]
```shell
	>>> sample_series.ix[0]
	99.0
	>>> sample_series.ix[1]
	32.0
	>>> sample_series.ix[2]
	7.0
	>>> sample_series.ix[3]
	11.5
```


Contrary see what happens when we go label based, using regular indexing
```shell
	>>> sample_series['z']
	99.0
	>>> sample_series['a']
	32.0
	>>> sample_series[3]
	7.0
	>>> sample_series['e']
	11.5
```

Using ix[ ], now there is a problem
```shell
	>>> sample_series.ix['z']
	99.0
	>>> sample_series.ix['a']
	32.0
	>>> sample_series.ix[3]
	11.5
	>>> sample_series.ix['e']
	11.5
```
**Note:** ix [ ] will be extremely useful in hierarchical indexing (or multi-indexing) where we need mixed label and integer index access

##### Using loc[ ] and iloc[ ]
loc[ ] will always access the data based on key-labels. Whereas,
iloc[ ] will always access the data based on integer locations.

We use this when we know if we access indexes using either labels or integer locations **but not mixed**

```shell
	>>> sample_series.loc[3]
	7.0
	>>> sample_series.iloc[3]
	11.5
```

##### Sequential scanning using .iteritems() method
We can lazily iterate over all the elements of series using iteritems() function.
This is quite useful for sequential scans and we wan't to access indexes and corresponding elements

```shell
	>>> for elem in sample_series.iteritems(): print(elem)
	...
	('z', 99.0)
	('a', 32.0)
	(3, 7.0)
	('e', 11.5)
```

### Data Manipulation or Transformation in Series