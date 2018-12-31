# Introduction to Linear Modeling Notes

## Review of Single Variable Statistics

### Mean

The mean describes the center of the data.

```python
mean = sum(x) / len(x)
```

### Deviation

The deviation is the distance of each point from center.

```python
dx = x - np.mean(x)
```

### Variance

The variance is a measure of the average of the deviations. By design, the average deviation would be zero, so we square them before averaging.

```python
variance = np.mean(dx * dx)
```

### Standard Deviation

The standard deviation is a better measure of the variance, as the units match those of the deviation.

```python
std = np.sqrt(variance)
```

### Covariance

The covariance is a measure of how two variables change together.

```python
dx = x - np.mean(x)
dy = y - np.mean(y)

deviation_products = dx * dy

covariance = np.mean(dx * dy)
```

### Correlation

Covariance can be difficult to interpret, in part because the variables have a different center and spread.  The result is one variable has more weight when calculating the covariance.
Alternatively, in the correlation every deviation is normalized by the standard deviation of each variable.

```python
zx = dx / np.std(x)
zy = dy / np.std(y)

correlation = np.mean(zx * zy)
```

## Optimization

To optimize a linear fit, the best slope would be $a_1 = \text{covariance}(x, y) / \text{variance}(x)$ and the best intercept would be $a_0 = mean(y) - a_1 \times mean(x)$$.

### NumPy

This can be explicitly coded as follows.

```python
x_mean = np.sum(x) / len(x)
y_mean = np.sum(y) / len(y)

x_dev = x - x_mean
y_dev = y - y_mean

a1 = np.sum(x_dev * y_dev) / np.sum(x_dev ** 2)
a0 = y_mean - (a1 * x_mean)
```

### SciPy

For more complicated models, an algebraic solution in not possible.  While not required for this simple case, SciPy's `optimize` function provides a more generally applicable option.

```python
from scipy import optimize

def model_func(x, a0, a1):
    return a0 + (a1 * x)

param_opt, param_cov = optimize.curve_fit(model_func, x_data, y_data)

a0 = param_opt[0]
a1 = param_opt[1]
```

### Statsmodels

Similar, to SciPy, Statsmodels provides a ordinary least-squares method, but more tailored to a pandas DataFrames.

```
from statsmodels.formula.api import ols

df = pd.DataFrame(dict(x_name=x, y_name=))
model_fit = ols(formula="y_name ~ x_name", data=df).fit()

y_model = model_fit.predict(df)
x_model = x_data

a0 = model_fit.params['intercept']
a1 = model_fit.params['x_name']

e0 = model_fit.bse['intercept]  ## uncertainty or standard error in intercept
e1 = model_fit.bse['x_name']  ## uncertainty or standard error in slope
```

### Scikit-Learn

Scikit-Learn provides another great library for modeling.

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression(fit_intercept=True)

## may need to reshape the data
x_data = x_raw.reshape(-1, 1)
y_data = y_raw.reshape(-1, 1)

model_fit = model.fit(x_data, y_data)

intercept = model.intercept_[0]
slope = model.coef_[0, 0]

future_x = 2100
future_y = model.predict(future_x)
```

## Goodness-of-Fit

There are 3 R-metrics used in statistics.  The first is used for building models:

- RSS: Residual Sum of Squares

The other two are used for evaluating models:

- RMSE: Root Mean Square Error
- R-Squared

### RMSE

To obtain a metric on the quality of fit that is independent of the specific data, we can normalize the residual sum of squares (RSS) by the number of data points.  This becomes the mean squared residuals, or the mean squared error (MSE).

The MSE considers the squared errors.  To have a metric that has the same units as our original response, we first must take the square root of the MSE, giving the root mean square error (RMSE).

```python
residuals = y_model - y_data
RSS = np.sum(np.square(residuals))

## normalize the RSS
mean_squared_residuals = RSS / len(residuals)
## or equivalently
MSE = np.mean(np.square(residuals))

RMSE = np.sqrt(np.mean(np.square(residuals)))
RMSE = np.std(residuals)
```

### R-Squared

The R-squared metric quantifies how much of the variation is described by the model.  More specifically, it compares the ratio between the residual sum of squares to the overall variance.

Deviations:
```python
deviations = np.mean(y_data) - y_data
VAR = np.sum(np.square(deviations))  ## not actually the variance, not divided by n
```
The deviations captures all the variation in the data.  This includes both the overal trend/s and any random variantion.

Residuals:
```python
residuals = y_model - y_data
RSS = np.sum(np.square(residuals))
```
The residuals capturs only the variation that differs from the chosen model.

R-squared:
```python
r_squared = 1 - (RSS / VAR)
r = correlation(y_data, y_model)
```
The ratio between RSS and VAR provides a scaled describes how much of the variance in the data is described by the model.

Jason Vestuto makes the following important comparison between the difference in R-squared and RMSE.  "When the variation due to linear trend is larger than the variation due to residuals, the model is better.  The effect is real and practical.  The randomness of the residuals can completely mask the linear dependence for small slope but be relatively unimportant for large slope.  R-squared captures this but RMSE does not."

## Sampling and Resampling (Bootstrapping)

Any measured dataset is a sample of the larger population. Resampling is used to quantify the variantion in that sampling process and infer the variation of the population.

Here is an example of taking 30 samples of 100 points from the larger population of data.  For each sample, the sample mean is computed and appended to a list.

```python
n_points = 100
n_samples = 30
distribution_of_means = []

