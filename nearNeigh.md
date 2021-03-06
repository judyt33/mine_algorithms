KNN
========================================================

## In class exercise #34.

Use knn() to fit the 1NN classifier to the last column of the sonar traiing data. 


```r
setwd("./data")
##download.file("nice redirects google","sonar_train.csv", method="curl")
##download.file("https://sites.google.com/site/stats202/data/sonar_train.csv",
##              "sonar_test.csv",method="curl")

train<-read.csv("sonar_train.csv",header=F)
test<-read.csv("sonar_test.csv",header=F)

setwd("../")
```

Load libraries. 


```r
library(class)
```

Using default values. 


```r
## set the y var as factor for class prediction.
y<-as.factor(train[,61])
## set the remaining colums to be test data.
x<-train[,1:60]
## train model to predict y using all x.
fit<-knn(train=x,test=x,cl=y,k=1)
## Note: both train and test parms use the same, in this case, train data set. This 
## gives the training misclass error.
```

Assess fit on training data. (Really silly b/c you're using the training data to  predict itself. Further, w/ k=1 it's not doing any real fitting.)


```r
fit
```

```
##   [1] -1 1  -1 1  1  1  -1 -1 1  -1 -1 1  1  -1 -1 1  -1 1  1  1  1  -1 -1
##  [24] 1  1  1  -1 1  1  1  -1 1  1  1  -1 1  1  -1 -1 1  -1 -1 1  1  -1 -1
##  [47] 1  -1 1  1  -1 -1 1  -1 1  -1 -1 1  -1 1  1  1  1  -1 -1 -1 -1 1  1 
##  [70] 1  1  -1 1  -1 1  1  -1 -1 -1 1  1  -1 -1 1  -1 1  -1 -1 -1 -1 1  1 
##  [93] -1 1  -1 1  -1 -1 1  -1 -1 -1 1  -1 -1 -1 -1 -1 1  1  -1 -1 1  -1 1 
## [116] 1  1  -1 -1 -1 -1 -1 -1 1  -1 1  1  1  1  -1
## Levels: -1 1
```

```r
## fit give the predicted class labels of the training data.

1-sum(y==fit)/length(y)
```

```
## [1] 0
```

```r
## sum(y==fit) gives the number of times your prediction (fit) matched your actual
## class label (y). So, this gives the overall accuracy (1-that pct) or the overall 
## error rate. Zero in this case. 
```

Compute the misclass error on the training and test data. 


```r
y_test<-as.factor(test[,61])
x_test<-test[,1:60]

fit_test<-knn(x,x_test,y,k=1)

1-sum(y_test==fit_test)/length(y_test)
```

```
## [1] 0.2051
```

```r
## how often does my predicted test lables (fit) = my true test labels (y_test)
## 0.21
```

So, when k=1, train error = 0, test error = 0.21... slightly better than decision trees. Not bad w/ 60 dimension data. 

So, let's try using k=5.


```r
fit_test<-knn(x,x_test,y,k=5)

1-sum(y_test==fit_test)/length(y_test)
```

```
## [1] 0.2308
```

```r
## 0.23
```

Could be we're generalizing away from the signal. k=3.


```r
fit_test<-knn(x,x_test,y,k=3)

1-sum(y_test==fit_test)/length(y_test)
```

```
## [1] 0.1795
```

```r
## 0.18
```

k=3 looks like balance between over-fitting (k=1) and over-generalizing (k=5). Now would be a good time to bring in another test data set. 
