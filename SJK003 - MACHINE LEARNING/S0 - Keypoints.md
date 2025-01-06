
# U8 - Multiclassifier

## Terms for referring

- Ensamble of classifiers
- Combining classifiers
- Decision Commite

## Parts

$$ MultiClassifier = Combiner = \sum_{i=0}^i (BaseClassifier_i) == \sum_{l=o}^L(D_i)$$
![[Pasted image 20250104215604.png]]

## Levels of the system

- Combination level: design different combiners; how the outputs of individual models or classifiers are aggregated. There are three primary strategies (voting, averaging, stacking...)
- Classifier level: Combinig the decisions or outputs of different combiners (ensable learning models; bagging/boosting, classifier fusions; decision for multiple classifiers are fused into a single decision, dynamic classifier selection)
- Data level: Data integration, augmentation, partitioning and resampling
- Feature level combination: Feature concentartion; features from different datasets concatenated from a larger feayire space, feature transformation (techniques like pca or autoencoders for combining features onto a reduced space), and feature selection -> most relevant features.


## Fusion vs selection:

### Fusion: 
Each ensamble base has knowledge about the whole feature space, using combiners like average or majority voting
- Competitive classifiers
- Ensemble approach
- Multiple topology

#### Majority vote

Decision rule: to chose the class most voted by the classifier:
- Unanimity -> Else, no class
- Simple majority -> 50% +1
- Plurarity -> Most votes

Weighted majority vote is a version of any of the later two, where each base classifier has a different weight to its vote. Giving more competent classifiers more power in the final decision
![[Pasted image 20250104224413.png]]
### Selection
Each ensamble member knows well a part of the feature space, and are responsible of that part

- cooperative classifiers
- modular approach
- hybrid topology

The feature space ($\mathbb{R}^k$) is divided into $K > 1$ selection regions, one for each classifier (usually $K=L$). Each region is associated with a classifier, which will be responsible of deciding on teh input objects in this space.

![[Pasted image 20250104224806.png]]



## Stacking; classifier level:

*"Learning different weak learners, combined by training a meta-model"*

Define two things in order to build the model:
- L-base learners
- Meta model that combines them 

>[!example] For example...
> ...select as weak learners a k-NN classifier, a decision tree and a SVM, and decide to learn a neural network as meta-model. Then, the neural network will take as inputs the outputs of our three weak learners and will learn to return final predictions based on it

Steps for stacking
1. Initialize parameters (L weak learners)
2. Split the data in two folds
3. Train weak learner with first fold -> Make predictions on the second fold
4. Train the meta model on the second fold using predictions made by the weak learners as inputs

## Bagging; data level:

Ensemble is made of classifiers built on bootstrap replicates of the training set Ttra = {x1, …, xn}; the classifier outputs are combined by the plurality vote

- Sample with replacement from the original Ttra to create L new sets
- All L base classifiers are of the same type
- The base classifier should be unstable (small changes in Ttra lead to large changes in output --> Very sensitive)
- Parallel alg -> Both in training and operation
![[Pasted image 20250104225522.png]]
Training phase

1. Initialize the parameters
	𝐷 = ∅, the ensemble 
	𝐿, the number of classifiers to train
1. For 𝑙 = 1, … , 𝐿
	Take a bootstrap sample 𝑆𝑙 from the original training set Ttra 
	Build a classifier 𝐷𝑙 using 𝑆𝑙 as the training set
	Add the classifier to the current ensemble, 𝐷 = 𝐷 ∪ 𝐷𝑙 
	Return 𝐷

Classification (regression) phase
1. Run 𝐷1 ⋯ 𝐷𝐿 on the input x
2. Assign x to the class with the maximum number of votes (simple majorityvoting, for classification) 
	Assign x with the average of the estimated values (simple average, for regression)

>[!example] Examples of bagging
>![[Pasted image 20250104225708.png]]


## Boosting; data level:

Developing the ensamble adding Bases ONE BY ONE; some ensambles have more say than others; they should be weighted.

Often the classifier D_i is made by taking the errors of D_(i+1) into account

