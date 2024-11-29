
# Final objective
![[Pasted image 20241127170837.png]]

# Alternatives

## Poor model, base -> Overfitting
![[Pasted image 20241127171846.png]]

## Regularization:

Introduction of constraints in the model optimization to:
- **promote simpler useful models** 
- discourage (avoid) **complex and excessively flexible** models that tend to overfit the training data (overfitting) 
- help balance the model's ability to **fit the training data and generalize to new data.**
- **reduce generalization error** (over unseen test data)

### L1, L2 technique

**hypothesis**: *smaller weights* promote *simpler models* (and simpler models tend to generalize better)

**p-norm** (reminder) of a vector ($\theta$, of $n$ dimensions) 
$$||\theta||_p = \sqrt[p]{|\theta_1|^p + |\theta_2|^p ...+|\theta_n|}$$
- We will usually use
	- 1-norm
	- 2-norm (lasso)

![[Pasted image 20241127173232.png]]
![[Pasted image 20241127174253.png]]
## Pooling (subsampling)
![[Pasted image 20241127174447.png]]
![[Pasted image 20241127174614.png]]
- goal: model simplification, better generalization, overfitting reduction 
- method: propagate local representative activation values 
- hyperparameters: pool size, stride 
- trainable parameters: 0 
- effects: 
	- if stride == pool size, then no overlap 
	- quadratic size reduction of feature maps 
	- reduces the number of parameters and training effort (simpler models) 
	- applies to individual maps

![[Pasted image 20241127175632.png]]
![[Pasted image 20241127175646.png]]
![[Pasted image 20241127175658.png]]