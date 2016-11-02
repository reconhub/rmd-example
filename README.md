

## Estimating the incubation period of a simulated Ebola outbreak

#### Note: compiling this document

This is a rmarkdown document.
Compile it using [*knitr*](http://yihui.name/knitr/) (google: *knitr* for more information).
Use `knit2html(...)` to generate a html report.



#### Incubation period

In the following, we use a simulated Ebola outbreak linelist to illustrate how to estimate the empirical distribution of the incubation time (delay between exposure and symptom onset).


We first load the data:


```r
library(outbreaks)
dat <- ebola.sim$linelist
```

We compute the incubation period and ensure that the result is `numeric`:

```r
incub <- dat$date.of.onset - dat$date.of.infection
head(incub)
```

```
## Time differences in days
## [1] NA  6  3 NA  4 37
```

```r
length(incub)
```

```
## [1] 5888
```

```r
summary(incub)
```

```
##   Length    Class     Mode 
##     5888 difftime  numeric
```

```r
incub <- as.numeric(incub)
summary(incub)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   0.000   4.000   8.000   9.921  14.000  62.000    2087
```

We remove the missing values, compute the mean and variance:

```r
sum(is.na(incub))
```

```
## [1] 2087
```

```r
incub <- incub[!is.na(incub)]
mean(incub)
```

```
## [1] 9.921336
```

```r
var(incub)
```

```
## [1] 59.89197
```

Get the 95 percentile:

```r
t <- quantile(incub, .95)
t
```

```
## 95% 
##  25
```

Quarantine should last for at least **25 days**.

Plot using basic graphics:

```r
hist(incub, col="grey", border="white", xlab = "Days to show symptoms", main = "Distribution of incubation period")
abline(v = t, col = "red", lty = 2)
```

![plot of chunk plot](figure/plot-1.png)

Version using *ggplot2*:


```r
ggplot(data.frame(incubation = incub), aes(x=incub)) + 
  geom_histogram() +
  labs("Days to show symptoms", main = "Distribution of incubation period")
```

```
## Error in eval(expr, envir, enclos): could not find function "ggplot"
```