for n in range(n_samples):
    sample = np.random.choice(data, n_points)
    distribution_of_means.append(sample.mean())

mean_of_means = np.mean(distribution_of_means)
stdev_of_means = np.std(distribution_of_means)
```

## Estimating a Population distribution from a Sample

We will commonly assume a sample is representative of the entire population.  Under the assumption that the population distribution is Gaussian and also that the population model can be estimated from the sample statistics, the population model would be defined as follows.

```python
def gaussian_model(x, mu=0, sigma=1):
    coeff_part = 1 / np.sqrt(2 * np.pi * sigma ** 2)
    exp_part = np.exp(-(x - mu) ** 2 / (2 * sigma ** 2))
    return coeff_part * exp_part

# compute the sample statistics
mean = np.mean(sample)
stdev = np.std(sample)

# model the population based on these sample statistics
pop_model = gaussian(sample, mu=mean, sigma=stdev)
```

### Liklihood vs Probability

Conditional naming convention:
- Probability: P(data|model)
- Likelihood: L(model|data)

If we are comparing two models, we want to select the one with the greater likelihood of outputting the measured data.  In code this could be expressed as follows.

```python
# guess parameters
mu_guess = np.mean(sample_distances)
sigma_guess = np.std(sample_distances)

# compute the probability of each sample point
probabilities = np.zeros(len(sample_distances))
for n, distance in enumerate(sample_distances):
    probabilities[n] = gaussian_model(distance, mu=mu_guess, sigma=sigma_guess)

likelihood = np.product(probabilities)
loglikelihood = np.sum(np.log(probabilities))
```

Note that using the logarithm of the likelihood produces better numerical properties.

We can iterate the above process for a series of guess values to identify the best model.

```python
n_guesses = 101

# array of guesses
low_guess = sample_mean - 2 * sample_stdev
high_guess = sample_mean + 2 * sample_stdev
mu_guesses = np.linspace(low_guess, high_guess, n_guesses)

loglikelihoods = np.empty(n_guesses)
for n, mu_guess in enumerate(mu_guesses):
    loglikelihoods[n] = compute_loglikelihood(sample_distances, mu=mu_guess, sigma=sample_stdev)

# best guess
max_loglikelihood = np.max(loglikelihood)
best_mu = mu_guesses[np.argmax(loglikelihood)]
```

This iteration should return `sample_mean` as the best estimator of the population mean, or the "Maximum Likelihood Estimator" of the population mean `mu`.

## Bootstrap Resampling

```python
# use the sample data as the population model
population_model = sample_data

bootstrap_mean = np.empty(n_resamples)

# simulate repeated data acquisitions by resampling the "model"\
for i in range(n_resamples):
    bootstrap_sample = np.random.choice(population_model, size=resample_size, replace=True)
    bootstrap_mean[i] = np.mean(bootstrap_sample)

# get the mean of the bootstrap resampled distribution
estimate_temperature = np.mean(bootstrap_means)

# get the standard deviation of the bootstrap resampled distribution
estimate_uncertainty = np.std(bootstrap_means)

# another possible metric would be to consider the percentile ranges
ci_90 = np.percentile(bootstrap_mean, [5, 95])
```

## Hypothesis Testing

There are three types of error common to data sampling and measurement:

1. Measurement error
    - broken sensor
    - incorrect data recording
2. Sampling bias
    - only sampling/measuring from a small portion of the population that is not representative of the entire population
3. Random chance
    - could any trends in the data be a consequence of random fluctuations or noise
    - Is our affect due to a relationship or due to random chance?  Does the ordering or grouping of the data cause an effect larger than could be produced by randomly shuffled data?

```python
# group into long and short trips
group_short = sample_distances[times < 5]
group_long = sample_distances[times > 5]

# resample distributions
resample_short = np.random.choice(group_short, size=500, replace=True)
resample_long = np.random.choice(group_long, size=500, replace=True)

# test statistic
test_statistic = resample_long - resample_short

# effect size as mean of test statistic distribution
effect_size = np.mean(test_statistic)

# concatenate and shuffle the groups
shuffle_bucket = np.concatenate((group_short, group_long))
np.random.shuffle(shuffle_bucket)

# split the data
slice_index = len(shuffle_bucket) // 2
shuffled_half1 = shuffle_bucket[0:slice_index]
shuffled_half2 = shuffle_bucket[slice_index:]

# resample shuffled populations
shuffled_sample1 = np.random.choice(shuffled_half1, size=500, replace=True)
shuffled_sample2 = np.random.choice(shuffled_half2, size=500, replace=True)

# recompute the effect size
shuffled_test_statistic = shuffled_sample2 - shuffled_sample1
effect_size = np.mean(shuffled_test_statistic)
```
