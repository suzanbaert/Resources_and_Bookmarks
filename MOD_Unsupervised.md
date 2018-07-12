# Unsupervised learning: Clustering

**Goal:** finding homogeneous groups in the data, measured by distance within observations.

**Types of data:**
+ Numeric: euclidean distance, in R: `dist()`
+ Binary: 1 - Jaccard index, in R: `dist(x, method = "binary")`
+ Categorical: change to set of binary variables, in R: `dummies::dummy.data.frame(x)`, then binary dist.

**Data cleanup**  
Not always required. `colMeans(x)` can be used for a quick check of means, `apply(x, 2, sd)` for standard deviations

+ Standardization: scale all data until mean = 0, sd = 1. In R: `scale()`
+ Normalization: all observations on a vector from 0 to 1. In R: `(x - min(x) / max(x) - min(x))`


<br><hr>

### 1. Kmeans

**Principle:**
+ Randomly assign k centroids
+ Assign each observation to the closest centroid
+ Recalculate centroid as the middle of the current cluster
+ Repeat until no observation changes cluster
+ Run the whole model multiple times to get the best model, set seed for reproducibility
+ The best model has the lowest total within cluster sum of squares (square of the distance of all observations to their center)

```
model <- kmeans(data, centers = 3, nstart = 10)
data$cluster <- model$clusters
plot(data, col = model$cluster)

```

<br>

**Choosing the number of clusters:**
Either based on context/business requirements or empirically from the data.

*Elbow plot*
Executing kmeans with different centers to understand the link of number of clusters to total within cluster SS.
```
#elbow plot
wss <- 0
for (i in 1:10) {
  kmeans_output <- kmeans(mean_agg_data[-1], centers=i, nstart=10)
  wss[i] <- kmeans_output$tot.withinss
}
plot(1:10, wss, type = "b")
```


*Silhouette analysis*
Can serve as model performance check, plus help decide on the number of clusters.  
Silhouette width: how similar is this observation to its cluster versus all other clusters. If 0, on the border. If clsoe to 1, well matched with the cluster. If close to -1, would fit better with neighbouring cluster.  
Silhouette width Si = (bi - ai) / max(bi, ai), with ai: average distance from observation i to all observations within its clusters, and bi: average distance to all observations in the nearest cluster.

```
model <- cluster::pam(data, k = 3)
model$silinfo$widths    #how well matched with the clusters
model$silinfo$avg.width   #effectiveness of the clustering in general

plot(silhouette(model))
```


To help decide on number of clusters:
```
#silhouette choice of clusters
silwidth <- 0
for (i in 2:10) {
  silh_output <- cluster::pam(mean_agg_data[-1], k=i)
  silwidth[i] <- silh_output$silinfo$avg.width
}
plot(1:10, silwidth, type = "b" )
```


<hr>

### Functions for copy pasting

```
kmeans_elbow <- function(data, kmax = 10) {
  wss <- 0
  for (k in 1:kmax) {
    kmeans_output <- kmeans(mean_agg_data[-1], centers=k, nstart=10)
    wss[k] <- kmeans_output$tot.withinss
  }
  plot(1:kmax, wss, type = "b", main = "Elbow plot")
}


silhouette_clusters <- function(data, kmax = 10) {
  silwidth <- 0
  for (k in 2:kmax) {
    silh_output <- cluster::pam(mean_agg_data[-1], k=k)
    silwidth[(k-1)] <- silh_output$silinfo$avg.width
  }
  plot(2:kmax, silwidth, type = "b", main = "Silhouette plot")
}
```

<br>
<hr>


### 2. Hierarchical clustering

**Principle:**
+ Each observation starts in its own cluster
+ Join the two closest clusters based on distance
+ Repeat until 1 cluster left

Measuring distance:
+ Complete linkage: join based on maximum distance (farthest neighbour)
+ Single linkage: join based on minimum distance (nearest neighbour) (not recommended, leads often to unbalanced trees)
+ Average linkage: join based on average distance

![](https://image.slidesharecdn.com/clusteranalysis-091117015845-phpapp01/95/cluster-analysis-7-728.jpg)

```
dist_data <- dist(data[-1])  #if column one contains label
model <- hclust(dist_data, method = "complete")

plot(model)
abline(h = 10, col = "red")

data$cluster <- cutree(model, k = 5)  #measured in number of clusters
data$cluster <- cutree(model, h = 10)  #measured in height (distance)

```
