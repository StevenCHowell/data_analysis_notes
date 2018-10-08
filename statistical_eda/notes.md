# Notes from Statistical EDA

- When creating a histogram, a good rule of thumb is to select the number of bins as the square root of the number of samples.

## Bee Swarm Plot

Compared to a histogram, a bee swarm plot captures the same data without being susceptible to binning bias.  This is because each data point is included in the plot.
The Bee Swarm plot loses its value if the number of data points gets too large for the plotting region.
It may be possible to alleviate this using smaller markers to represent each data point.
A bee swarm plot is available from the Seaborn package as
`sns.swarmplot()`.

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
```

## Gaussian Kernel Density Estimate (KDE)

Another histogram alternative is the Gaussian Kernel Density Estimate (KDE).  Essentially, this is a continuous representation of a histogram.  It can be generated using Seaborn's `distplot` function.

```python
import seaborn as sns

sns.distplot(df['alcohol'])
```