# Comparing Different Distributions

When comparing different distributions, before you can identify which test to use for the comparison, identify if the distributions are parametric or nonparametric.  There are 3 important assumptions for parametric distributions

1. The samples within each distribution are independent
2. The distributions are normal 
3. The distributions are homoscedastic, or the standard deviations are the same

To test normallity, consider the following tests

- [Shapiro-Wilk test](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test), typically the best options though it does not work well for samples with many identical values.
- [Anderson-Darling test](https://en.wikipedia.org/wiki/Anderson%E2%80%93Darling_test), typically second best choice, after the Shapiro-Wilk test.
- [D'Agostino's K-squared test](https://en.wikipedia.org/wiki/D%27Agostino%27s_K-squared_test), evaluating the skewness and kurtosis of the distribution.
- [Kolmogorov-Smirnov test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test), nonparametric test 
- [Kruskal-Wallis test](https://en.wikipedia.org/wiki/Kruskal%E2%80%93Wallis_one-way_analysis_of_variance), is a nonparametric test, comparable to the [one-way annalysis of variance (ANOVA) test](https://en.wikipedia.org/wiki/One-way_analysis_of_variance)
- [Shapiro-Francia test](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Francia_test)
- [Normal probability plot](https://en.wikipedia.org/wiki/Normal_probability_plot), graphical technique for evaluating outliers, skewness, kurtosis, mixtures, and a need for transformation.

It is surprising that there are so many different tests to evaluate normality.  Additionally, it is surprising to note that some of these tests, useful for identifying if a sample distribution is parametric, only apply under the assumption that the sample is parametric.
This introduces a chicken-and-the-egg scenario.  One way to address this is to first perform the nonparametric test, which assumes less, then if the data passes, then use the parametric test.
I am reminded of Allen Downey's blog posts, [There is only one test!](http://allendowney.blogspot.com/2011/05/there-is-only-one-test.html), and [There is still only one test!](http://allendowney.blogspot.com/2016/06/there-is-still-only-one-test.html).
