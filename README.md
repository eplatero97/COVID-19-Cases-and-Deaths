# COVID-19 Deaths

## Introduction

The objective of this project is to explore deaths due to COVID-19 over time and over U.S. regions and territories from February 29-April 13th of 2020. 

This project applies the following statistical analysis: 

* Distribution analysis 
* ANOVA tests
* Pair-wise tests
* linear and log regression

Such results are complemented with visualizations to give the reader a more intuitive understanding of our results.

## Dataset

The file `COVID_19_deaths.csv` file reports deaths due to COVID-19 from February 29th to April 13th among the 50 states and all territories in the United States.

## Findings Summary

* U.S. territories contain significantly less deaths due to COVID-19 than U.S. regions.
* Average COVID-19 related deaths are significantly different between U.S. territories and all regions of the U.S. (Northeast, South, and Midwest).
* Average COVID-19 related deaths are significantly different between the Midwest U.S. region and all other regions (Northeast, South) and U.S. territories.
* There is a significant linear (and log) relationship between days and new confirmed deaths due to COVID-19.

## Reported Deaths Due to COVID-19

Below data reports the total deaths per day from COVID-19 between February 29 to April 13th:

```R
         Date total_deaths_per_day
1: 2020-02-29                    1
2: 2020-03-01                    1
3: 2020-03-02                    4
4: 2020-03-03                    3
5: 2020-03-04                    2
6: 2020-03-06                    3
```

#### Exploratory Data Analysis (EDA)

What is the average total deaths per day of the week?

![covid_19_deaths_against_time_day_of_week.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/covid_19_deaths_against_time_day_of_week.png?raw=true)



We can interpret the above graph as each corresponding day having its corresponding positive and negative effect on reported COVID-19 deaths (e.g., on Mondays, we expect to see about a 23 decrease in COVID-19 deaths than Sunday and Tuesday we expect a +48 increase in deaths due to COVID-19).

**# what is the total deaths due to COVID-19 per U.S. region?**

![count_of_covid_19_deaths_per_region.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/count_of_covid_19_deaths_per_region.png?raw=true)



We see that although the U.S. regions have a similar count, U.S. territories appear to have much less deaths due to COVID-19. This may be because: 

* U.S. territories may have a significant less population than U.S. regions, 
* U.S. territories may not have all the medical resources available to determine whether a death was caused by COVID-19 and/or
* U.S. territories may have significant less hospital beds for patients compared to the territories population (which would cause deaths outside the hospital to be undetermined whether it was cauded by COVID-19).

**# what is the distribution of deaths caused by COVID-19?**

![histogram_of_deaths_due_to_covid_19.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/histogram_of_deaths_due_to_covid_19.png?raw=true)

The above histogram resembles an exponential decay. We can see that there is a value of around 800 that causes the histogram to have a large right tail. To have a single (or a few) value so high is concerning and gives concern whether this was an error on inserting 800 instead of 80 or something similar. 

**# what is the distribution of deaths per regions and U.S. territories?**

![histogram_of_deaths_due_to_covid_19_per_region.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/histogram_of_deaths_due_to_covid_19_per_region.png?raw=true)

All distributions per region follow the same exponential decay pattern.

**# is the data variance homogenous?**

We will conduct Levene's test for variance homogeneity to assert whether our data passes such constraint required by ANOVA. Our hypothesis are the following:

* Ho: Variance is homogenous
* Ha: Variance is NOT homogenous

When conducting the Levene's test, a p-value of 4.83207e-32 was received, which means that data did NOT pass homogeneity test of variance, thus we REJECT Ho and resolve to Kruska's non-parametric ANOVA as parametric methods are NOT robust against non-homogeneity

**# Kruska's non-parametric ANOVA test?**

The hypotheis of Kruska's non-parametric ANOVA test are:

* Ho: Distributions among groups are NOT statistically significant
* Ha: Distributions among groups ARE statistically significant

When conducting the ANOVA test, a p-value of 4.83207e-32 was received, which means that we reject the notion that average reported_COVID_cases between region are not statistically significant. Thus, we reject Ho and and now perform pairwise testing to explore which group(s) are statistically significantly different from each other. 

**# Wilcox's pairwise non-parametric test**?

![pairwise_relationships.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/pairwise_relationships.png?raw=true)

From the above graph, we see that there are **significant differences** in reported deaths due to COVID-19 between the following region pairs:

| Territories  & Midwest   | West  & Northeast   |
| ------------------------ | ------------------- |
| Territories  & Northeast | West  & South       |
| Territories  & South     | West  & Territories |



## Data Fitting

On this section we will explore the data fitting capabilities of linear and log regressions.

#### Linear Regression

Let us analyze a linear model with total deaths per day as y and each passing day as x

![covid_19_deaths_against_time_linear_regression.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/covid_19_deaths_against_time_linear_regression.png?raw=true)

Notice that the shape of our graph highly resembles our graph of confirmed COVID-19 cases.

**Why might this be the case?**

Well, it could be that deaths due to COVID-19 are in fact only confirmed once the person has passed away. 

Let’s further explore the properties of our model

```R
Call:
lm(formula = total_deaths_per_day ~ Date, data = death_by_dates)

Residuals:
    Min      1Q  Median      3Q     Max 
-449.11 -314.74  -17.83  230.71  686.51 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) -8.467e+05  7.425e+04  -11.40 1.93e-14 ***
Date         4.619e+01  4.048e+00   11.41 1.90e-14 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 345.7 on 42 degrees of freedom
Multiple R-squared:  0.7561, Adjusted R-squared:  0.7503 
F-statistic: 130.2 on 1 and 42 DF,  p-value: 1.895e-14
[1] "MSE: 119530.052249247"
```

where the data interpretation that the:

- `Date` Coefficient: For every day that passes, there is a `46` increase in deaths due to COVID-19.
- `p-value`: Given that our p-values for our Intercept and Date coefficients are approximately zero, we would reject the Null hypothesis that suggests that there is no relationship between the number of passing days and the number of deaths caused by COVID-19.
- `MSE`: Our Estimated Mean Squared error is `119530.05`.
- `R-squared`: 75.61% of the variation in our deaths due to COVID-19 can be explained by the passing days.

![covid_19_deaths_against_time_linear_regression_assumptions.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/covid_19_deaths_against_time_linear_regression_assumptions.png?raw=true)

From above, although our model approximately satisfies our normal assumption, it does not meet our requirement for the error’s to be uncorrelated. Despite the violation, our model seems to be performing reasonably well.

#### Log Regression

Let us apply a log transformation to our data 

![covid_19_deaths_against_time_log_regression.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/covid_19_deaths_against_time_log_regression.png?raw=true)

From first glance, we can already tell that our log transformation of the input significantly improves our model’s fitted values.

Now, let us analyze its linear properties

```R
Call:
lm(formula = log_deaths ~ Date, data = death_by_dates)

Residuals:
     Min       1Q   Median       3Q      Max 
-1.27330 -0.32549  0.04296  0.43837  1.01432 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) -3.572e+03  1.197e+02  -29.84   <2e-16 ***
Date         1.950e-01  6.526e-03   29.88   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.5574 on 42 degrees of freedom
Multiple R-squared:  0.9551, Adjusted R-squared:  0.954 
F-statistic: 892.6 on 1 and 42 DF,  p-value: < 2.2e-16
[1] "MSE: 0.310656955968673"
```

where the data interpretation is that the:

- `Date` Coefficient: For every log(day) that passes, there is an expected `.195` increase in deaths due to COVID-19
- `p-value`: Given that our p-values for our Intercept and Date coefficients are approximately zero, we would reject the Null hypothesis that suggests that there is no relationship between the number of passing days and the number of deaths caused by COVID-19
- `MSE`: Our Estimated Mean Squared error is `.3107`.
- `R-squared`: 95.51% of the variation in our deaths due to COVID-19 can be explained by the log(days).

While we are unable to compare `MSE` results between the linear and log models, we can see that our `log` transformation significantly increases the linear correlation of the mode.

Let us check if our model validates our linear model’s assumptions.

![covid_19_deaths_against_time_log_regression_assumptions.png](https://github.com/eplatero97/COVID-19-Reported-Deaths/blob/master/imgs/covid_19_deaths_against_time_log_regression_assumptions.png?raw=true)

Our QQ-plot shows that as we further down the tails on either side, the further they are from our linear line.

Further, our residual-fitted plot indicates significant deviation from our desired constant variance as we move further down to the right.

## References

* Template:COVID-19 pandemic data/United States medical cases. (2020, May 3). Retrieved from https://en.wikipedia.org/wiki/Template:COV