Sequential algorithm
![[Pasted image 20250104225903.png]]
![[Pasted image 20250104225852.png]]
![[Pasted image 20250104225920.png]]
### AdaBoost


![[Pasted image 20250104225945.png]]
![[Pasted image 20250104230202.png]]
### Gradient boosting

It involves:
- Loss function to be optimized (mse for regression, or log loss for classifiers)
- Weak learner to make decisions, usually decision trees
- Additive model that minimizes loss function when adding trees -> **Gradient descent** is used to minimize loss

Extreme gradient boost -> XGBoost
- Efficient and effective implementation of gradient boost
- Scalable and can handle large datasets
- Trees are built in parallel :LiBookCheck:
- Early stopping when no improvement

Categorical Boosting -> CatBoost
- Work on heterogeneous data (categorical, numerical, data)
- Works well with small datasets
- Improved accuracy by reducing overfitting

## Random subspace; feature level

Ensemble made of random subsets of features -> Outputs are combined by popularity vote

- Attractive choice for high-dimensional problems, more features than training points
- It works best when discriminative information is dispersed across all features

![[Pasted image 20250104230736.png]]

# U9 - Clustering

## Distance based clustering algs

### 1. Partitioning algs

- Use of prototypes (central points for each cluster)
- Assign each cluster to the closest centroid
- NEED OF PRIOR KNOWLEDGE about the number of classes
- K-Means, K-Medians, K-Medioids, K-Modes (depending on the preferred measure)
#### K-Means:

- Partition the data into K clusters
- Object closest to the cluster centroid -> Cluster
- After each iteration, centroid recalculatiron --> Convergence
- Hyperparams:
	- Number of clusters
	- Distance measure
	- Initial values for convergence

- Centroid is calculated as the MEAN (in this case)
- Random initialization of centroids
- Computes dissimilarity by EUCLIDIAN DISTANCE
- Linear complexity

**VARIATIONS:**
- C-medians:
	- Median instead of mean
	- Use of manhattan distance
	- Less sensitive to outliers
- C-medioids:
	- The medioid is an object that represents the default of the class, a real object, not an average
	- CLARA; Clustering LARge Applications, which comes from... -> 
	- PAM; Partitioning Around Medioids
	- C-Modes, for categorical data
 
```python
convergence = False
while !convergence:
	calculate_distance_between_each_object_and_clusterCentroid() 
	assign_each_object_to_nearest_centroid()
	new_pos = recalculate_centroids()
	if max(new_pos - old_pos) < threshold:
		convergence = True
```

## Determining the number of clusters:

### Elbow method:

One of the most popular heuristics to determine the optimal value of C

- Plotting the sum of squared errors (SSE) as a function of the number of clusters:
- Steps:
	- Run the clustering for a range of values for C
	- After clustering, compute the SSE
	- Line plot of the SSE for each value of C
	- Look at the graph. It should have an inflexion point. THAT IS THE OPTIMAL NUMBER OF CLUSTERS

![[Pasted image 20250105002022.png]]
But not always has an elbow:
![[Pasted image 20250105002114.png]]

### Silhouette Method:

Methods consisting of computing coefficients for each point; those Silhouette coefficients measure how similar an object is to its own cluster (cohesion) compared to other clusters (separation)

The average of the silhouette coefficients for all the objects gives the silouhette score

This score ranges between -1 and 1, where high value indicates the object is well matched to its own cluster and poorly matches other clusters.

If most objects have a high value, clustering is appropriate. If many points have low or negative values, the clustering may not have the optimal number of classes.

![[Pasted image 20250105002439.png]]
Like in the elbow method, we can pick a range of candidate values of C. We have to run the clustering alg for each item in C, plot the silhouette_score/C and take the clustering with highest vale (more obvious a peak than an elbow)

OR, making a *Silhouette Plot*
- X-axis displays the silhouete coeficients
- Y-axis displays the labels of each cluster
- If all cluster plots are beyond the average silhoete score, have mostly uniform thickness, and do not have wide fluctuation, the number of clusters is optimal

![[Pasted image 20250105002942.png]]
(2 clusters)

![[Pasted image 20250105002953.png]]

