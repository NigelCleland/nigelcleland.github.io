---
layout: post
title: "Selecting Data with Pandas"
date: 2013-05-12 13:12
comments: true
categories: [python, pandas, Data Analysis]
---

As part of my job I need to do a large amount of sample analysis, and visualisation of small subsets of data. This requires a large quantity of iteration, trial and error and repetitive coding. Something we all wish we could avoid, yet sometimes can't. To give a practical example, I often need to work with half hourly data, for a wide range of individual metrics which may have similarities. What I need to do, is isolate this data in various ways.

<!-- more -->

Now, here we introduce pandas. Pandas is an analysis library for Python built on top of numpy for fast merges, joins and analysis. It is an essential part of my day to day work and if you have to work with any decent amount of data I highly recommend using it. However, there is one slight issue I've had with it. Selecting small subsets of data in a simple fashion. To give an example, in python, to iterate we can use:

``` python
for item in container:
    if item in subset:
		    print item
```

Here the key is that we're looking at the entire container, and then assessing whether our item fits that subset. However, in pandas it isn't so easy.
In Pandas, to select a subset of the data we create an array of booleans and then apply this to the dataframe as follows:

``` python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.random.random(10, 4), columns=["A", "B", "C", "D"])

subset = df[(df["A"] <= 0.5) & (df["B"] >= 0.2)]
```
In this example, we are selecting a subset of our data which satisfies the condition of column A being less than or equal to 0.5 and column B greater than or equal to 0.2. (Note, the brackets around each statement are required.

However, this syntax, while clear for simple examples does start to break down at higher orders. For example, let's say we had qualitative values in our data frame and we want to select a small subset of them. We cannot use the in operator, or a multiple equal operator. Instead, we need to use the or operator.

``` python

# Note this won't work:
df[df["A"] == (0.3, 0.2, 0.7)]

# Require the following:
df[(df["A"] == 0.3) | (df["A"] == 0.2) | (df["A"] == 0.7)]
```

As you can see the amount of duplicate code requires continues to increase linearly with each additional item we wish to check. Is there a better way?
Turns out, maybe.

What I have done is develop a range of common masks and place them in a single [repository](https://github.com/NigelCleland/masks) or it may be downloaded using pip

``` 
pip install masks
```

To use it, you simply import the masks module after importing pandas and it will apply a number of additional methods to the Series and DataFrame classes in the pandas module. I've attempted to avoid all known api clashes through the addition of _masks at the end of each function, which should also make the meaning clearer. Using this module our multiple selection example becomes much simpler

``` python
import pandas as pd
import masks
import numpy as np

df = pd.DataFrame(np.arange(30).reshape(10, 3), columns=["A", "B", "C"])

# Use the in_eqmask this mask will return all values which meet the conditions
# the eqmask instead of just mask is to specify that equality conditions are
# being introduced

df.in_eqmask("A", (6, 12, 24))

# Or another simple use case which has frustrated me endlessly in pandas is
# returning all rows which satisfy a between type condition.

df[(df["A"] >= 0.5) & (df["A"] <= 0.7)]
df.bet_mask("A", (0.5, 0.7))
```

Which is much much simpler no?

An additional benefit of this is that we are able to chain methods together in order to create the desired subsets. For example.

``` python
df.ge_mask("A", 5).le_mask("C", 15).ne_mask("B", 10)
```

We just applied three different refinements to get all rows of the dataframe where A is greater than 5, C less than 15 and B not equal to 10.

There are a few more complex functions in the module, such as selecting the top, bottom or middle x% of a particular column or columns e.g.

``` python
# Take the rows which satisfy the top 25% of column A, then take the bottom 10% of column B
# from this subset.
df.top_mask("A", 25).bot_mask("B", 10)
```

masks is still a work in progress, however it is usable as is for prototyping and data exploration. There are a number of additional features which are still needed at this stage. There repo is [here](https://github.com/NigelCleland/masks) and all contributions/comments are welcome. I'm not sure this is the best way to add this functionality, however it has been useful for me in the past and thus I'm putting it out there incase anyone else finds it useful.