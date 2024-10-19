# 8.1 Terminology and introduction

**Multi-classifiers synonyms**: ensemble of classifiers, combining classifiers, decision committee, multiple classifier system, mixture of experts, committee-based learning, etc.

## 8.1.1 Motivation:

Setting the environment, decisions:
- which learning algorithm to use? 
- which parameters to choose? -> Best discerning ones, how to select them
- how to use the training data? 
- which vector space to map the data onto? What is the most discriminating representation?

Diverse models may appear while searching for a solution. Many options might be equally valid, or some methods might be slightly more valid than another. 

>[!tip] Tips for obtaining the combined model:
>- In this case, a reasonable choice is to keep them all and create a final system integrating the pieces 
>- The core idea behind this is to aggregate multiple models to obtain a combined model $D$ that outperforms every single model $D_i$ in it 
>- Each single model $D_i$ is called base learner (classifier) or individual learner (classifier)
>
>The aim is for this multi-classifier to be much better than the application of a single model



## 8.1.2 Strategies:

-  Combination level: design different combiners 
- Classifier level: use different base classifiers 
- Data level: use different data subsets 
- Feature level: use different feature subsets

![[Pasted image 20241016151535.png]]


## 8.1.3 Combination level: fusion vs selection

**Fusion**:
- each ensemble member is supposed to have knowledge of the whole feature space 
- some combiner such as the average and majority vote is applied to label the input object x
- Synonums:
	- Competitive classifiers
	- Ensemble approach
	- Multiple topology
   
**Selection**: 
- each ensemble member is supposed to know well a part of the feature (*be the expert in one field*) space and to be responsible for objects in this part.
- one member is chosen to label the input object x (not all classifiers take part in the classification of each object, but each classifier will be in charge of a region of the class space/vector space)
- Synonums:
	- Cooperative classifiers
	- Modular approach
	- Hybrid topology


# 8.2 Fusion

Based on the vote of the majority of classifiers; deciding the class by the most voted by the base classifier. Different consensus patterns are available.

![[Pasted image 20241016152135.png]]
Let it be:
$$
[d_{i,1} ... d_{i,C}] \in \{0, 1\}, i = 1,..., L
$$
Where:
- $d_{i,j}=1$ If $D_i$ labels $x$ in class $\omega_j$, and 0 otherwise
- $L$ Is the number of base classifiers

Then the plurality vote rule will result in an ensemble decision for class:
$$
\sum_{i=1}^{L} = max_{j=1,...,C}\sum_{i=1}d_{i,j}
$$

A variant of this would be the classifier with option to reject: if we increase the set of classes with one more class $\omega_{c+1}$, for objects for which the ensemble does not determine a class label with a sufficient confidence. Now, the decision is

$$
\begin{equation}
	\begin{cases}
		w_k\\
		w_{c+1}
	\end{cases}
\end{equation} 
if 
\sum_{i=1}^L d_{i,k} \ge \alpha·L
$$
Where:
- $0 \le \alpha \le 1$ 

>[!abstract]
>If 𝛼 = 1, this becomes the unanimity vote rule

Another option would be the weighted majority vote

![[Pasted image 20241016153205.png]]
# 8.3 Selection

The feature space ($R^d$) is divided into k > 1 selection regions (or *regions of competence*) which are denoted by $R_1,...,R_K$

- Usually K = L, but this doesn't have to be true, one *base classifier* can be competent in more than one selection regions (it can happen that $K>L$)
- each region $Ri$ is associated with a classifier, which will be responsible for deciding on the input objects in this part of the space
- these regions are not associated with specific classes, nor do they need to be of a certain shape or size