(3 clusters)

![[Pasted image 20250105003014.png]]

(4 clusters)

![[Pasted image 20250105003035.png]]

(5 clusters)

![[Pasted image 20250105003053.png]]

(6 clusters)

For three clusters, all clusters have scores above the average, that is the optimal configuration.

## Hierarchical algorithms:

- Unsupervized learning algs that seek to build a hierarchy between objects while clustering them
- Incremental algs
- No requirement for a number of clusters to be defined
- Create complex shaped clusters
- Quadratic complexity

### Agglomerative or divisive:

- **Agglomerative**: Bottom-up, starting from a cluster for each object, they merge the most similar clusters into a new cluster until some stopping criterion is met
- **Divisive**: Top-down, starting from a single cluster, is divided until the stopping criteria

### How do they work?

Computing a distance matrix of all the existing clusters, and perform the linkage between the clusters depeding on the criteria of linkage:

- Single linkage: Distance between the two clusters is the shortest distance between objects in those two clusters
- Complete linkage: The distance between the two clusters is the furthest distnce bewoeen objects in those two clusters
- Average linkage: The distance between two clusters is the average distance of every object in the cluster with every object in the other cluster.
- Centroid linkage: The distance between two clusters is the distance between their centroids

![[Pasted image 20250105003844.png]]
## Dendograms
#exam 

The output can be represented by a dendogram, which shows hierarchical relationship between clusters
Similarity between two objects is represented by the height of the node that joins them:


Schematic::
![[Pasted image 20250105004013.png]]
Sample 2::
![[Pasted image 20250105004028.png]]

>[!example] Example with 25 objects
>
>![[Pasted image 20250105004122.png]]
>
>![[Pasted image 20250105004133.png]]

>[!example] A simple example with two objects:
>
>![[Pasted image 20250105004159.png]]
>
>![[Pasted image 20250105004208.png]]


## Density based clustering:

- Assume that a cluster is a dense region of points, separated from other dense regions by spare regions.
- Useful when clusters can be of arbitrary shapes, are interleaved, or ther are outliers/noise in the data
- The best known alg is DBSCAN (Density Based Spatial Clustering of Applications with Noise)

- Clusters are formed by connecting the objects that are densely located in a region
- Concepts like Core Point, Border Point and Noise Point
- Time complexity is logaritmic

### Concepts:

- Epsilon ($\epsilon$) -> Maximum distance between a pair of objects
- MinPts -> Minimum number of objects to define a region (cluster)

$\epsilon$ decides the size of a circle
$MinPts$ define the minimum number of points to be in that circle

![[Pasted image 20250105004634.png]]

Two points are considered neighbors if they are sepparated by a distance smaller or equal to $\epsilon$ 

![[Pasted image 20250105004728.png]]

1. For each object, check if it is a core point
2. For each core point, find all connected objects
3. Assign every non-core object to the nearest cluster if the cluster is in its $\epsilon$, else, it will be a **noise** point
4. The alg stops when has explored all the objects one by one and classifies them as core, border or noise.

## Summary:

**PROS:**
![[Pasted image 20250105005025.png]]

**CONS**:
![[Pasted image 20250105005046.png]]
# Data reduction:

![[Pasted image 20250105125656.png]]

## Objectives:

- Datasets might be too big
- ""                      be too computational demanding
- ""                      have noisy data that lead to misclassifications
- ""                      have redundant or irrelevant data that might affect the learning process by giving too much weight to duplicated features, or use features that don't provide any relation to the class prediction


## Strategies:

- Removing irrelevant attributes
- ""              attributes with little to no variance --> They are constant
- ""              highly correlated attributes --> They are the same
- ""              redundant attributes

## Curse of dimensionality

*"Number of needed samples need scale exponentially with increase in dimensions"*

***Principle of parsimony*** or ***Occam's Razor*** -> Develop the simplest, most explainable model.

>[!example] When LARGE NUMBER OF ATTRIBUTES; LOW NUMBER OF SAMPLES :
>
>- DNA microarrays: expression of thousands of genes // just a few samples (tens or hundreds)
>- Just a few of them are related to the target phenotypes of the study
>- Dimension can be highly reduced

