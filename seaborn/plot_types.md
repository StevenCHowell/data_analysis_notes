# Seaborn plot types

## `distplot()`

The `distplot()` function provides a histogram alternative which plots a Gaussian Kernel Density Estimate (KDE).  Essentially, this is a continuous representation of a histogram.

## `regplot()`

The `regplot()` function generate a scatter plot with a regression line.

```python
sns.regplot(
    x='alcohol',
    y='ph',
    data=df,
    order=2,  # apply a polynomial model in the regression
)
```

## `lmplot()`

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

## `swarmplot()`

Similar to a histogram, with every data point represented.

## `stripplot()`

Similar to the swarmplot, without the algorithmic spreading of the data.  Without the `jitter=True` parameter, all the data falls on a single line.  With that parameter, the data is spread into a thicker "strip", true to the plots name.

## `lvplot()`

Letter value (LV) plots provide a non-parametric estimate of a dataset's distribution.  They are similar to boxplots, and violin plots, but unlike violin plots, they do not require fitting a kernel density estimate so they are faster to generate.

## `heatmap()`

The `heatmap` plot is ideal for visualizing a matrix of numbers in a more visually simple way.  One of the most common applications is for plotting a cross-tabulation or correlation matrix (not the same).

```python
# Create a crosstab table of the data
pd_crosstab = pd.crosstab(df['label_2'], df['label_2'])

# Plot a heatmap of the table
sns.heatmap(pd_crosstab)

# Rotate tick marks for visibility
plt.yticks(rotation=0)
plt.xticks(rotation=90)

plt.show()
```

The heatmap can be customized for more visual appealing result.

```python
sns.heatmap(
    df,
    annot=True,  # show numbers in cells
    fmt='d',  # numeric format for annotations
    cmap='YlGnBu',
    cbar=False,  # annotations already included
    linewidths=0.5,  # add space between cells
    center=df.loc[row, col],  # center the cmap at a specific value
)
```

# Colors

You can pass matplotlib colorcodes to functions by setting `color_codes=True`
```python
sns.set(color_codes=True)
sts.distplot(df['col'], color='g')
```

Example of the built-in palettes
```python
for p in sns.palettes.SEABORN_PALETTES:
    sns.set_palette(p)
    sns.palplot(sns.color_palette())
    plt.show()
```

To create a palette from a predefine colormap use
```python
sns.color_palette(cmap_name, n_colors=8)
```

```python
for i, name in enumerate(['Purples', 'Blues', 'Reds', 'Greens', 'inferno', 'viridis', 'coolwarm']):
    sns.palplot(sns.color_palette(name, n_colors=i+3))
    plt.show()
```

In matplotlib, you can group several `ax.set_` options using `ax.set`
```python
fig, ax = plt.subplots()
ax.plot(some_data)
ax.set(
    xlabel='x description',
    ylabel='y description',
    xlim=(0, 100),
    ylim=(3, 6),
    title='data description',
)
```

Here is a nice way to draw attention to a specific value, e.g., mean and median.

```python
# Create a figure and axes. Then plot the data
fig, ax = plt.subplots()
sns.distplot(df['fmr_1'], ax=ax)

# Add vertical lines for the median and mean
ax.axvline(x=df.fmr_1.median(), color='m', label='Median', linestyle='--', linewidth=2)
ax.axvline(x=df.fmr_1.mean(), color='b', label='Mean', linestyle='-.', linewidth=2)

# Show the legend and plot the data
ax.legend()
plt.show()
```

## Combining plots using `FacetGrid`, `factorplot`, and `lmplot`

## `FacetGrid`

Uses a two-step process to generate facetted plots.

```python
g = sns.FacetGrid(df, col='HighDeg')
g.map(sns.boxplot, 'Tuition', order=['1', '2', '3', '4'])
```

## `factorplot`

Combines the two-step process of `FacetGrid` into a single function.  Note that `factorplot`'s `kind` input does not support all seaborn plot types, such as `lvplot`.

```python
sns.factorplot(x='Tuition', data=df, col='HighDeg', kind='box')
```

## `lmplot`

Uses `FacetGrid` to plot scatter and regression plots.

```python
sns.lmplot(data=df, x='Tuition', y='SAT_AVG_ALL',
           col='HighDeg', row='Region')
```

## `PairGrid` and `pairplot`

Similar to the relation between `FacetGrid` and `factorplot`, so to `PairGrid` gives more control over `pairplot` on visualizing the relation between factors/features of the data.

To get a series of plot similar to the `pairplot` default, you would map `plt.hist` to the diagonal and `plt.scatter` to the off-diagonal.

```python
g = sns.PairGrid(df, vars=['factor_1', 'factor_2'])
g = g.map_diag(plt.hist)
g = g.map_offdiag(plt.scatter)
```

By default, `pairplot` includes a regression line on the off-diagonal plots.