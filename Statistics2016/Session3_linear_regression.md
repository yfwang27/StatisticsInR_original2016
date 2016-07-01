Session3: Linear Regression
========================================================
author: MRC Clinical Sciences Centre (http://mrccsc.github.io/)
date: 12/July/2016
width: 1440
height: 1100
autosize: true
font-import: <link href='http://fonts.googleapis.com/css?family=Slabo+27px' rel='stylesheet' type='text/css'>
font-family: 'Slabo 27px', serif;
css:style.css

Outline
========================================================
- correlation

- linear regression


Dataset - use the "iris" data (1/3)
========================================================
We have used the "iris" data for the [Intermediate R course]. We are going to work on this data again for this session.

Dataset - use the "iris" data (2/3)
========================================================
Some basic checks

```r
class(iris)
```

```
[1] "data.frame"
```

```r
str(iris)
```

```
'data.frame':	150 obs. of  5 variables:
 $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```
***

```r
head(iris)
```

```
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
```

Dataset - use the "iris" data (3/3)
========================================================
[*intermediate R; add a link to the intermediate R]

```r
# library("dplyr")
# tbl_df()
```
Correlation (1/8)
=========================================================

A common task in statistical analysis is to investigate the relationship between pairs of numeric vectors.

This can be done by identifying the correlation between numeric vectors using the **cor()** function in R.

In this example we use cor() to identify the Pearson correlation between two variables.  The **method** argument may be set to make use of different correlation methods.

- Perfectly posively correlated vectors will return 1
- Perfectly negatively correlated vectors will return -1
- Vectors showing no or little correlation will be close to 0.

Correlation (2/8)
========================================================
- pearson

- spearman

Correlation between vectors (3/8)
=========================================================


```r
x <- rnorm(100,10,2)
z <- rnorm(100,10,2)
y <- x
cor(x,y) #
```

```
[1] 1
```

```r
cor(x,-y)
```

```
[1] -1
```

```r
cor(x,z)
```

```
[1] -0.04009353
```
***
![plot of chunk unnamed-chunk-5](Session3_linear_regression-figure/unnamed-chunk-5-1.png)


Correlation over a matrix (4/8)
=========================================================
left: 70%
Often we wish to apply correlation analysis to all columns or rows in a matrix in a pair-wise manner. To do this in R, we can simply pass the **cor()** function a single argument of the numeric matrix of interest. The **cor()** function will then perform all pair-wise correlations between columns.

```r
iris4cor<-iris[,1:4]
colnames(iris4cor)<-gsub("(.+)(\\.)(\\w{3})(.+)","\\1\\2\\3",colnames(iris4cor))
head(iris4cor)
```

```
  Sepal.Len Sepal.Wid Petal.Len Petal.Wid
1       5.1       3.5       1.4       0.2
2       4.9       3.0       1.4       0.2
3       4.7       3.2       1.3       0.2
4       4.6       3.1       1.5       0.2
5       5.0       3.6       1.4       0.2
6       5.4       3.9       1.7       0.4
```

Correlation over a matrix (5/8)
=========================================================

```r
cor(iris4cor)
```

```
           Sepal.Len  Sepal.Wid  Petal.Len  Petal.Wid
Sepal.Len  1.0000000 -0.1175698  0.8717538  0.8179411
Sepal.Wid -0.1175698  1.0000000 -0.4284401 -0.3661259
Petal.Len  0.8717538 -0.4284401  1.0000000  0.9628654
Petal.Wid  0.8179411 -0.3661259  0.9628654  1.0000000
```

Correlation (6/8)
========================================================
left: 70%

```r
cor(iris4cor)
```

```
           Sepal.Len  Sepal.Wid  Petal.Len  Petal.Wid
Sepal.Len  1.0000000 -0.1175698  0.8717538  0.8179411
Sepal.Wid -0.1175698  1.0000000 -0.4284401 -0.3661259
Petal.Len  0.8717538 -0.4284401  1.0000000  0.9628654
Petal.Wid  0.8179411 -0.3661259  0.9628654  1.0000000
```
***
<img src="Session3_linear_regression-figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="820px" />

Correlation (7/8)
========================================================

```r
pairs(iris4cor)
```

![plot of chunk unnamed-chunk-10](Session3_linear_regression-figure/unnamed-chunk-10-1.png)

Correlation (8/8)
========================================================
[probably find an example that requires the spearman method]


Regression and linear models (1/)
=========================================================

We have seen how we can find the correlation between two sets of variables using cor() function.

R also provides a comprehensive set of tools for regression analysis including the well used linear modeling function lm()

To fit a linear regression we use a similar set of arguments as passed to the t-test fuction in the previous slide.

Regression and linear models (2/)
=========================================================
Using the Petal.Width from iris data as example

We could like to use the current information to predict the width of a petal from Iris.versicolor
![alt text](imgs/Iris_versicolor.jpg)
***

```r
iris_versi<-iris[iris$Species=="versicolor",]

str(iris_versi)
```

```
'data.frame':	50 obs. of  5 variables:
 $ Sepal.Length: num  7 6.4 6.9 5.5 6.5 5.7 6.3 4.9 6.6 5.2 ...
 $ Sepal.Width : num  3.2 3.2 3.1 2.3 2.8 2.8 3.3 2.4 2.9 2.7 ...
 $ Petal.Length: num  4.7 4.5 4.9 4 4.6 4.5 4.7 3.3 4.6 3.9 ...
 $ Petal.Width : num  1.4 1.5 1.5 1.3 1.5 1.3 1.6 1 1.3 1.4 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 2 2 2 2 2 2 2 2 2 2 ...
```


Regression and linear models (3/)
=========================================================
Try to use the mean of total Petal.Length first

```r
head(iris_versi[,c("Petal.Length",
                   "Species")])
```

```
   Petal.Length    Species
51          4.7 versicolor
52          4.5 versicolor
53          4.9 versicolor
54          4.0 versicolor
55          4.6 versicolor
56          4.5 versicolor
```

```r
mean(iris_versi$Petal.Length)
```

```
[1] 4.26
```
***

```r
plot(iris_versi$Petal.Length, ylab="Petal Length of Iris.versicolor")
abline(h=mean(iris_versi$Petal.Length),
       col="forestgreen",lwd=3)
```

![plot of chunk unnamed-chunk-14](Session3_linear_regression-figure/unnamed-chunk-14-1.png)


Regression and linear models (4/)
=========================================================
Try to use the mean of total Petal.Length first

![plot of chunk unnamed-chunk-15](Session3_linear_regression-figure/unnamed-chunk-15-1.png)
***
In this case, the expected values is  $$ mean  = \bar{y} $$
- residuals (Error)
$$
  \begin{aligned}

  Error_i & = y_i - \bar{y}
  \\ \\
  \end{aligned}
$$



Regression and linear models (5/)
=========================================================
Zoom in [just see first 4 data points]

![plot of chunk unnamed-chunk-16](Session3_linear_regression-figure/unnamed-chunk-16-1.png)
***
In this case, the expected values is  $$ mean  = \bar{y} $$
- residuals (Error)
$$
  \begin{aligned}
  \\
  Error_i & = y_i - \bar{y}
  \end{aligned}
$$
- square of the residuals
$$
  \begin{aligned}
  Error_i^2  = (y_i - \bar{y})^2
  \end{aligned}
$$
- sum of the square of the residuals (SSE)
$$
  \begin{aligned}

  SSE  = \sum_{i=1}^{n}(y_i-\bar{y})^2
  \end{aligned}
$$


Regression and linear models (/)
=========================================================
Use the "iris_versi" Petal.Width to predict Petal.Length

![plot of chunk unnamed-chunk-17](Session3_linear_regression-figure/unnamed-chunk-17-1.png)
***
$$
  x = \text{independent or explanatory variable}
\\
  y = \text{dependent variable or }f(x)

$$

**$$f(x)  = b_0 + b_1x$$**

$$\begin{aligned}
  b_0 = intercept
  \\
  b_1 = slope
\end{aligned}$$



Regression and linear models (/)
=========================================================
The lm() function fits a linear regression to your data and provides useful information on the generated fit.

In the example below we fit a linear model using  lm() on the iris_versi dataset with Petal.Length (Y) as the dependent variable and Petal.Width (X) as the explanatory variable.

```r
lmResult<-lm(formula = Petal.Length ~ Petal.Width, data = iris_versi)
lmResult
```

```

Call:
lm(formula = Petal.Length ~ Petal.Width, data = iris_versi)

Coefficients:
(Intercept)  Petal.Width  
      1.781        1.869  
```


Interpreting output of lm()
=========================================================
As we have seen, printing the model result provides the intercept and slope of line.
To get some more information on the model we can use the summary() function

```r
summary(lmResult)
```

```

Call:
lm(formula = Petal.Length ~ Petal.Width, data = iris_versi)

Residuals:
    Min      1Q  Median      3Q     Max 
-0.8375 -0.1441 -0.0114  0.1984  0.6755 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   1.7813     0.2838   6.276 9.48e-08 ***
Petal.Width   1.8693     0.2117   8.828 1.27e-11 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2931 on 48 degrees of freedom
Multiple R-squared:  0.6188,	Adjusted R-squared:  0.6109 
F-statistic: 77.93 on 1 and 48 DF,  p-value: 1.272e-11
```

Regression and linear models - coefficients (/)
=========================================================

```r
lmResult$coefficients
```

```
(Intercept) Petal.Width 
   1.781275    1.869325 
```
From the $coefficients of object lmResult, we know the equation for the best fit is

**Y = 1.781275 + 1.869325 *X**

We can add the line of best fit using **abline()**
***
![plot of chunk unnamed-chunk-21](Session3_linear_regression-figure/unnamed-chunk-21-1.png)

Regression and linear models - residuals
=========================================================

The **residuals** are the difference between the predicted and actual values.
To retrieve the residuals we can access the slot or use the resid() function.


```r
> summary(resid(lmResult))
```

```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-0.8375 -0.1441 -0.0114  0.0000  0.1984  0.6755 
```

```r
> summary(lmResult$residual)
```

```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-0.8375 -0.1441 -0.0114  0.0000  0.1984  0.6755 
```
Ideally you would want your residuals to be normally distributed around 0.

Regression and linear models - R-squared
=========================================================
Left: 70%

```

Call:
lm(formula = Petal.Length ~ Petal.Width, data = iris_versi)

Residuals:
    Min      1Q  Median      3Q     Max 
-0.8375 -0.1441 -0.0114  0.1984  0.6755 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   1.7813     0.2838   6.276 9.48e-08 ***
Petal.Width   1.8693     0.2117   8.828 1.27e-11 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2931 on 48 degrees of freedom
Multiple R-squared:  0.6188,	Adjusted R-squared:  0.6109 
F-statistic: 77.93 on 1 and 48 DF,  p-value: 1.272e-11
```
***
- The **R-squared** value represents the proportion of variability in the response variable that is explained by the explanatory variable.

- A high **R-squared** here indicates that the line fits closely to the data.

Regression and linear models - F-statistics.
=========================================================
Left: 70%

```

Call:
lm(formula = Petal.Length ~ Petal.Width, data = iris_versi)

Residuals:
    Min      1Q  Median      3Q     Max 
-0.8375 -0.1441 -0.0114  0.1984  0.6755 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   1.7813     0.2838   6.276 9.48e-08 ***
Petal.Width   1.8693     0.2117   8.828 1.27e-11 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.2931 on 48 degrees of freedom
Multiple R-squared:  0.6188,	Adjusted R-squared:  0.6109 
F-statistic: 77.93 on 1 and 48 DF,  p-value: 1.272e-11
```
***
The results from linear models also provides a measure of significance for a variable not being relevant.

Statistics (Extra) - A fit line
=========================================================

![alt text](imgs/fittedline.png)

Statistics (Extra) - Calculating R-squared
=========================================================

![alt text](imgs/rsquared.png)

Statistics (Extra) - Calculating R-squared
=========================================================


```r
SSE <- sum(resid(lmResult)^2)
TSS <- sum((iris_versi$Petal.Length - mean(iris_versi$Petal.Length))^2)
1- SSE/TSS
```

```
[1] 0.6188467
```

```r
summary(lmResult)$r.squared
```

```
[1] 0.6188467
```

Statistics (Extra) - Calculating F-stat
=========================================================

![alt text](imgs/fstatistic.png)

Statistics (Extra) - Calculating F-stat
=========================================================


```r
> MSE <- mean(lmResult$residuals^2)
> RSS <- sum((predict(lmResult) - mean(iris_versi$Petal.Length))^2)
> 
> summary(lmResult)$fstatistic
```

```
   value    numdf    dendf 
77.93357  1.00000 48.00000 
```

