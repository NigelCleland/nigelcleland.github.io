---
layout: post
title: "Tutorial Series: Merging Series with Pandas"
date: 2013-05-18 19:03
comments: true
categories: [python, pandas, merging]
---

This post will cover some quick usage surrounding merging a Series and a DataFrame or merging two Series together. This is functionality which I've often felt is lacking and is boiler plate I've had to write a number of times. I've added the functionality to my new repo [pdtools](github.com/NigelCleland/pdtools) and should hopefully simplify life for other people.

This post will be a simple explanation as to how it works as well as a brief explanation. Broadly, as some of my work I had to work with extremely large datasets, tens of millions of rows, dozens of columns the full works. I had to merge aspects of these datasets together, often from disparate data sources. one element which was particularly frustrating for me was having large numbers of duplicate columns, and irrelevant information when all I wanted was a single series.

<!-- more -->

So, how do we get around this issue. First a brief overview of merging DataFrames together with Pandas. Of course the pandas documentation covers this in far more depth but in general the code we are interested in is as follows (for index merging)

``` python
import pandas as pd
import numpy as np

# Create DataFrames
df = pd.DataFrame(np.random.randn((10,4)), columns=["A","B","C","D"])
df1 = df[["A", "B"]]
df2 = df[["C", "D"]]

# Merging Method One

df3 = pd.merge(df1, df2, how='inner', left_index=True, right_index=True)
df3 == df # Should return True

# Method Two

df4 = df1.merge(df2, left_index=True, right_index=True)
df4 == df # Should return True

# Method Three
df5 = df2.merge(df1, left_index=True, right_index=True)
df5 == df # Will return False as columns are different
df5 = df5[["A", "B", "C", "D"]] # Reorder the columns
df5 == df # Should return True
```

Broadly, we can either merge two DataFrames using the general pandas merge method, or the specific DataFrame method. Now what if we want to merge a series into a DataFrame, for ecxample.

``` python
s1 = df["C"]
s2 = df["D"]

# Pseudo code
df6 = df1.merge(s1).merge(s2) # Fails miserably
```

Broadly, pandas just does not like doing this. However, we can get around this by creating a single column DataFrame from the Series and merging that as follows.

``` python

# As a single statement, note this is not the best way to do this.
# I'm also taking advantage of the fact that the series has a name which
# Will be passed as the new column name.
df6 = df1.merge(pd.DataFrame({s1.name : s1}), left_index=True, right_index=True).merge(pd.DataFrame({s2.name : s2}), left_index=True, right_index=True)
```

Wow, that is ugly. Wouldn't it be simpler if we could just merge the two together. Now, what we can do is create a bit of a wrapper and syntactic sugar around our ugly code above. Let's wrap it in a function to begin with.

``` python
def merge_dataframe_series(df, s, **kwds):
	return df.merge(pd.DataFrame({s.name: s}), **kwds)


df7a = merge_dataframe_series(df1, s1, left_index=True, right_index=True)
df7 = merge_dataframe_series(df7a, s2, left_index=True, right_index=True)

df7 == df # Returns True
```

Hmmm, slightly, better but still pretty sub par over all. Lets cover the things which are wrong with the first attempt.

1. Pretty unwieldy syntax, have to call it with a DataFrame and Series.
2. Still need a lot of key word arguments to specify how the merge will occur.
3. The name is very long, want it much shorter and sweeter.
4. Ideally we want to be able to call this directly from a DataFrame.

So, let's try again.

``` python
def merge_series(self, series, **kwds):
	return self.merge(pd.DataFrame({series.name: series}), right_index=True, **kwds)

pd.DataFrame.merge_series = merge_series

df8 = df1.merge_series(s1, left_index=True).merge_series(s2, left_index=True)

df8 == df # Should be True
```

Now that is much better. The usage is much simpler, we don't need to specify what DataFrame since it is now a method of the DataFrame class. Since we're always going to be using the index of the Series we can automatically incorporate the ```right_index=True``` directly into the function. We're still using the **kwds to pass what argument we want for the merge function. Thus we don't need to use the index on the DataFrame, we could use a column using ```left_on="column"``` as well.

Let's also do the same for a series

``` python
# Use series_merge instead of merge_series to avoid name space error.
# We'll map it to merge_series to maintain the consistent api usage though.
def series_merge(self, other, **kwds):
	return pd.DataFrame({self.name: self}).merge(pd.DataFrame({other.name : other}), left_index=True, right_index=True)

pd.Series.merge_series = series_merge

df9 = s1.merge_series(s2)
df10 = df1.merge(df9, left_index=True, right_index=True)
df10 == df # Should be True
```

Now, since we can only merge series based upon their indices. I'm happy to be corrected on this but it is difficult to think of a different use case we can automatically assign the ```left_index=True``` and ```right_index=True``` key word arguments to simplify it.

Now, what we've accomplished in this blog post is a couple quick helper functions to make merging Series together, and into DataFrames much much simpler. These are automatically incorporated into the pdtools package. To apply this functionality you can download the package and incorporate it.

``` python
import pandas as pd
import pdtools.merging # Just import the two merge functions

# Or
import pdtools # Will import masks as well.
```

If using the general purpose pdtools import the masks functionality will also be added to the pandas classes. A brief overview of this functionality can be found [here](http://nigelcleland.github.io/blog/2013/05/12/selecting-data-with-pandas/).

Thoughts/Questions/etc, open an issue on [github](https://github.com/nigelcleland/pdtools/issues)