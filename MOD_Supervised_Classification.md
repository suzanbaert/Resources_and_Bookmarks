# Supervised learning: Classification

**Goal:** training the machine to learn from prior examples, in this case: categorizing new data.


<br><hr>

### 1. KNN

**How does it work**

+ Given a set of training data and a new datapoint, K Nearest Neighbours looks for k most similar examples to classify the new data.
+ If k = 1: new data will be classified like the closest neighbour. If k =5, it will find 5 closest cases and majority rules. If a tie, the classification is decided at random. A larger k is not always better: small k makes small neighbourhoods and might pick up more subtle patterns (however those subtle patterns can be noise).
+ Deciding on k: rule of thumb k = sqrt(n), or test several k's and compare accuracy of test set.
+ Measured in terms of Euclidean distance (-> requires numeric values)


**Data preparation**

+ Assumes numerical. Dummify if needed.
+ Normalize data.


**In R:**

```
library(class)
pred <- knn(train, test, cl = labels, k = 1, prob = TRUE) #prob = TRUE shows neighbour votes

#confusion matrix
table(pred, test$value)

#accuracy
mean(pred == test$values)
```
