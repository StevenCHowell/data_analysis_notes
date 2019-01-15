# Notes from Statistical EDA

- When creating a histogram, a good rule of thumb is to select the number of bins as the square root of the number of samples.  `n_bins = np.sqrt(len(data))`

## Bee Swarm Plot

Compared to a histogram, a bee swarm plot captures the same data without being susceptible to binning bias.  This is because each data point is included in the plot.
The Bee Swarm plot loses its value if the number of data points gets too large for the plotting region.
It may be possible to alleviate this using smaller markers to represent each data point.
A bee swarm plot is available from the Seaborn package as
`sns.swarmplot()`.

```python
_ = sns.swarmplot(x='x_col', y='y_col', data=df)
```

## Empirical Cumulative Distribution Function (ECDF)

Similar to a bee swarm plot, an empirical cumulative distribution function provides a better summary of data than a histogram would.
Similar to a histogram, the data values we are interested are plot on the x-axis.  For an ECDF, the x-values are the sorted data values.  The y-values range from 1/n to 1, in equally spaced increments.
This will show clusters of similar values as sharp vertical increases in the curve.

Here is example code that returns the x- and y-values for constructing an ECDF:

```python
def ecdf(data):
    """Compute ECDF for a one-dimensional array of measurements."""
    # Number of data points: n
    n = len(data)

    # x-data for the ECDF: x
    x = np.sort(data)

    # y-data for the ECDF: y
    y = np.arange(1, n+1) / n

    return x, y

x, y = ecdf(data)
_ = plt.plot(x, y, marker='.', linestyle='none')
```

## Gaussian Kernel Density Estimate (KDE)

Another histogram alternative is the Gaussian Kernel Density Estimate (KDE).  Essentially, this is a continuous representation of a histogram.  It can be generated using Seaborn's `distplot` function.

```python
import seaborn as sns

sns.distplot(df['alcohol'])
```

## Percentile Values

NumPy's `percentile` function provides a quick way to calculate the percentiles.

```python
percentiles = np.array([2.5, 25, 50, 75, 97.5])
p_vals = np.percentile(data, percentiles)
```

These can be shown on the ECDF to provide better intuition about the distribution.

```python
_ = plt.plot(
    p_vals, percentiles/100,
    marker='D', color='red',
    linestyle='none'
)
```

## Distribution Function

Two closely related distribution functions are the probability distribution function (PDF) and the cumulative distribution function (CDF).  The PDF depicts how likely something is to happen.  Though a PDF will have corresponding x- and y-values, it does not make sense to talk about the probability at a precise x-value, as it would be zero.  Instead one should consider the probability of occurance between two x-values, which is described by integrating the curve between these limits.

This idea of integrating the PDF leads to the CDF.  The CDF depicts the cumulative probability of occurance between x=0 and the specified x-value.

When comparing a data set to a Gaussian normal distribution, the CDF provides a better comparison than would a PDF.  Comparing a PDF to the histogram of the data is prone to error caused by binning bias.  Instead, calculate the CDF of a Gaussian distribution using the mean and standard deviation of the data, and comparing this with the ECDF.

```python
mean = np.mean(data)
std = np.std(data)
samples = np.random.normal(mean, std, size=10000)  # this seems unnecessary
x, y = ecdf(data)
x_theory, y_theory = ecdf(samples)
_ = plt.plot(x_theory, y_theory)
_ = plt.plot(x, y, marker='.', linestyle='none')
plt.show()
```

Another important distribution function is the exponential distribution function.  When the occurance of an event can be modelled using a Poisson process, an exponential distribution describes the time between events.  This distribution is implemented within NumPy as `np.random.exponential(scale=1.0, size=None)`.