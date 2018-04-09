# Tricks for unsupervised learning


## Kmeans clustering

To quickly check the number of clusters (from DC [course](https://campus.datacamp.com/courses/unsupervised-learning-in-r/unsupervised-learning-in-r)):

```
# Initialize total within sum of squares error: wss
wss <- 0

# Look over 1 to 15 possible clusters
for (i in 1:15) {
  # Fit the model: km.out
  km.out <- kmeans(data_matrix, centers = i, nstart = 20, iter.max = 50)
  # Save the within cluster sum of squares
  wss[i] <- km.out$tot.withinss
}

# Produce a scree plot
plot(1:15, wss, type = "b", 
     xlab = "Number of Clusters", 
     ylab = "Within groups sum of squares")
```