>[!example] 
>suppose a data set with 2000 points and two classes $\omega_1$ and $\omega_2$, and we have an ensemble with three classifiers $D_1$, $D_2$, $D_3$, each one associated with regions $R_1$, $R_2$, $R_3$
>
>![[Pasted image 20241016153615.png]]
>
>Where:
>- $D_1$ always predicts $\omega_1$
>- $D_2$ always predicts $\omega_2$
>- $D_1$ is a linear classifier is a discriminant, whose discriminant is represented by the dashed line
>
>Accuracy of the **individual classifiers** is approx **0.5** for all, but since they work in regions, accuracy of the **selection combiner** is approx **1**


# 8.4 Stacking:

Technique based on learning **various different weak learners**, then, combine the base learners by training a ***meta-model***.

- we need to define two things in order to build our stacking model: the L base learners we want to fit and the meta-model that combines them 
- for example, we can choose as weak learners a *k-NN* classifier, a decision tree and a *SVM*, and decide to learn a neural network as meta-model. Then, the neural network will take as inputs the outputs of our three weak learners and will learn to return final predictions based on it

![[Pasted image 20241016154330.png]]
Designing a stacker:
1. Initialize parameters, $L = number\_of\_weak\_learners$
2. Split the data into two folds
3. For $l : (1,L)$ , train the weak learners with the data of the first fold, make predictions for data in the second fold (Bootstrapping)
4. Training the meta model of the second fold using predictions made by the week learners as inputs, select best ones and make regions of competence

# 8.5 Bagging

the ensemble is made of classifiers built on bootstrap replicates of the training set $T_{tra} = \{x1, …, xn\}$. The classifier outputs are combined by the plurality vote

Comments:
- Weak classifiers are advised to be unstable (little change in input data produces big change in classification), so the meta-model has more play
- we sample with replacement from the original Ttra to create L new training sets (often, also of size n)
- all L base classifiers are the same classification model
- this is a parallel algorithm in both its training and operational phases

![[Pasted image 20241016154835.png]]

Training phase:
1. Initialize the parameters $𝐷 = ∅$, the ensemble $𝐿$, the number of classifiers to train 
2. For $𝑙 = 1, … , 𝐿$ Take a bootstrap sample $𝑆_𝑙$ from the original training set $T_{tra}$ Build a classifier $𝐷_𝑙$ using $𝑆_𝑙$ as the training set Add the classifier to the current ensemble, $𝐷 = 𝐷 ∪ 𝐷𝑙$ Return $𝐷$ 

Classification/Regression phase:
1. Run $𝐷1 ⋯ 𝐷𝐿$ on the input $x$ 
2. Either:
	1. Assign $x$ to the class with the maximum number of votes (simple majority voting, for classification) 
	2. Assign $x$ with the average of the estimated values (simple average, for regression)

## 8.5.1 *"Random forest"*, special case of bagging:

![[Pasted image 20241016155212.png]]


# 8.6 Boosting:

Incrementally create the classifier ensemble $D$, with the idea of the introduced classifier will focus in errors done by the classifier from previous iteration. And so on. This is a **sequential** training algorithm.

Note that the errors that the first classifier makes influence how the second classifier is made, and so on.

![[Pasted image 20241016155445.png]]![[Pasted image 20241016155500.png]]
![[Pasted image 20241016155516.png]]

## 8.6.1 AdaBoost

1. Initialize the parameters with each sample weighting $w_i = 1/n$
2. For $l : (1, L)$ build a classifier $D_l$ with the training data. Calculate the proportion of errors in classification ($e_l$) and then compute $S_l = log((1-e_l)/e_l)$
3. Update the weights bt myltiplying the previous weight of the wrongly classified values by $(1-e_l)/e_l$
4. Iterate

![[Pasted image 20241016160532.png]]

## 8.6.2 Gradient Boosting:

It involves:
![[Pasted image 20241016160554.png]]

It allows working in parallel for training (tree building)
Scalable to large datasets

## 8.6.3 Categorical boost

![[Pasted image 20241016160711.png]]

# 8.7 Random subspace

![[Pasted image 20241016160732.png]]

![[Pasted image 20241016160909.png]]

Interesting when solving problems with high dimensionality (number of characteristics) OR when number of characteristics > number of samples in training set
