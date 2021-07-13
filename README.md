# Linear_Regression

## Outline
1. Understanding and Loading Data
2. Visualization and Prelimarary Exploration
3. Diagnostic Plots
4. Model Comparisons
5. Sequential Variable Selection Algorithm
6. Summary

## Understanding and Loading Data
The first file we are going to explore is *pollution.csv*, which contains data on 60 U.S. cities with variables describe below

* City = Name of the city.
* Mort = Total age-adjusted mortality from all causes, in deaths per 100,000 population 
* Precip = Mean annual precipitation (in inches)
* Educ = Median number of school years completed for people aged 25 or older
* NonWhite = Percentage of 1960 population that is nonwhite
* NOX = Relative pollution potential* of oxides of nitrogen
* SO2 = Relative pollution potential of sulfur dioxide

Let's look at a matrix of pairwise scatterplots first to see if some transformation of the variables might be necessary.

```
US.Cities <- read.csv("pollution.csv")
pairs(~Mort+Precip+Educ+NonWhite+NOX+SO2, data=US.Cities)
```

![Matrix of pairwise scatterplots](https://user-images.githubusercontent.com/87252001/125220199-4d9b6980-e294-11eb-9433-915adca2dbda.png)


The scatterplots of "Mort" against "NOX" and "SO2" suggest that a log transformation of the explanatory variables may be in order.

The following shows the scatterplots with those variables on a logarithmic scale.

```
pairs(~Mort+Precip+Educ+NonWhite+log(NOX)+log(SO2), data=US.Cities)
```

![Logarithmic-scale scatterplot](https://user-images.githubusercontent.com/87252001/125224189-18dee080-e29b-11eb-83d0-0296ced551c2.png)


Additionally, plots with city labels are provided for identification of outliers and potentially influential observations. There are several cities worth noting. In the plot of mortality versus education, York and Lancaster, PA stand out as cities with muchlower mortality than expected when adjusted for education level. On the other hand, the four cities in CA seem to form a cluster separate from the rest in the plot of mortality versus log(NOX). New Orleans is an extreme outlier in the plot of mortality versus log(SO2).
Let's ft a full model initially with all the variables and look at case statistics to identify influential observations.

![Full model](https://user-images.githubusercontent.com/87252001/125360346-bbe63780-e339-11eb-8793-00241d46f7a6.png)

```
model1 <- lm(Mort~Precip+Educ+NonWhite+log(NOX)+log(SO2), data=US.Cities)
summary(model1)
```

Coefficients:
|           |Estimate|Std. Error| t value |Pr|
|-----------|--------|----------|---------|--------|
|(Intercept)| 940.658| 94.055   | 10.00   | 6.8e-14|
|Precip     | 1.947  | 0.701    | 2.78    | 0.0075 |
|Educ       | -14.665| 6.938    | -2.11   |0.0392  |
|NonWhite   | 3.029  |0.669     |4.53     | 3.3e-05|
|log(NOX)   | 6.716  | 7.399    |0.91     |0.3680  |
|log(SO2)   | 11.358 |5.296     | 2.14    | 0.0365 |

Signif. codes: 0 *** 0.001 ** 0.01 * 0.05 . 0.1 1

Residual standard error: 36.3 on 54 degrees of freedom

Multiple R-squared: 0.688,Adjusted R-squared: 0.659

F-statistic: 23.8 on 5 and 54 DF, p-value: 1.42e-12

Cook's distance plot from this model fit (displayed below) suggests that New Orleans (city 60) is truly influential with its distance much greater than 1.

<img width="626" alt="Cook's distance" src="https://user-images.githubusercontent.com/87252001/125359515-5e052000-e338-11eb-8b55-0f891c0f5f65.png">

Let's rerun the regression analysis without New Orleans.

```
model1.r <- lm(Mort~Precip+Educ+NonWhite+log(NOX)+log(SO2), subset=c(-60), data=US.Cities)
summary(model1.r)
```

Coefficients:
|           |Estimate| Std. Error| t value| Pr|
|-----------|--------|-----------|--------|--------|
|(Intercept)| 852.378| 85.933    |9.92    | 1.1e-13 |
|Precip| 1.363 |0.636| 2.14 |0.037|
|Educ| -5.667| 6.524|-0.87| 0.389|
|NonWhite |3.040| 0.591 |5.15 | 3.9e-06|
|log(NOX) |-9.898| 7.731| -1.28| 0.206|
|log(SO2)| 26.033| 5.931| 4.39| 5.5e-05 |

Signif. codes: 0 *** 0.001 ** 0.01 * 0.05 . 0.1 1

Residual standard error: 32.1 on 53 degrees of freedom 

Multiple R-squared: 0.725,Adjusted R-squared: 0.699 

F-statistic: 27.9 on 5 and 53 DF, p-value: 9.93e-14

From the output the p-value for log(NOX) is quite large (0.206). After accounting for the effects of the climate and socioeconomic variables, it is estimated that mortality is strongly associated with SO2 but not with NOX.

Let's remove the term log(NOX) and rerunning the regression yields the following:

```
model2.r <- lm(Mort~Precip+Educ+NonWhite+log(SO2), subset=c(-60), data=US.Cities)
summary(model2.r)
```

Coefficients:
|           |Estimate| Std. Error| t value| Pr|
|-----------|--------|-----------|--------|--------|
|(Intercept)| 862.965 |86.039| 10.03 |6.2e-14 |
|Precip| 1.785| 0.547 |3.26 |0.0019|
|Educ| -7.934 |6.316| -1.26| 0.2145|
|NonWhite| 2.730| 0.542 |5.04 |5.6e-06|
|log(SO2)| 19.688| 3.279| 6.00| 1.7e-07|

Signif. codes: 0 *** 0.001 ** 0.01 * 0.05 . 0.1 1

Residual standard error: 32.3 on 54 degrees of freedom

Multiple R-squared: 0.716,Adjusted R-squared: 0.695

F-statistic: 34.1 on 4 and 54 DF, p-value: 3.48e-14

There are no big problems apparent on the residual plots of this fit (not shown). In conclusion, after accounting for the effects of the climate and socioeconomic variables, there is strong evidence showing that log(SO2) has linear relationship with mortality. With the same values of other variables, if log(SO2) increases 1 unit, mortality will increase 19.688 deaths per 100,000 population.
