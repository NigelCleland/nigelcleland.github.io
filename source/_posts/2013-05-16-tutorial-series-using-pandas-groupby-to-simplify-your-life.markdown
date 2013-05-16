---
layout: post
title: "Tutorial Series: Using pandas groupby to simplify your life"
date: 2013-05-16 10:40
comments: true
categories: [python, pandas, groupby, data analysis]
---

This is the second instalment in a brief series on using the pandas library for data analysis. The general aim of this series to to highlight some of the cool functionality which is in pandas. None of this is revolutionary, however Wes McKinney and the other contributors have made exceptional contributions to the speed and ease of which it is completed.

In this post I'm going to over the groupby function. Highlight a couple of the interesting things which you can do with it and generally show how much simpler pandas makes it to accomplish data analysis. I'm going to be using a real data set, hydrology data from the last 80 odd years for the New Zealand electricity grid, this dataset has approximately 30,000 data points (1 per day) and is therefore a good size for this work. This post will cover a brief example of using groupby for time series and won't go through any of the more complex functionality and usage which can be developed when working with full DataFrames.

<!-- more -->

Right to begin we need to load our data, I'm going to use a custom module which makes this a bit easier called [nzem](http://github.com/NigelCleland/nzem). The nzem library is my own personal library which simplifies some of the initial data analysis I use most frequently. For example, the following will load hydrology data and automatically parse the relatively disgusting date format which the csv file has. Note, that the nzem module automatically loads and initlises the [masks](http://github.com/NigelCleland/masks) library which I discussed in my last post.

``` python
# Load modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import nzem

# Load the data, niwa_date is a specific format not recognised by the dateutil.parser utility
hydro_data = nzem.load_csvfile('hydro_data/Hydro_Lake_Data.csv', niwa_date=True)

# Get a single column, daily_level will then be a series
daily_level = hydro_data["Daily Stored"]

# Confirmation that we are working with a time series
type(daily_level) # pandas.core.series.Series
type(daily_level.index) # pandas.tseries.index.DatetimeIndex
```

Now, we have a single series which contains the daily storage level of the entire countries hydro lakes (in GWh).

Now, on to using the group by functionality. The pandas [groupby](http://pandas.pydata.org/pandas-docs/stable/groupby.html) library has two primary uses. Aggregations and Transformations. An Aggregation will return a new series with indices set by the groupby keys.
A Transformation will return the original series transformed according to the predefined function.

This is best demonstrated by example

``` python
# Aggregation
# Group by the day of year, take the mean of these points
doy = lambda x: x.dayofyear
doy_mean = daily_level.groupby(doy).aggregate(np.mean) # Note you don't need to use the () at the end of np.mean.

len(doy_mean) # 366, one for each day

# Transformation
# Group by the day of year, but subtract the mean from each point

relative_mean = lambda x: x - np.mean(x)
doy_rel_mean = daily_level.groupby(doy).transform(relative_mean)

len(doy_rel_mean) == len(daily_level) # returns True
```

Now, we need to cover where we use each one. The aggregations are useful when what we are interested in is the summary statistic, and we do not intend to remerge it with our dataframe. For example, if we wanted to remerge it we would have to go through a bunch of steps which would be quite painful. The transform function on the other hand should be used when you want a data point equivalent to each of the original series/dataframe.

For example, in the above example the transform example can be used to create a repeating series which we then apply against our raw data. A visual explanation as to why this is beneficial may be most appropriate.
I'm not going to attempt to make this figure extremely pretty, that is for a later post which will cover beautifying matplotlib somewhat. I am chaining methods here which if you are not used to pandas may be slightly confusing. The explanation is that 

``` python
# Helper functions to reduce line length
per10 = lambda x: np.percentile(x, q=10)
rel_per10 = lambda x: x - per10(x)

# Create our subplots, share the y axis to make comparison more intuitive
fig, axes = plt.subplot(1, 3, figsize=(16,9), sharey=True)

# Plot from 2012 only to avoid too much visual clutter.
# Raw Data
daily_level.ix["2012":].plot(ax=axes[0])

# Transformation to be applied
daily_level.groupby(doy).transform(per10).ix["2012":].plot(ax=axes[1])

# Final Result
daily_level.groupby(doy).transform(rel_per10).ix["2012":].plot(ax=axes[2])

# Labels
titles = ["Raw hydrology data", "Bottom Decile", "Transformed Data"]
for ax, title in zip(axes, titles):
	ax.set_title(title)

axes[0].set_ylabel("Quantity of Hydro Storage [GWh]")

fig.savefig("hydro_data_over_time.png", dpi=150)
```

{% img /images/may2013/hydro_data_over_time.png %}

Right, an explanation. What we have done is create three plots which gives the reader a fairly decent overview as to the hydrology situation from 2012 onwards. From this figure we get an appreciation of the raw situation as seen on a daily basis. In the second plot we can then visually compare this to the bottom decile of what has occurred for the past 80 years. Finally, in the final plot we compare the two sets of data which highlights the low hydro storage experienced during the Autumn and Winter of 2012. It also adequately shows the large series of inflows which occurred over the 2012-2013 new year periods. Finally, the tail end of the figure shows that hydrology levels have again deteriorated from the large burst of inflows.

Not too bad for a few simple lines of code. In total, to produce the image required 14 individual lines excluding the module import.

Hopefully this post has given you a bit more of a detailed explanation as to using the groupby functionality, especially with time series. I'll attempt to extend upon this in the coming days as time and motivation permits.
