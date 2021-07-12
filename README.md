# Linear_Regression

## Outline
1. Understanding and Loading Data
2. Visualization and Prelimarary Exploration
3. Diagnostic Plots
4. Model Comparisons
5. Sequential Variable Selection Algorithm
6. Summary

## Understanding and Loading Data
The first file we are going to explore is *Pollution.csv*, which contains data on 60 U.S. cities with variables describe below

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
