# Comparing Different Sample Distributions

## Testing for statistically significant differences between <a href="https://www.codecogs.com/eqnedit.php?latex=\geq" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\geq" title="\geq" /></a> 2 groups for a single factor variable

To compare two or more independent populations, we would use the t-test or analysis of variance (ANOVA) test, respectively.  The hypothesis we would be testing are

- <a href="https://www.codecogs.com/eqnedit.php?latex=\text{H}_o:\,&space;\mu_1&space;=&space;\mu_2&space;=&space;\dots&space;=&space;\mu_n" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{H}_0:\,&space;\mu_1&space;=&space;\mu_2&space;=&space;\dots&space;=&space;\mu_n" title="\text{H}_0:\, \mu_1 \neq \mu_2 \neq \dots \neq \mu_n" /></a>
- <a href="https://www.codecogs.com/eqnedit.php?latex=\text{H}_a:\,&space;\mu_1&space;=&space;\mu_2&space;=&space;\dots&space;=&space;\mu_n" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{H}_a:\,&space;\mu_1&space;\neq&space;\mu_2&space;\neq&space;\dots&space;\neq&space;\mu_n" title="\text{H}_a:\, \mu_1 \neq \mu_2 \neq \dots \neq \mu_n" /></a>

Before performing the t-test or the ANOVA, one should verify the samples meet the following assumptions

1. The samples within each distribution are independent
2. The response value is quantitative and continuous 
2. The distributions are normal (parametric)
3. The standard deviations for the distributions are the same (homoscedastic)

Given assumption 1 and 2 are met, the process of testing 3 and 4, then proceeding based on those results would be as follows:

1. Test the assumptions
    - Normality (parametric): Shapiro-Wilk's test or Kolmogorov-Smirnov
    - Homogeneity of variances (homoscedastic): Levene's test
2. Based on these results, compare the population distributions
    - Both pass, parametric and homoscedastic:
        - 2 groups: t-test (or Welch's t-test for small sample sizes, less than 30)
        - 2 or more groups: ANOVA test
    - Fail one, parametric but heteroscedastic:
        - 2 groups: [Welch's t-test](https://en.wikipedia.org/wiki/Welch%27s_t-test) (implemented in Python as [`scipy.stats.ttest_ind(a, b, axis=0, equal_var=False)`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html))
        - 2 or more groups: Welch's ANOVA (have not found Python implementation)
    - Fail one, homoscedastic but nonparametric:
        - 2 groups: Mann-Whitney U test (also know as the Wilcoxon rank-sum test)
        - 2 or more groups: Kruskal Wallis test (<a href="https://www.codecogs.com/eqnedit.php?latex=\text{H}_o/\text{H}_a" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{H}_o/\text{H}_a" title="\text{H}_o/\text{H}_a" /></a>: mean ranks of the groups are the same/different)
    - Fail both, nonparametric and heteroscedastic:
        - Still under active research 
            - [recent thesis](https://scholarscompass.vcu.edu/cgi/viewcontent.cgi?article=5026&context=etd)
            - [recent article](https://doi.org/10.1080/10485252.2016.1225735)
        
## Variance Testing

To compare the variance of the different sample distributions, consider the following tests:

- [Levene test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35a.htm), typically the best option as it is insensitive to departures from normality.  This is implemented in SciPy as [`scipy.stats.levene()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.levene.html).
- [Bartlett’s test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda357.htm), sensitive to departures from normality.  I am not sure when this would be preferred over the Levene test.  This is implemented in SciPy as [`scipy.stats.bartlett()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.bartlett.html).

## Normality Testing

To test normality, consider the following tests:

- [Shapiro-Wilk test](https://en.wikipedia.org/wiki/Shapiro-Wilk_test), typically the best options though it does not work well for samples with many identical values.  This is implemented in SciPy as [`scipy.stats.shapiro()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.shapiro.html).
- [Anderson-Darling test](https://en.wikipedia.org/wiki/Anderson-Darling_test), typically second best choice, after the Shapiro-Wilk test.  This is implemented in SciPy as [`scipy.stats.anderson()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.anderson.html).
- [D'Agostino's K-squared test](https://en.wikipedia.org/wiki/D%27Agostino%27s_K-squared_test), evaluating the skewness and kurtosis of the distribution.  Note that this requires a sample size of 8 or larger.  This is implemented in SciPy as [`scipy.stats.normaltest()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html).
- [Kolmogorov-Smirnov test](https://en.wikipedia.org/wiki/Kolmogorov-Smirnov_test), nonparametric test. This is implemented in SciPy two different ways, for comparing to some reference data or data generating function, [`scipy.stats.kstest()`](https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.stats.kstest.html), and for comparing two samples, [`scipy.stats.ks_2samp()`](https://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.ks_2samp.html).
- [Kruskal-Wallis test](https://en.wikipedia.org/wiki/Kruskal-Wallis_one-way_analysis_of_variance), is a nonparametric test, comparable to the [one-way annalysis of variance (ANOVA) test](https://en.wikipedia.org/wiki/One-way_analysis_of_variance).  This is implemented in SciPy as [`scipy.stats.kruskal()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kruskal.html).
- [Pearson's chi-squared test](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test).  This is implemented in SciPy as [`scipy.stats.chi2_contingency()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.chi2_contingency.html).
- [Shapiro-Francia test](https://en.wikipedia.org/wiki/Shapiro-Francia_test), a simplification of the Shapiro-Wilk test.
- [Normal probability plot](https://en.wikipedia.org/wiki/Normal_probability_plot), graphical technique for evaluating outliers, skewness, kurtosis, mixtures, and a need for transformation.  This is imlemented in SciPy as [`scipy.stats.probplot()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.probplot.html).

It is surprising that there are so many different tests to evaluate normality.  Additionally, it is surprising to note that some of these tests, useful for identifying if a sample distribution is parametric, only apply under the assumption that the sample is parametric.
This introduces a chicken-and-the-egg scenario.  One way to address this is to first perform the nonparametric test, which assumes less, then if the data passes, then use the parametric test.

Discussing all these different tests reminds me of Allen Downey's blog posts, [There is only one test!](http://allendowney.blogspot.com/2011/05/there-is-only-one-test.html), and [There is still only one test!](http://allendowney.blogspot.com/2016/06/there-is-still-only-one-test.html).

Recommended introductory books discussing this topic:
1. [Statistics in a Nutshell](http://shop.oreilly.com/product/0636920023074.do) (1st edition has more discussion on alternative test cases)
2. [Intermediate Statistics for Dummies](https://books.google.com/books/about/Intermediate_Statistics_For_Dummies.html?id=jwmdUe0dDSAC) 