## Feature selection:

- Using a feature selection alg --> data contains many attributes that can be removed without loosing much info (:LiArrowBigDown: Efficiency; :LiArrowDownRight: Precision (but not much))

Consists in:
1. Search technique for finding features subset
2. Evaluation measure that scores different features in function of their relevance

Simplest version: Exhaustive search of the space --> Test each possible subset finding the one that minimizes the error rate --> Computationally inaccurate.

## Types of feature selection algs:

![[Pasted image 20250105131726.png]]
### WRAPPERS:

- Predictive model to score feature subsets
	- Search technique to search to the space of possible attribs
	- Usefulness of a subset is judjed by the estimated accuracy

- Very computationally intensive; learning process must happen for every found subset to evaluate the quality

- Risk of overfitting

### FILTERS:

- Sepparate the selection of attributes from the learning algorithm
- Attributes are selected from correlation to class label
- Proxy measure to score the feature subset (instead of relying on training)

- Faster and computationally cheap

**++:** PROVIDE a RANKING of FEATURES by CORRELATION -> User can select how many features or which is the threshold.

### EMBEDDED METHODS:

- Combining feature selection with model construction process --> Use algs that have built-in feature selection models

- Intermediate complexity:

- Popular method => LASSO:
	- Penalize the regression coefficients, reducing many of them to close to zero
	- Any features whose regression coef. cannot be reduced is selected

## Search Techniques:

### Sequential Forward Selection (SFS): 

- Iterative method

- START with having no features in the model
	- For each iteration:
		- Add the feature that best improves our model
		- If Quality_Now < Quality_before + Threshold: break

*:LiAsterisk:Alternatively, the process can stop once the desired amount of features is reached*

### Sequential Backward elimination:

- START with all features
- While !convergence:
	- Remove the least significant feature at each iteration -> Improve performance

:LiAsterisk:*Convergence is reached like in previous case (either performance is not improved enough, or number of features desired is reached)*

### Bidirectional selection

Combination of both: Search in both directions, performing SFS and SBE.

Convergence is reached when one of them finds the best subset or the desired number of features OR when both reach the midpoint

## Proxy measures:

- Information mesures -> Score the "amount of information from each feature"
	- Mutual information: Mutual dependence between two random features
		![[Pasted image 20250105135401.png]]
	- Entropy: Measure uncertainty of a dataset
		![[Pasted image 20250105135453.png]]
		- LOW ENTROPY -> The data labels are uniform
		- HIGH ENTROPY -> Chaos, lower information
	- Conditional entropy: Uncertainty in the dataset when the information of the attributes is already low.
		![[Pasted image 20250105135614.png]]

- Dependency measures -> Correlation between an attribute and the predicted class
	- Pearson's correlation coefficient: measure of linear dependency between two or more features
		![[Pasted image 20250105135650.png]]
		- the logic behind using correlation for feature selection is that the **good variables are highly correlated with the class**
		- furthermore, variables should be correlated with the class but should be **uncorrelated among themselves**
		- if **two attributes are correlated**, the **model only really needs one of them**, as the second one does not add additional information → redundant attributes
	- Spearnan's correlation coef: like Pearson's but for non-linear correlations

- Distance measures -> Space between classes

- Consistency measures -> Minimum number of attributes that consistently discriminate classes like using the full dataset
	- Consistency based filter: Score the worth of a subset of attributes by level of consistency in the class values
		- Inconsistency -> Two instances with the same input but different output

- Accuracy measures -> Direct performance of the subset

- Other proxy measures:
	- Variance Threshold -> Remove low variance features
	- Mean Absolut Difference (MAD) -> Attributes whose values differ more from the mean value -> Higher discriminatory power (like variance)

## FILTER vs WRAPPER vs EMBEDDED

![[Pasted image 20250105140106.png]]

## Feature extraction 
-> Linearly combine features into new ones to capture the same information with fewer attributes

- Not useful when model interpretability is key
- PCA and LDA

