
# 10.0 Context

**Objective:** Obtain a reduced representation of the data that is much smaller in size and make it feasible for analysis. We need this due to:
- data analysis could be too complex with huge data sets 
- data analysis could be too computational expensive with huge data sets 
- noisy instances can lead to misclassifications
- irrelevant and redundant attributes can hinder any machine learning process

**Data reduction is:**
- the reduction in the number of attributes or 
- the reduction in the number of instances or 
- the reduction in both the number of instances and the number of attributes

**Categorization:**
![[Pasted image 20241023163326.png]]

**Reasons to go in for dimensionality reduction**: 
- reducing the complexity of the model (a simpler model is easier to understand and interpret)
- improving the predictive performance of models (less noise) 
- providing faster and more cost-effective models (reducing time and storage requirements)

We achieve that through the removal of:
- **irrelevant** attributes
  e.g. Feet size for facial recognition
- attributes with **zero/low variance** -> Almost or exactly constant; does not add any discrimination between classes.
- **highly correlated** attributes (or **redundant**); two attributes that are highly correlated provide the same information.

These values receive the name of "**curse of Dimensionality**" -> s the number of dimensions increases, the sample size needs to increase exponentially in order to achieve an effective estimate of performance; so you might as well reduce the dimensionality as much as possible

All the efforts to reduce dimensionality are geared towards the Principle of Parsimony or the **Occam’s Razor**, which in Machine Learning means to develop the simplest and most explainable model

Dimensionality reduction is more important nowadays because of the Big Data phenomenon.

# 10.1 Feature selection
The premise when using a feature selection algorithm is that the data contains many attributes that can be removed without incurring much loss of information

2 components:
- **Search algorithm** -> Proposing subsets
- **Evaluation Measure** -> score each different subset

# 10.2 Methods

## 10.2.1 Wrappers

Use a **predictive model to score feature subsets**; search technique to search through the space of possible attributes, and the **usefulness** of a feature subset is judged directly by the estimated **accuracy of the model**.

As we train the model for each subset, they are very computationally intensive, but usually provide the best performing feature set for that particular type of mode.

Risk of over-fitting due to using the same model for training as for evaluation.
## 10.2.2 Filters

- the selection of attributes is independent of any learning algorithm 
- attributes are selected on the basis of their relationship to the class label 
- filters use some proxy measure instead of the error rate to score a feature subset 
- fast and computationally inexpensive, thus usually the best approach when the number of attributes is huge 
- many filters provide an ordered ranking of attributes rather than an explicit best feature subset → feature ranking algorithms

## 10.2.3 Embedded

- these perform feature selection during the model construction process → they use algorithms that have built-in feature selection methods 
- they tend to be between filters and wrappers in terms of computational complexity 
- an example of this approach is the LASSO method for constructing a linear model: 
	- LASSO penalizes the regression coefficients, reducing many of them to zero or almost zero 
	- any features that have non-zero regression coefficients are ‘selected’ by the LASSO algorithm

# 10.3 Search techniques

- **Sequential forward selection** (SFS): an iterative method in which we start with having no feature in the model. In each iteration, we keep adding the feature that best improves our model till an addition of a new variable does not improve the performance of the model, or until the desired number of features is reached 
- **Sequential backward elimination** (SBE): we start with all the features and removes the least significant feature at each iteration that improves the performance of the model until no improvement is observed on removal of features, or until the desired number of features is reached 
- **Bidirectional selection**: we start the search in both directions, performing SFS and SBE concurrently. It stops when one search finds the best subset with the desired number of features, or when both searches achieve the middle of the search space

# 10.4 Common Proxy Measures
- **Information** measures quantify the information that can be gained from each attribute 
- **Dependency** measures evaluates the strength of the correlation between two attributes or between an attribute and the class
- **Distance** measures assess the separability between classes 
- **Consistency** measures attempt to find the minimum number of attributes that ‘consistently’ discriminate classes as if using the full set of attributes 
- **Accuracy** measures evaluates the performance of each subset of attributes

## 10.4.1 Information measures

- Mutual Information (information gain): mutual dependence between two random variables -> Information we gain by using a specific variable
	$$
	I(c,f) = H(c) - H(c|f)
	$$
	Where:
	- $H(c)$: Initial entropy of the class
	- $H(c|f)$: Conditional entropy of the class verse a variable
	
	Interested in the entropy to be low for a more pure value
	
	Initial entropy:
	$$
	H(c) = -\sum_{c=1}^{N_c} p(c)·log_2(p(c))
	$$
	- a low entropy indicates that the data labels are quite uniform (e.g., suppose a data set with 1 Positive and 99 Negative samples, then the entropy is very low. If all the 100 samples are Positive, then the entropy is zero) 
	- a high entropy means the labels are in chaos, that is, lower information (e.g., a dataset with 45 Positive samples and 55 Negative samples has a very high entropy)
	  
	Conditional entropy:
	$$
	H(c|f) = -\sum_{f=1}^{N_f}p(f)(\sum_{c=1}^{N_c}p(c|f)·log(p(c|f)))
	$$
	*where 𝑝(𝑓) is the probability of attribute 𝑓, and 𝑝(𝑐|𝑓) is the conditional probability for class 𝑐 given the input attribute 𝑓*
## 10.4.2 Dependency measures

![[Pasted image 20241023170656.png]]
*focusing in the second point, variables should be correlated, obviously, to the class. If not, they don't provide any information and could be removed from the dimensionality mat*

![[Pasted image 20241023170851.png]]
## 10.4.3 Consistency measures

![[Pasted image 20241023170923.png]]
![[Pasted image 20241023171220.png]]

# 10.5 Feature extraction

