---
title: Data munging cheat sheet
updated: 2015-11-03
---

<head>
<style>
table {
  margin-left: -140px;
  position: relative;
  table-layout: auto;

  thead {
    background: #f2f2f2;
  }
  th {
    text-align: left;
    padding: 8px 10px;
    border-bottom: 15px solid #fff;
  }
  td {
    padding-left: 5px;
    padding-right: 5px;
  }
  tr:nth-child(even) {background: #f2f2f2;}
}
</style>
</head>



## Basic data munging operations: structured data

_This page is developing_



|  | Python pandas | PySpark RDD | PySpark DF | R dplyr | Revo R dplyrXdf |
| ---- |:-----------:|:-----------:|:----------:|:-------:|:--------------:|
|subset columns|`df.colname`, `df['colname']`|`rdd.map()`|`df.select('col1', 'col2', ...)`|`select(df, col1, col2, ...)`||
|new columns|`df['newcolumn']=...`|`rdd.map(function)`|`df.withColumn(“newcol”, content)`|`mutate(df, col1=col2+col3, col4=col5^2,...)`||
|subset rows|`df[1:10]`, `df.loc['rowname':]`|`rdd.filter(function or boolean vector)`, `rdd.subtract()`||`filter`||
|sample rows||`rdd.sample()`||||
|order rows|||`df.sort('col1')`|`arrange`||
|group & aggregate|`df.sum(axis=0)`, `df.groupby(['A', 'B']).agg([np.mean, np.std])`|`rdd.count()`, `rdd.countByValue()`, `rdd.reduce()`, `rdd.reduceByKey()`, `rdd.aggregate()`|`df.groupBy('col1', 'col2').count().show()`|`group_by(df, var1, var2,...) %>% summarise(col=func(var3), col2=func(var4), ...)`|`rxSummary(formula, df)` <br> or <br> `group_by() %>% summarise()` |
|peek at data|`df.head()`|`rdd.take(5)`|`df.show(5)`|`first()`, `last()`||
|quick statistics|`df.describe()`||`df.describe()`|`summary()`|`rxGetVarInfo()`|
|schema or structure|||`df.printSchema()`||||

...and there's always SQL





---

### Syntax examples

#### Python pandas

* [Pandas slicing and indexing](http://pandas.pydata.org/pandas-docs/stable/indexing.html)
* [Pandas aggregation](http://pandas.pydata.org/pandas-docs/stable/groupby.html)

```python
# TBC
```

---

#### PySpark RDDs & DataFrames

__RDDs__

Transformations return pointers to new RDDs

* map, flatmap: flexible,
* reduceByKey
* filter

Actions return values

* collect
* reduce: for cumulative aggregation
* take, count


A reminder: how `lambda` functions, `map`, `reduce` and `filter` work

```python
### lambda functions are shorthand anonymous functions
map(lambda x: x * x, range(10))
# is the equivalent of:
def map_squares(nums):
    res = []
    for x in nums:
        res.append( x * x )
    return res
map_squares(range(10))

### map - apply function to each element
## in python
numbers = [1,2,3,4]

def square(x):
    return x*x

results = map(square, numbers)

## in PySpark
nums = sc.parallelize([1, 2, 3, 4])
nums.map(lambda x: x * x).collect()


### reduce = accumulate elements via function
## in python
reduce(lambda x,y: x+y, numbers)

## in PySpark
nums.reduce(lambda x,y: x+y)
# there's also reduceByKey & countByKey
rdd.reduceByKey(lambda x,y: x+y).collect()

### filter = returns new list of items where applied function is TRUE
## in Python
filter(lambda x: x>2, numbers)

## in PySpark

# another example: drop header / first row
lines = lines.filter(lambda x: x.find('x') != 0)


```

Partitions: `rdd.getNumPartitions()`, `sc.parallelize(data, 500)`, `sc.textFile('file.csv', 500)`, `rdd.repartition(500)`



__Additional functions for DataFrames__

If you want to use an RDD method on a dataframe, you can often `df.rdd.function()`.


```python


```

Miscellaneous examples of chained data munging:

```python

## examples of chaining
# filter rows
 adultDataFrame
      .select("workclass","age","education","occupation","income")
      .filter( adultDataFrame("age") > 30 )
      .show()

# group by & sort
    adultDataFrame
      .groupBy("income","occupation")
      .count()
      .sort("occupation")
      .show()

# Word count
words = sc.textFile('hamlet.txt')\
        .flatMap(lambda line: re.split('\W+', line.lower().strip()))\
        .filter(lambda x: len(x) > 2 )\
        .map(lambda word: (word, 1))\
        .reduceByKey(lambda a, b: a + b)\
        .map(lambda x: (x[1], x[0])).sortByKey(False)

```

Further resources

* [My IPyNB scrapbook](https://github.com/standarderror/Jupyter-Notebooks/blob/master/PySpark%20syntax%20notes.ipynb) of Spark notes
* Spark programming guide (latest)
* Spark programming guide (1.3)
* [Introduction to Spark](http://researchcomputing.github.io/meetup_spring_2014/python/spark.html) illustrates how python functions like map & reduce work and how they translate into Spark, plus may data munging examples in Pandas and then Spark





---

#### R dplyr

[Copy in examples](https://gist.github.com/standarderror/f7c2ae19fdbbb01b59ff#file-r-code-library-r)

The 5 verbs:

* select = subset columns
* mutate = new cols
* filter = subset rows
* arrange = reorder rows
* summarise


```r
# select


# summarise


# using pipes to chain transformations
hflights %>%
  group_by(UniqueCarrier) %>%
  filter(!is.na(ArrDelay)) %>%
  summarise(p_delay = mean(ArrDelay > 0)) %>%
  mutate(rank = rank(p_delay)) %>%
  arrange(rank)


```

Additional functions in dplyr

* first(x) - The first element of vector x.
* last(x) - The last element of vector x.
* nth(x, n) - The nth element of vector x.
* n() - The number of rows in the data.frame or group of observations that summarise() describes.
* n_distinct(x) - The number of unique values in vector x.

---

#### Revo R dplyrXdf

Notes:

* xdf = "external dataframe" or distributed one in, say, a Teradata database
* If necessary, transformations can be done using `rxDataStep(transforms=list(...))`

Manipulation with dplyrXdf can use:

* filter, select, distinct, transmute, mutate, arrange, rename,
* group_by, summarise, do
* left_join, right_join, full_join, inner_join
* these functions supported by rx: sum, n, mean, sd, var, min, max


```r
#


# summary aggregations
rxSummary(~default + balance, data = BankDS)

# pipe notation works fine
bankSmry <- group_by(bankXdf, marital, housing) %>%
  summarise(mean_age=mean(age), mean_balance=mean(balance))

```


Further resources

* [Revo R course notes](https://gist.github.com/standarderror/f7c2ae19fdbbb01b59ff#file-revo-r-training-r)