### PCA: 
- Unsupervised learning technique -> Obtain IMPORTANT VARIABLES by finding small combination of variables that best summarizes the initial attribute

- Find the most significant attributes -> Direction of maximum variance -> Project them to a set of orthogonal axes

- Principal Component -> Normalized linear combination of the original features:
	- First prioncipal component -> Maximum variance
	- All other components: Perpendicular to eachother

- Result: No overlapping direction -> Orthogonal dimensions assure that PCA1 is not present in PCA2 -> Totally independent

![[Pasted image 20250105140559.png]]
1. Normalize data (mean 0; variance 1)
2. Find covariance matrix $A$ of the scaled data ![[Pasted image 20250105140656.png]]
4. Find EIGENVECTORS and EIGENVALUES
5. Project the scaled data into one dimension using vector V

![[Pasted image 20250105140927.png]]

![[Pasted image 20250105140935.png]]
### LDA :
- Project the data onto a new axis to maximize the separation of classes
- This new axis created according to:
	- Distance between means maximization
	- Scatter minimization
![[Pasted image 20250105141155.png]]

1. Data normalization (mean 0; variance 1)
2. Compute the d-dimensional mean and variance vectors of each class
3. Compute intra-class scatter matrix $S_w$ (spread around means of each class) and Inter-Class scatter matrix $S_B$ (distance between classes means)
	
	![[Pasted image 20250105141407.png]]
	
	Where $\mu$ is the overall mean, $\mu_i$ and $n_i$ are the Mean and Size of each class
4. From $S_W^{-1}S_B$, calculate eigen vectors and eigen values
5. Sort the eigen-things by eigen-value decreasing
6. Chose a desired number of eigen vectors ($k$) with the highest vale -> assemble a projection matrix (size $k·d$) where each column corresponds to an eigenvector
7. Transform the data:
	$Y = X · W$

## PCA vs LDA

1. LDA is SUPERVIZED // PCA is UNSUPERVIZED
2. LDA describes the direction of maximum sepparability // PCA describes the direction of max variance

# U11 - Numerosity reduction

# U12 - ANNs

## Linear model: 

*"Composition of non-linear transformations of linear models"*

$$
f(x|wk, ..., w_1) = g^*(w_k^tg_{k-1}(w_{k-1}^tg_{k-2}(...g_2(w_2^tg_1(w_1^tx)))))
$$
Sort of recursive function, as a mathematical function is hard but when coding, it's just a for

- 𝑥, the input vector to the network
- 𝑘, the number of layers (or transformations)
- 𝑤𝑖, weight matrix of the layer 𝑖, 1 ≤ 𝑖 ≤ 𝑘
- 𝑔𝑖, nonlinear activation function of the layer 𝑖 (they are generally the same)
- 𝑔∗, output layer activation function (can be the identity function)
- 𝑓, network function composed from chains of linear and nonlinear functions

![[Pasted image 20250105190403.png]]
![[Pasted image 20250105190623.png]]![[Pasted image 20250105190758.png]]

## Activation functions (basic)
### **1. Step Function**

- **Definition**:  
    A simple binary activation function that **outputs 1** if the **input is above a threshold** (commonly 0) **and 0 otherwise.**
	- 1 if x > 0
	- 0 otherwise
	
- **Characteristics**:
    - Introduces **no non-linearity in the derivative**, which makes it **unsuitable for learning complex patterns**. (NOT CONTINUOUS AT ALL POINTS)
    - **Non-differentiable at x=0x = 0x=0**, making gradient-based optimization difficult.
    
- **Applications**:
    - Historically used in **simple perceptrons** and **early neural networks**.
    - Rarely used in modern deep learning.

---

### **2. Sigmoid Function**

- **Definition**:  
    A smooth, differentiable function that outputs values between 0 and 1, resembling an S-shaped curve.    $$f(x)=11+e−xf(x) = \frac{1}{1 + e^{-x}}f(x)=1+e−x1​$$
- **Characteristics**:
    - Output: ***(0,1)*** making it **useful for probability-based tasks**.
    - **Differentiable**: Supports gradient-based optimization.
    - **Saturation Issue**: Gradients become **very small for large positive or negative $x$** (vanishing gradient problem), making **training deep networks challenging**. (very sensitive)

