RandomForest
========================================================

Load files / libraries


```r
library(randomForest)
```

```
## Warning: package 'randomForest' was built under R version 3.1.1
```

```
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```r
setwd("./data")

train<-read.csv("sonar_train.csv",header=F)
test<-read.csv("sonar_test.csv",header=F)

setwd("../")
```

Format training and test data sets and fit model

Note defaults for RF will:
- Take training data
- Take test data
- Run thru 500 trees, take majority vote
- Each tree is diff based only on attr avail for split decision (e.g., 60 in our set)
- You can control much of the detail about what to weight on split


```r
y_train<-as.factor(train[,61])
x_train<-train[,1:60]

fit<-randomForest(x_train, y_train, )

1-sum(y_train==predict(fit,x_train))/length(y_train)
```

```
## [1] 0
```

```r
fit
```

```
## 
## Call:
##  randomForest(x = x_train, y = y_train) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 7
## 
##         OOB estimate of  error rate: 18.46%
## Confusion matrix:
##    -1  1 class.error
## -1 57  9      0.1364
## 1  15 49      0.2344
```

Note:
- 0 class prediction errors on training data (good though not surprising given 500 trees)
- When you print fit, you see that RF tried 7 vars at each split.
- Got an error rate of 17.7%.
- Provides confustion matrix w/ class error calculated. 

Run against test data set and calc misclass error. 


```r
y_test<-as.factor(test[,61])
x_test<-test[,1:60]

1-sum(y_test==predict(fit,x_test))/length(y_test)
```

```
## [1] 0.1538
```

Note: only 12.8% test error. Did much better than prior methods w/ only 7 vars/split. Power of the 500 decision trees. 

So, why doesn't it to better than we would forecast using the pbinom() formula? 


```r
1-pbinom(50,60,0.7)
```

```
## [1] 0.005871
```

Problem is it's very difficult to generate completely independent classifiers. Trees built uisng the same methodology, though splits may be randomized, will still share a common set of errors do to the common approach. So, not comletely independent as pbinom() expects. 

If you use more features (higher % of total) on each split, you'll likely get higher interdependence. Tradeoff here as the fewer features you use, the less representative your model can be of your data. 
