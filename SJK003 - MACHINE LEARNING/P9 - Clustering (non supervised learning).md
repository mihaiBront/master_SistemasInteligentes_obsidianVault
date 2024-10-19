# 9.1 Context and intro

Unsupervised learning can be applied if:
- there are no labeled data 
- possibly there are no predefined classes

The aim is ti divide the set of objects into a number of groups such that objects in the same group are more similar (closer in the dimensional space)

A cluster is, therefore, a collection of objects that are similar between them and different from other classes will be a cluster

Result quality will depend on:
- Clustering algorithm
- Quality of the available data set
- Similarity measure selected for object comparation (similarity measures are distance-based classifiers)

We need to define:
- How many clusters there are; it can be a subjective measure:
	![[Pasted image 20241016163743.png]]

Applications:

![[Pasted image 20241016163905.png]]

Clustering algorithms:

![[Pasted image 20241016164156.png]]

# 9.2 Partitioning algorithms

About partitioning algoritms:

- these algorithms use prototypes (central points or centroids) to characterize clusters and assign each object to the cluster whose centroid is the closest
- they require prior knowledge about the number of clusters (classes, C) to divide the data
- the best known algorithm and one of the most widely used within this group is C-means (some authors call it K-means)

Drawback: Number of classes has to be indicated by the user

## 9.2.1 C-Means // K-Means

1. it partitions the data points into C clusters based upon a similarity measure -> Define $C$ centroids randomly.
2. the object that is closest to the cluster centroid is assigned to that cluster -> $C$ classes
3. after an iteration, it recalculates the centroids of those clusters, and the process continues until a predefined number of iterations is reached or the cluster centroids do not change after an iteration (for N iterations, or until the refinement of the centroids is precise enough) -> Recalculate centroids according to its samples
4. hyperparameters of the algorithm: 
	- initial values of clusters 
	- distance measures 
	- number of clusters (C)

```
Randomly select C cluster centroids repeat

	- Calculate the distance between each object and each cluster centroid 

	- Assign each object to the nearest cluster centroid 

	- Calculate the new centroid of each cluster 
	
until the centroids stop moving (i.e. they do not change their positions)
```

1.  the centroid of a cluster corresponds to the mean (or the central point) of the cluster 
2. random initialization of centroids at the start of the algorithm may lead to different clusters when running the algorithm multiple times 
3. it computes the dissimilarity between data points using the Euclidean distance metric 
4. time complexity is linear $O(n)$

![[Pasted image 20241016165323.png]]

**VARIATIONS:**

- C-medians -> Median instead of mean; Manhattan distance instead of euclidean; less sensitive to outliers in data set
- C-medioids -> Centroids for start are taken as samples, not just points on the plain
- C-modes -> Used with categorical (non-numeric) data

**GETTING THE OPTIMAL VALUE OF C**

A fundamental step for any unsupervised algorithm is to determine the optimal number of clusters C into which the data should be clustered 

The Elbow method is one of the most popular heuristics to determine this optimal value of C 

A better method than Elbow is the Silhouette method, which can be used to study the separation distance between the resulting clusters

### 9.2.1.1 Elbow Method

It consists of plotting the sum of squared errors (SSE) as a function of the number of clusters C and picking the elbow (inflection point) of the curve as the optimal value of C. Then:

1. Clustering for a range of values for C
2. After each clustering, compute Sum Sqared Error of each cluster
3. Plot SSE for each value of C -> **Elbow Plot**
	SSE measures the average distance from a sample to its centroid
4. Look at the elbow plot and find the value of C where the SSE begins to decrease linearly. That value is the optimal number of clusters

For each cluster:
- For each object in the cluster:
	- Calculate the distance from the object to the centroid
	- Square the distance
- Compute the sum of squared distances
Add up total distances computed for each cluster to give the sum of squared error (SSE)

![[Pasted image 20241016170425.png]]
With an increase in C, SSE decreases
Inflection point is the point from where the decreasing rate smoothens -> optimal value

>[!danger] What if the SSE decreases linearly?
>![[Pasted image 20241016170620.png]]
>
>No obvious elbow...
>
>Other more elaborated methods...**silhouette**

### 9.2.1.2 The silhouette method:

![[Pasted image 20241016170814.png]]
![[Pasted image 20241016170931.png]]

Sequence:
- For each object in data set:
	- Compute average distance from object to all other objects in the same cluster ($a(i)$)
	- Compute the average distance from object to all objects in the closest cluster (not own) ($b(i)$)
	- Complete the silhouette coefficient as:
		$$
		s(i) = \frac{b(i) - a(i)}{max(b(i), a(i))}
		$$
- Average silhouette coefficients to get silhouette score
- Similar to the elbow method, pick a range of candidate values for C, choose highest silhouette score value:
	![[Pasted image 20241016171420.png]]

**ALTERNATIVE FOR C SELECTION:**
Another way to determine the optimal value of C consists of analyzing a silhouette plot (histogram for each class): 
- the x-axis displays the silhouette coefficients (sorted) 
- the y-axis displays the labels of each cluster 

If all the cluster plots are beyond the average silhouette score, have mostly uniform thickness, and do not have wide fluctuation in size, the number of clusters used is optimal 

The number of clusters is suboptimal if: 
- the plot for a cluster falls below the average coefficient, or
- there are wide fluctuations in the size and thickness of the cluster plots

>[!Example] 
>![[Pasted image 20241016172014.png]]
>![[Pasted image 20241016172029.png]]
>![[Pasted image 20241016172042.png]]
>![[Pasted image 20241016172119.png]]
>![[Pasted image 20241016172137.png]]
>![[Pasted image 20241016172148.png]]
>
>Best case is C=4

