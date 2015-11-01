---
title: Plotting data in PySpark
updated: 2015-11-01
---

For blog

PySpark doesn't have any plotting functionality (yet). If you want to plot something, you can bring the data out of the Spark Context and into your "local" Python session, where you can deal with it using any of Python's many plotting libraries.

Note that if you're on a cluster:

* By "local," I'm referring to the Spark master node - so any data will need to fit in memory there. (Sample if you need to, I guess)
* You'll need to have whatever Python libraries you need installed on the Spark master node

Here's two examples. If you have a Spark DataFrame, the easiest thing is to convert it to a Pandas DataFrame (which is local) and then plot from there. Pandas has some [very convenient shortcuts](http://pandas.pydata.org/pandas-docs/version/0.15.0/visualization.html#visualization-scatter-matrix).

```python
# create a spark dataframe with 3 numeric columns and one categorical (colour)
A = [random.normalvariate(0,1) for i in range(100)]
B = [random.normalvariate(1,2) for i in range(100)]
C = [random.normalvariate(-1,0.5) for i in range(100)]
col = [random.choice(['#e41a1c', '#377eb8','#4eae4b']) for i in range(100)]

df = sqlCtx.createDataFrame(zip(A,B,C,col), ["A","B","C","col"])

# convert to pandas and plot
pdf = df.toPandas()

from pandas.tools.plotting import scatter_matrix
stuff = scatter_matrix(pdf, alpha=0.7, figsize=(6, 6), diagonal='kde', color=pdf.col)
```
![]({{ site.url }}/assets/2015-11-01-scatter.png)


If you have only a Spark RDD then we can still take the data local - into, for example, a vector - and plot with, say, Matplotlib.


```python
import random

# create an RDD of 100 random numbers
x = [random.normalvariate(0,1) for i in range(100)]
rdd = sc.parallelize(x)

# plot data in RDD - use .collect() to bring data to local
num_bins = 50
n, bins, patches = plt.hist(rdd.collect(), num_bins, normed=1, facecolor='green', alpha=0.5)
```

![]({{ site.baseurl}}/assets/2015-11-01-histogram.png)
