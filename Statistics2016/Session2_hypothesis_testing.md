Session2: Hypothesis Test
========================================================
author: MRC CSC Bioinformatics Core
date: 12/July/2016

Hypothesis test
========================================================

- SD (standard deviation) and SE (standard error; standard error of sample mean)

- (CI) Confidence Interval

- parametric test:

e.g. t-test

- non-parametric test:

e.g. Wilcoxon test

- chi-square test; Fisher's exact test


Test of significance in ratio
========================================================

[example: EU referendum reuslt 2016]


Hypothesis test in propotion
========================================================

[example: EU referendum reuslt 2016]
[data/EU-referendum-result-data.csv]

```r
vote.leave=17410742
vote.remain=16141241
total.vote=vote.leave+vote.remain
prop.test(vote.leave, 33551983, p=0.5, alternative = "less")
```

```

	1-sample proportions test with continuity correction

data:  vote.leave out of 33551983, null probability 0.5
X-squared = 48034, df = 1, p-value = 1
alternative hypothesis: true p is less than 0.5
95 percent confidence interval:
 0.0000000 0.5190603
sample estimates:
        p 
0.5189184 
```

```r
prop.test(vote.remain, 33551983, p=0.5, alternative = "less")
```

```

	1-sample proportions test with continuity correction

data:  vote.remain out of 33551983, null probability 0.5
X-squared = 48034, df = 1, p-value < 2.2e-16
alternative hypothesis: true p is less than 0.5
95 percent confidence interval:
 0.0000000 0.4812235
sample estimates:
        p 
0.4810816 
```


Statistical tests
========================================================

On top of descriptive statistics, R has several statistical tests covering a range of problems and data types.

Some common tests include:

- var.test() - Comparing 2 variances (Fisher's F test)
- t.test() - Comparing 2 sample means with normal errors (Student's t-test)
- wilcox.test() - Comparing 2 means with non-normal errors (Wilcoxon's rank test)
- fisher.test() - Testing for independence of 2 variables in a contingency table (Fisher's exact test)

Confidence Interval (CI)
========================================================

For more details on authoring R presentations click the
**Help** button on the toolbar.

- Bullet 1
- Bullet 2
- Bullet 3

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

t-test example - Load data (2/2)
========================================================

Convert the input data into the proper format






















```
Error in library("tidyr") : there is no package called 'tidyr'
```
