# Linear_Regression
Let's look at a matrix of pairwise scatterplots first to see if some transformation of the variables might be necessary.

US.Cities <- read.csv("pollution.csv")
pairs(~Mort+Precip+Educ+NonWhite+NOX+SO2, data=US.Cities)

