---
title       : Why a good model does not necessarily have high $R^2$
subtitle    : Understand it in less than 10 mins
author      : liangcc
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
github      :
  user:"SatoshiLiang"
  repo:"Rsq"
---

## The Argument

1. Most of the time, your data contains a lot of unpredictable noise, so the R squared value of your best model is inherently limited.
2. You can add irrelevant feature to your model to improve (meaningless) R squared value.

--- .class #id 

## Why not a experiment

Suppose this is how your data generated:
- your independent variable $x$ is a number between 1 and 100
- your dependent variable $y$ is a (linear) function of $x$ with hairy noise $\epsilon \sim \mathcal{N}(0,1000)$. For each $i$ in $x$ we have $$y_i=3x_i+5+ \epsilon_i$$


```r
# with large enough amount of sample
N = 10000
x = seq(0, 100, length.out = N)  #  num [1:10000] 0 0.01 0.02 0.03 0.04 ...99.98 99.99 100.00

# This is the 'Data Generating Process'. Note that the large sigma makes the
# noise noisy
y = 10 * x + 5 + rnorm(N, 0, 1000)
```

---
## Why not a experiment


```r
plot(x, y)  # It's hard to tell what y could be given x, but the y dots appearently go upward as x increases.
```

<img src="assets/fig/unnamed-chunk-2.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />


--- 


```r
model= lm(y~x)
summary(model) # The model has significant x but poor R^2 fit.

# Call:
# lm(formula = y ~ x)
# 
# Residuals:
#     Min      1Q  Median      3Q     Max 
# -3541.2  -667.0    10.2   667.4  4218.0 
# 
# Coefficients:
#             Estimate Std. Error t value Pr(>|t|)    
# (Intercept) -13.8934    19.8449   -0.70    0.484    
# x            10.1551     0.3437   29.55   <2e-16 ***
# ---
# Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
# 
# Residual standard error: 992.3 on 9998 degrees of freedom
# Multiple R-squared:  0.0803,  Adjusted R-squared:  0.08021 
# F-statistic: 872.9 on 1 and 9998 DF,  p-value: < 2.2e-16
```

---
## For the second argument

We will introduce another irrelevant feature $z$, a pure noise, to fit the data.

```r
z = rnorm(N, 0, 30)
model2 = lm(y ~ x + z)
```



---
Surprisingly, The multiple R-squared is improved! 

```r
summary(model2)
# Call:
# lm(formula = y ~ x + z)
# 
# Residuals:
#     Min      1Q  Median      3Q     Max 
# -3520.5  -665.6    11.9   667.6  4214.7 
# 
# Coefficients:
#             Estimate Std. Error t value Pr(>|t|)    
# (Intercept) -13.9364    19.8446  -0.702    0.483    
# x            10.1546     0.3437  29.544   <2e-16 ***
# z            -0.3879     0.3309  -1.172    0.241    
# ---
# Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
# 
# Residual standard error: 992.3 on 9997 degrees of freedom
# Multiple R-squared:  0.08042,  Adjusted R-squared:  0.08024 
# F-statistic: 437.2 on 2 and 9997 DF,  p-value: < 2.2e-16
```


