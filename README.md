
## Durbin-Watson Statistic
In statistics, the Durbin–Watson statistic is a test statistic used to detect the presence of autocorrelation (a relationship between values separated from each other by a given time lag) in the disturbance (prediction errors) from a regression analysis. We use user-generated data to illustrate its usefulness, 

```
## import library
> library(lmtest)
```

First we generate a AR(1) error term with parameter rho = 0 (white noise) and perform the Durbin-Watson test

```
> err1 <- rnorm(100)
> 
> ## generate regressor and dependent variable
> x <- rep(c(-1,1), 50)
> y1 <- 1 + x + err1
> 
> ## perform Durbin-Watson test
> dwtest(y1 ~ x)

	Durbin-Watson test

data:  y1 ~ x
DW = 1.9101, p-value = 0.3621
alternative hypothesis: true autocorrelation is greater than 0
```

Now we generate another AR(1) error term with parameter rho = 0.9, then run Durbin-Watson test again and expect strong autocorelation in the disturbance

```
> err2 <- filter(err1, 0.9, method="recursive")
> y2 <- 1 + x + err2
> dwtest(y2 ~ x)

	Durbin-Watson test

data:  y2 ~ x
DW = 0.33352, p-value < 2.2e-16
alternative hypothesis: true autocorrelation is greater than 0
```

Since in the Durbin-Watson test, the null hypothesis is that there is no correlation among the disturbance, i.e., they are independent, and the alternative hypothesis is that disturbance are autocorrelated.A p-value near 0 means we can reject the null (and conclude disturbance are autocorelated). This is consistent with our examples, where in the white noise example we obtain p-value much greater from 0, but in the second example p-value is basically 0.
