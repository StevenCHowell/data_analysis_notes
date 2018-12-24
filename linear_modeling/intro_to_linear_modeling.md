# Review of Single Variable Statistics

## Mean

The mean describes the center of the data.

```python
mean = sum(x) / len(x)
```

## Deviation

The deviation is the distance of each point from center.

```python
dx = x - np.mean(x)
```

## Variance

The variance is a measure of the average of the deviations. By design, the average deviation would be zero, so we square them before averaging.

```python
variance = np.mean(dx * dx)
```

## Standard Deviation

The standard deviation is a better measure of the variance, as the units match those of the deviation.

```python
std = np.sqrt(variance)
```

## Covariance

The covariance is a measure of how two variables change together.

```python
dx = x - np.mean(x)
dy = y - np.mean(y)

deviation_products = dx * dy

covariance = np.mean(dx * dy)
```

## Correlation

Covariance can be difficult to interpret, in part because the variables have a different center and spread.  The result is one variable has more weight when calculating the covariance.
Alternatively, in the correlation every deviation is normalized by the standard deviation of each variable.

```python
zx = dx / np.std(x)
zy = dy / np.std(y)

correlation = np.mean(zx * zy)
```