- **Applications**:
    - **Widely used in the output layer** for **binary classification problems** (e.g., logistic regression).
    - **Less commonly used in hidden layers due to saturation issues**.

---

### **3. Rigid Sigmoid (Hard Sigmoid)**

- **Definition**:  
    A simplified version of the sigmoid function that is computationally more efficient and linear in certain regions. The hard sigmoid can be approximated as:
    
    - $0$                 if x <= 2.5
    - $1$                 if x >= 2.5
    - $0.2x +0.5$   otherwise

- **Characteristics**:
    - **Faster to compute** **than** the standard **sigmoid**.
    - Helps **mitigate vanishing gradient problems** compared to the standard sigmoid.
    - **Not as widely used due to the emergence of more efficient activation functions like ReLU.**
    
- **Applications**:
    - Occasionally used in **simpler networks or where computational efficiency is critical.**

### **4. ReLU (Rectified Linear Unit)**

- **Definition**:  
    Outputs the input directly if positive, otherwise 0.
    $f(x)=max⁡(0,x)f(x) = \max(0, x)$
    
- **Characteristics**:
    - Introduces **non-linearity while avoiding saturation for positive values**.
    - **Computationally efficient**.
    - Issue: **Dying ReLU problem** (neurons can get stuck outputting 0 and stop learning if weights are not updated).
	
 - **Applications**:
	 -  Dominantly used in hidden layers of deep neural networks.

| **Function**      | **Output Range** | **Differentiability** | **Pros**                                 | **Cons**                         | **Best For**                         |
| ----------------- | ---------------- | --------------------- | ---------------------------------------- | -------------------------------- | ------------------------------------ |
| **Step**          | 0 or 1           | No                    | Simple, binary output.                   | Non-differentiable, no gradient. | Early perceptrons.                   |
| **Sigmoid**       | (0, 1)           | Yes                   | Smooth, probabilistic interpretation.    | Vanishing gradients.             | Binary classification output layers. |
| **Rigid Sigmoid** | (0, 1)           | Approx. Yes           | Efficient, avoids saturation partially.  | Still prone to small gradients.  | Simple networks.                     |
| **ReLU**          | [0, ∞)           | Yes                   | Efficient, non-saturating for positives. | Dying ReLU issue.                | Hidden layers in deep networks.      |


## Limitation of the linear classifier

![[Pasted image 20250105191820.png]]

## Fully connected neural networks

![[Pasted image 20250105191850.png]]

- Each layer consists in linear function + activation layer
- Funcion de transformacion ($\sum$ in the previous image)
	$z_l = W^l · I^l \space ; \space I_{l+1} = g(z_l)$
- Let $H_l$ be the units of tha $l row$ 
...


## BackPropagation:

Backpropagation (short for "**backward propagation of errors**") is a fundamental algorithm in training artificial neural networks. It **calculates the gradient of the loss** **function** **with respect to each weight in the network**, enabling the **weights to be updated using gradient descent** or similar optimization algorithms.

---

### **Key Concepts in Backpropagation**

To understand backpropagation, it’s essential to be familiar with the following:

1. **Feedforward Pass**:
    - Input data is **passed through the network layer by layer** to **produce an output** (prediction).
    - At each **layer, the activation function is applied**, and **outputs are computed until the final result**.

2. **Loss Function**:
    - Measures the **difference between the predicted output** (y^\hat{y}y^​) **and the actual output** (yyy).
    - Common loss functions include **Mean Squared Error** (MSE) for regression and **Cross-Entropy Loss for classification**.

3. **Gradient Descent**:
    - An **optimization algorithm** that **updates weights to minimize the loss function**. The weights are updated using: $$w = w - \eta \cdot \frac{\partial L}{\partial w}$$where w is the weight, **$\eta$** is the **learning rate**, and $\frac{\partial L}{\partial w}$ is **the gradient of the loss $L$ with respect to $w$**.

4. **Chain Rule**:
    - Used to **compute the gradient of a composite function**. In neural networks, the **chain rule allows the calculation of gradients layer by layer**.

