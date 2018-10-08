# Seaborn plot types

# `distplot()`
The `distplot()` function provides a histogram alternative which plots a Gaussian Kernel Density Estimate (KDE).  Essentially, this is a continuous representation of a histogram.

# `regplot()`
The `regplot()` function generate a scatter plot with a regression line.

```python
sns.regplot(
    x='alcohol',
    y='ph',
    data=df
)
```

# `lmplot()`
The `lmplot()` function is a more versatile version of `regplot()`. One advantage is it provides the ability to facet the plotting qualities to further express the data.

```python
sns.lmplot(
    x='alcohol',
    y='ph',
    data=df,
    col='type',      # facetting example
    hue = 'Region'
)
```