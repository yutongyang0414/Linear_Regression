# Linear_Regression
Let's look at a matrix of pairwise scatterplots first to see if some transformation of the variables might be necessary.

```
US.Cities <- read.csv("pollution.csv")
pairs(~Mort+Precip+Educ+NonWhite+NOX+SO2, data=US.Cities)
```

![Matrix of pairwise scatterplots](https://user-images.githubusercontent.com/87252001/125220199-4d9b6980-e294-11eb-9433-915adca2dbda.png)
