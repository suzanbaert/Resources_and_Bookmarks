# Tricks for unsupervised learning


## Kmeans clustering

In R:
Base function `kmeans()`

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


## HCA

In R:

+ If needed: normalize the data by `scale(data_matrix)`
+ Can use apply to check all means/sd of each column: `apply(data_matrix, 2, sd)`

+ Use `dist()` to get a distance data_matrix
+ Use `hclust()` to get the hierarchical clustering, default is complete linkage
+ Dendogram: `plot(hclust_output)`. To add the cluster line `abline(h=7, col="red")`
+ Getting the cluster info: `cutree()` either by height `h=7` or by number of clusters `k=3`.

```
#AHC
data_matrix <- scale(data_matrix_orig)
hclust_output <- hclust(dist(data_matrix))

#plotting
plot(hclust_output)
abline(h=7, col="red")

#getting the cluster
cutree(hclust_output, k=3)
```


## PCA

+ PCA: `prcomp(data, scale = TRUE, center = TRUE/FALSE)`.
+ Plotting: biplot(PCA_output)
+ New loadings of first 6 components: `PCA_output$x[, 1:6]`
+ Checking number of PCs necessary: `summary(PCA_output)` or scree plot:

```
# Variability of each principal component: pr.var
pr.var <- pr.out$sdev^2

# Variance explained by each principal component: pve
pr.var / sum(pr.var)

# Plot variance explained for each principal component
plot(pve, xlab = "Principal Component",
     ylab = "Proportion of Variance Explained",
     ylim = c(0, 1), type = "b")
```


From [Julia Silge's talk on Rstudio conference 2018](https://www.rstudio.com/resources/videos/understanding-pca-using-shiny-and-stack-overflow-data/):

+ Started with dataframe with AccountId, Tags and Value (= proportion of visits to that tag by this user)
+ Transform into sparse matrix. Function from tidytext, but it'ssomething we do often in tidytext
+ Scale data
+ PCA made for handling sparse matrices via irlba package, so faster than base PCA
+ Going back to tidy dataframe


```
sparse_tag_matrix <- tidytext::cast_sparse(user_tag_counts, AccountId, Tag, Percent)
tags_scaled <- scale(sparse_tag_matrix)
tags_pca <- irlba::prcomp_irlba(tags_scaled, n=64)
tidied_pca <- bind_cols(Tag = colnames(tags_scaled), tidy(tags_pca$rotation))
```
