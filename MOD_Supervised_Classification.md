# Supervised learning: Classification

**Goal:** training the machine to learn from prior examples. If numeric, it's regression. If binary/categorical: classification.

<br><hr>

### 1. Logistic regression

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

### 2. KNN

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

### 3. Naive Bayes

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



### 4. Decision tree

**How does it work**

+ Creates a set of ifelse decisions to divide the general set into partitions with the most homogeneous outcome ("purity").
+ Tree can be prepruned to decide a depth level, or minimum number of observations at a node
+ Tree can be postpruned as well
+ Measure: accuracy, confusion matrix (caret::confusionMatrix), auc

Advantages: ease of use (no data prep), model interpretability and fast.
Disadvantages: issue to overfit, single tree can have high variance

**Data preparation**

+ No prep needed. No Standardization or normalization needed.
+ Can deal with numeric predictors or categorical (no dummy coding needed)
+ Can handle NA. Two options when arrive at a split with NA data: i) random choose left or right, or ii) go down both paths and average final outcome


**In R:**

Use package rpart, which stands for recursive partitioning - the process used to train decision trees

```
library(rpart)
model <- rpart(y ~ ., data = df, method = "class")
model <- rpart(y ~ ., data = df, method = "class", control = rpart.control(maxdepth = __, minsplit = ___)) #prepruned

predict(model, testdata, type = "class") #output classification
predict(model, testdata, type = "prob") #output probability for each class, which is needed for auc

#visualization
library(rpart.plot)
rpart.plot(model)

#postpruning
plotcp(model) #plot error rate vs complexity
opt_cp_index <- which.min(model$cptable[ , "xerror"]) #find index of lowest xerror in cptable
model_pruned <- prune(model, cp = model$cptable[opt_cp_index, "CP"])
```

Can add different decision criteria and then compare the classification error (lower is better obviously)
```
model1 <- rpart(y ~ ., data = df, method = "class", parms = list(split = "gini"))
model2 <- rpart(y ~ ., data = df, method = "class", parms = list(split = "information"))

pred1 <- predict(object = model1, newdata = testdf, type = "class")
pred2 <- predict(object = model2, newdata = testdf, type = "class")   

ce(actual = df$y, predicted = pred1)
ce(actual = df$y, predicted = pred2)  

```

Or use auc to compare versus other models:
```
pred_for_auc <- predict(model, testdata, type = "prob")
Metrics::auc(actual = testset$y, predicted <- pred_for_auc[ , 2]) #predicted values needs yes column

```
