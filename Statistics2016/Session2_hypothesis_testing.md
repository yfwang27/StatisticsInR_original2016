Session2: Hypothesis Testing and ANOVA
========================================================
author: MRC Clinical Sciences Centre (http://mrccsc.github.io/)
date: 12/July/2016
width: 1440
height: 1100
autosize: true
font-import: <link href='http://fonts.googleapis.com/css?family=Slabo+27px' rel='stylesheet' type='text/css'>
font-family: 'Slabo 27px', serif;
css:style.css

Hypothesis testing and ANOVA
========================================================

- SD (standard deviation) and SE (standard error; standard error of sample mean)

- Confidence Interval (CI)

- Hypothesis testing

 -- parametric test:e.g. t-test

 -- non-parametric test: e.g. Wilcoxon test; chi-square test and Fisher's exact test

- Analysis of Variance (ANOVA)


SD (standard deviation) and SE (standard error) (1/)
========================================================
![alt text](imgs/stat_sampling.png)


SD (standard deviation) and SE (standard error) (2/)
========================================================


Confidence Interval (CI)
========================================================
- point 1
- point 2

Statistical tests
========================================================

On top of descriptive statistics, R has several statistical tests covering a range of problems and data types.

Some common tests include:

- var.test() - Comparing 2 variances (Fisher's F test)
- t.test() - Comparing 2 sample means with normal errors (Student's t-test)
- binom.test() - Performs an exact test of a simple null hypothesis about the probability of success in a Bernoulli experiment.
- wilcox.test() - Comparing 2 means with non-normal errors (Wilcoxon's rank test)
- fisher.test() - Testing for independence of 2 variables in a contingency table (Fisher's exact test)


Hypothesis test in propotion
========================================================

[example: EU referendum reuslt 2016]


Hypothesis test in propotion
========================================================

[example: EU referendum reuslt 2016]
[data/EU-referendum-result-data.csv]
Is the leave vote more than the 50%?

```r
vote.leave=17410742
vote.remain=16141241
total.vote=vote.leave+vote.remain
```

Hypothesis test in propotion
========================================================

$$H_0:\text{ Leave vote is more than 50%}
\\
H_a:\text{ Leave vote is no more than 50%}$$

```r
binom.test(vote.leave, total.vote, p=0.5, alternative = "less")
```

```

	Exact binomial test

data:  vote.leave and total.vote
number of successes = 17411000, number of trials = 33552000,
p-value = 1
alternative hypothesis: true probability of success is less than 0.5
95 percent confidence interval:
 0.0000000 0.5190603
sample estimates:
probability of success 
             0.5189184 
```

Hypothesis test in mean
========================================================
t-test

t-test example - Load data (1/2)
========================================================

We use "PlantGrowth" as example

```r
PlantGrowth
```

```
   weight group
1    4.17  ctrl
2    5.58  ctrl
3    5.18  ctrl
4    6.11  ctrl
5    4.50  ctrl
6    4.61  ctrl
7    5.17  ctrl
8    4.53  ctrl
9    5.33  ctrl
10   5.14  ctrl
11   4.81  trt1
12   4.17  trt1
13   4.41  trt1
14   3.59  trt1
15   5.87  trt1
16   3.83  trt1
17   6.03  trt1
18   4.89  trt1
19   4.32  trt1
20   4.69  trt1
21   6.31  trt2
22   5.12  trt2
23   5.54  trt2
24   5.50  trt2
25   5.37  trt2
26   5.29  trt2
27   4.92  trt2
28   6.15  trt2
29   5.80  trt2
30   5.26  trt2
```
***
data visualisation

![plot of chunk unnamed-chunk-4](Session2_hypothesis_testing-figure/unnamed-chunk-4-1.png)


t-test example - Load data (2/2)
========================================================

Convert the input data into the proper format

```r
#install.packages("tidyr")
library("tidyr")
```

```r
PlantGrowthforwide<-PlantGrowth
PlantGrowthforwide$replicate<-rep(c(1:10),3)
PlantGrowth_wide<-spread(PlantGrowthforwide, group, weight)
head(PlantGrowth_wide)
```

```
  replicate ctrl trt1 trt2
1         1 4.17 4.81 6.31
2         2 5.58 4.17 5.12
3         3 5.18 4.41 5.54
4         4 6.11 3.59 5.50
5         5 4.50 5.87 5.37
6         6 4.61 3.83 5.29
```



t-test example - Calculating variance
========================================================

First we can specify the columns of interest using $ and calculate their variance using var().

```r
var(PlantGrowth_wide$ctrl)
```

```
[1] 0.3399956
```

```r
var(PlantGrowth_wide$trt1)
```

```
[1] 0.6299211
```

```r
var(PlantGrowth_wide$trt2)
```

```
[1] 0.1958711
```

t-test example - Comparing variance
========================================================

Now we can test for any differences in variances between ctrl and trt1 and ctrl and trt2 with an F-test using the var.test() function.

```r
var.test(PlantGrowth_wide$ctrl,
         PlantGrowth_wide$trt1)
```

```

	F test to compare two variances

data:  PlantGrowth_wide$ctrl and PlantGrowth_wide$trt1
F = 0.53974, num df = 9, denom df = 9, p-value = 0.3719
alternative hypothesis: true ratio of variances is not equal to 1
95 percent confidence interval:
 0.1340645 2.1730025
sample estimates:
ratio of variances 
         0.5397431 
```
***

```r
var.test(PlantGrowth_wide$ctrl,
         PlantGrowth_wide$trt2)
```

```

	F test to compare two variances

data:  PlantGrowth_wide$ctrl and PlantGrowth_wide$trt2
F = 1.7358, num df = 9, denom df = 9, p-value = 0.4239
alternative hypothesis: true ratio of variances is not equal to 1
95 percent confidence interval:
 0.4311513 6.9883717
sample estimates:
ratio of variances 
          1.735813 
```

R objects (s3 and s4)
========================================================
Left:30% The data type holding the result var.test() is a little more complex than the data types we have looked.

In R, special objects (S3 or S4 objects) can be created which have methods associated to them. The result from var.test is an object of class htest.

Since we have not come across this before, in order to discover its structure we can use the str() function with the object of interest as the argument.

```r
result <- var.test(PlantGrowth_wide$ctrl, PlantGrowth_wide$trt1)
str(result)
```

```
List of 9
 $ statistic  : Named num 0.54
  ..- attr(*, "names")= chr "F"
 $ parameter  : Named int [1:2] 9 9
  ..- attr(*, "names")= chr [1:2] "num df" "denom df"
 $ p.value    : num 0.372
 $ conf.int   : atomic [1:2] 0.134 2.173
  ..- attr(*, "conf.level")= num 0.95
 $ estimate   : Named num 0.54
  ..- attr(*, "names")= chr "ratio of variances"
 $ null.value : Named num 1
  ..- attr(*, "names")= chr "ratio of variances"
 $ alternative: chr "two.sided"
 $ method     : chr "F test to compare two variances"
 $ data.name  : chr "PlantGrowth_wide$ctrl and PlantGrowth_wide$trt1"
 - attr(*, "class")= chr "htest"
```

R objects (s3 and s4)
========================================================
Now we know the structure and class of the htest object we can access the slots containing information we want just as with a named list.

The p-value

```r
result$p.value
```

```
[1] 0.3718963
```
The statistic

```r
result$statistic
```

```
        F 
0.5397431 
```
The data used in function call

```r
result$data.name
```

```
[1] "PlantGrowth_wide$ctrl and PlantGrowth_wide$trt1"
```

t-test example - Equal Variance
========================================================
We have ascertained that ctrl and trt1 have similar variances. We can therefore perform a standard t-test to assess the significance of differences between these groups.

```r
Result <- t.test(PlantGrowth_wide$ctrl,PlantGrowth_wide$trt1,alternative ="two.sided", var.equal = T)
Result
```

```

	Two Sample t-test

data:  PlantGrowth_wide$ctrl and PlantGrowth_wide$trt1
t = 1.1913, df = 18, p-value = 0.249
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -0.2833003  1.0253003
sample estimates:
mean of x mean of y 
    5.032     4.661 
```

t-test example - Unequal Variance
========================================================
To compare groups of unequal variance then the var.equal argument may be set to FALSE (which is the default).

note: [see exercise]

T-test example. Specifying a formula
========================================================
The same result to that shown could be achieved by specifying a formula for the comparison. Here we wish to compare ctrl versus trt1 so we could simply specify the formula and the data to be used.

```r
data4formula<-PlantGrowth[PlantGrowth$group!="trt2",]
result_formula <- t.test(weight~group,data4formula,alternative ="two.sided", var.equal = T)
result_formula
```

```

	Two Sample t-test

data:  weight by group
t = 1.1913, df = 18, p-value = 0.249
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -0.2833003  1.0253003
sample estimates:
mean in group ctrl mean in group trt1 
             5.032              4.661 
```


ANOVA
========================================================

Compute analysis of variance (or deviance) tables for one or more fitted model objects

- lm()
- anova()


ANOVA - use the anova() function
========================================================

```r
PG.lm<-lm(formula = weight ~ group,
          data = PlantGrowth)
PG.lm
```

```

Call:
lm(formula = weight ~ group, data = PlantGrowth)

Coefficients:
(Intercept)    grouptrt1    grouptrt2  
      5.032       -0.371        0.494  
```
***

```r
PG.anova<-anova(PG.lm)
PG.anova
```

```
Analysis of Variance Table

Response: weight
          Df  Sum Sq Mean Sq F value  Pr(>F)  
group      2  3.7663  1.8832  4.8461 0.01591 *
Residuals 27 10.4921  0.3886                  
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


Wilcoxon test
========================================================

For more details on authoring R presentations click the
**Help** button on the toolbar.

- Bullet 1
- Bullet 2
- Bullet 3

First Slide
========================================================

For more details on authoring R presentations click the
**Help** button on the toolbar.

- Bullet 1
- Bullet 2
- Bullet 3