---

### **Steps in Backpropagation**

Backpropagation involves two main passes: **Forward Pass** and **Backward Pass**.

---

#### **1. Forward Pass**

- The input (x) is passed through the network.
    
- Each layer computes:
        $$z^{(l)} = W^{(l)} a^{(l-1)} + b^{(l)}$$$$a^{(l)} = f(z^{(l)})$$    
    where:
    - $W^{(l)}$ and $b^{(l)}$ are weights and biases at layer $l$,
    - $a^{(l)}$ is the activation, and
    - $f$ is the activation function.

- The final output is compared with the target using the loss function to calculate the loss $L$.

---

#### **2. Backward Pass**

The backward pass propagates the error backward through the network to compute gradients for weight updates. It uses the **chain rule** to compute derivatives layer by layer.

1. **Calculate Output Layer Gradient**:
    
    - Compute the error at the output:
      ![[Pasted image 20250105193327.png]]
      where f′(z(L))f'(z^{(L)})f′(z(L)) is the derivative of the activation function at the output layer.

2. **Propagate Error Backward**:
    - For hidden layers 
	    ![[Pasted image 20250105193359.png]]
        - Here, δ(l)\delta^{(l)}δ(l) represents the error term for layer $l$.

3. **Calculate Gradients**:
    - Compute gradients for weights and biases: 
      ![[Pasted image 20250105193437.png]]

4. **Update Weights**:
    - Use gradient descent to update weights and biases: 
      ![[Pasted image 20250105193458.png]]
      

---

### **Advantages of Backpropagation**

- **Efficiency**: It **computes gradients efficiently** using the chain rule.
- **Scalability**: Works **well for deep neural networks** with many layers.
- **Flexibility**: Can be **used with various network architectures** and loss functions.

---

### **Limitations of Backpropagation**

1. **Vanishing/Exploding Gradients**:
    - In deep networks, **gradients can become extremely small or large**, making training difficult.
    - Mitigated by techniques like **ReLU activation**, **batch normalization**, and **gradient clipping**.

2. **Computational Cost**:
    - Backpropagation is **resource-intensive for large networks and datasets**.

3. **Local Minima**:    
    - Gradient descent **may converge to a local minimum instead of the global minimum.**

4. **Dependence on Hyperparameters**:
    - Performance is sensitive to **hyperparameters like the learning rate** and **initialization**.

```scala
initialize network weights w(0) with small arbitrary values 
for epoch = 1...K, do
	for batch = 1...N/batch_size, do
		batch <– randomly choose batch_size instances
		X, y <– preprocess(batch)
		z <– network(X) (forward execution)
		ℓ <– loss(z, y)
		g <- gradients(ℓ, w) (backward execution)
		w(t+1) <- w(t) – γ · g (weight optimization/fitting)
	end for 
end for
```
![[Pasted image 20250105194237.png]]
1. initialize weights, choose learning coefficient, choose stop criterion

2. create an arbitrary batch (X, y) --> (subset of instances, expected output).

3. forward execution: walk X through the network and get output z.

4. compute loss(y, z)

5. backward execution (backpropagation)

	-  compute sensitivity coefficient 𝛿𝐿 = 𝑓 𝛿𝐿+1 from input to layer L
	
	- compute gradients $\frac{\partial E}{\partial{w_{ij}^{L-1}}} = f(\delta_L)$

6. weight optimization

7. check stopping criteria; if it is fulfilled, then finish; otherwise, go to step 2.


![[Pasted image 20250105194620.png]]

## Softmax Function

A **Softmax layer** is a layer in a neural network that applies the **softmax activation function** to its inputs. The softmax function **converts raw scores** (logits) **into probabilities**, making it **particularly useful for multi-class classification tasks**.

## Categorical Cross-Entropy (CCE)

Categorical Cross-Entropy (CCE) is a **widely used loss function** for **classification tasks where the goal is to predict a single class label** out of **multiple mutually exclusive** classes. It measures the **difference between the true class labels** (one-hot encoded) **and the predicted probability distribution** (output of a softmax layer).