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


<br><hr>

### 2. Naive Bayes

**How does it work**

+ Bayesian methods work by estimating probability based on historic data.
+ Joint probability: P(A|B) = P(A & B) / P(B)
+ If more predictors: assumes events are independent and does not take into account all overlaps. P = P(A|B) * P(A|C)
+ Pitfall: Probability of an event that has never happened before is 0 because it has never been observed before. Laplace correction can be used to add at least 1 event to all intersections to add some probability even if not occurred before.



**Data preparation**

+ Works best with set of categories
+ Numeric -> use binning to get to ranges or percentiles
+ Test -> use bag of words to get to a frequency table



**In R:**

```
model <- naivebayes::naive_bayes(y ~ x, data = df)
predict(model, newdata)

#to get probabilities as well
predict(model, newdata, type = "prob")

#model with laplace correction
model <- naivebayes::naive_bayes(y ~ x, data = df, laplace = 1)
```

<br><hr>



### 3. Logistic regression

**How does it work**

+ Binary dependent variable, numeric independent variables
+ Fit of logistic function through with probability between 0 and 1
+ Fit of the model: ROC curves (pct of positive outcome vs pct of other outcomes) and AUC (area under the curve)



**Data preparation**
+ All predictors must be numeric. Dummify if needed.
+ If using `glm()`, dummy coding will automatically happen when variable is a factor.
+ NA: If categorial, put NA as category, if numeric mean or other imputation, but add a new binary variable to keep track of missing rows and add them to the model.



**In R:**

```
model <- glm(y ~ x1 + x2, data = df, family = "binomial")
model <- glm(y ~ x1 * x2, data = df, family = "binomial") #for interaction

#predict new data
pred <- predict(model, testset, type = "response")

#recode variables, set higher or lower to make model more or less aggressive
pred <- ifelse(pred > 0.5, 1, 0)

#ROC and AUC
library(pROC)
ROC <- roc(df$actual, pred)
plot(ROC, col = "blue")
auc(ROC)
```


Automatic feature selection: stepwise regression

+ Backward: starts with all, removes predictor with lowest impact on the model
+ Forward: stars with none, adds predictor with highest impact
+ Not always get to the best model, as use no common sense or business knowledge. Model may massive over or underestimate predictors.

```
nullmodel <- glm(y ~ 1, data = df, family = "binomial")
fullmodel <- glm(y ~ ., data = df, family = "binomial")
stepmodel <- step(nullmodel, scope = list(lower = nullmodel, upper = fullmodel), direction = "forward")
```



<br><hr>



### 4. Decision (classification) trees

**How does it work**

+ Creates a set of ifelse decisions to divide the general set into partitions with the most homogeneous outcome.
+ Issue: diagonal splits are not possible, so tree can get complex very quickly
+ Tree can be prepruned to decide a depth level, or minimum number of observations at a node
+ Tree can be postpruned as well



**In R:**

```
library(rpart)
model <- rpart(y ~ ., data = df, method = "class")
model <- rpart(y ~ ., data = df, method = "class", control = rpart.control(maxdepth = __, minsplit = ___)) #prepruned

predict(model, testdata, type = "class")

#visualization
library(rpart.plot)
rpart.plot(model)

#postpruning
plotcp(model) #plot error rate vs complexity
model_pruned <- prune(model, cp = 0.2)
```
