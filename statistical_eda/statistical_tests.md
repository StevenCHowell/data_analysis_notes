# Comparing Different Sample Distributions

## Testing for statistically significant differences between <a href="https://www.codecogs.com/eqnedit.php?latex=\geq" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\geq" title="\geq" /></a> 2 groups for a single factor variable

1. Perform Levene's Test to check for homogeneity of variances (homoscedasticity) between the different groups
2. Based on the results, perform a regression fit and get the residiuals using the appropriate ANOVA to check if the group means are different
    - homoscedasticity: One Way ANOVA
    - heteroscadisticity: Welch's One Way ANOVA

When comparing different distributions, before you can identify which test to use for the comparison, identify if the distributions are parametric or nonparametric.  There are 3 important assumptions for parametric distributions

1. The samples within each distribution are independent
2. The distributions are normal 
3. The distributions are homoscedastic, or the standard deviations are the same

## Variance Testing

To compare the variance of the different sample distributions, consider the following tests:

- [Levene test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35a.htm), typically the best option as it is insensitive to departures from normality.  This is implemented in SciPy as [`scipy.stats.levene()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.levene.html).
- [Bartlett’s test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda357.htm), sensitive to departures from normality.  I am not sure when this would be preferred over the Levene test.  This is implemented in SciPy as [`scipy.stats.bartlett()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.bartlett.html).

## Normality Testing

To test normality, consider the following tests:

- [Shapiro-Wilk test](https://en.wikipedia.org/wiki/Shapiro-Wilk_test), typically the best options though it does not work well for samples with many identical values.  This is implemented in SciPy as [`scipy.stats.shapiro()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.shapiro.html).
- [Anderson-Darling test](https://en.wikipedia.org/wiki/Anderson-Darling_test), typically second best choice, after the Shapiro-Wilk test.  This is implemented in SciPy as [`scipy.stats.anderson()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.anderson.html).
- [D'Agostino's K-squared test](https://en.wikipedia.org/wiki/D%27Agostino%27s_K-squared_test), evaluating the skewness and kurtosis of the distribution.  Note that this requires a sample size of 8 or larger.  This is implemented in SciPy as [`scipy.stats.normaltest()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html).
- [Kolmogorov-Smirnov test](https://en.wikipedia.org/wiki/Kolmogorov-Smirnov_test), nonparametric test. This is implemented in SciPy two different ways, for comparing to some reference data or data generating function, [scipy.stats.kstest](`https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.stats.kstest.html), and for comparing two samples, [`scipy.stats.ks_2samp()`](https://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.ks_2samp.html).
- [Kruskal-Wallis test](https://en.wikipedia.org/wiki/Kruskal-Wallis_one-way_analysis_of_variance), is a nonparametric test, comparable to the [one-way annalysis of variance (ANOVA) test](https://en.wikipedia.org/wiki/One-way_analysis_of_variance).  This is implemented in SciPy as [`scipy.stats.kruskal()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kruskal.html).
- [Pearson's chi-squared test](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test).  This is implemented in SciPy as [`scipy.stats.chi2_contingency()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.chi2_contingency.html).
- [Shapiro-Francia test](https://en.wikipedia.org/wiki/Shapiro-Francia_test), a simplification of the Shapiro-Wilk test.
- [Normal probability plot](https://en.wikipedia.org/wiki/Normal_probability_plot), graphical technique for evaluating outliers, skewness, kurtosis, mixtures, and a need for transformation.  This is imlemented in SciPy as [`scipy.stats.probplot()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.probplot.html).

It is surprising that there are so many different tests to evaluate normality.  Additionally, it is surprising to note that some of these tests, useful for identifying if a sample distribution is parametric, only apply under the assumption that the sample is parametric.
This introduces a chicken-and-the-egg scenario.  One way to address this is to first perform the nonparametric test, which assumes less, then if the data passes, then use the parametric test.

Discussing all these different tests reminds me of Allen Downey's blog posts, [There is only one test!](http://allendowney.blogspot.com/2011/05/there-is-only-one-test.html), and [There is still only one test!](http://allendowney.blogspot.com/2016/06/there-is-still-only-one-test.html).
