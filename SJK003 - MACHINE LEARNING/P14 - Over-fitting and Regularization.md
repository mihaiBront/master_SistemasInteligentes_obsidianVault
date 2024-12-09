
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


## Dropout

"*Dropout (in neural networks) means deactivate/cancel units in training phase, both in forward and backward executions, to prevent co-adaptation of units and to force the network to learn more robust features*
*Dropout (in neural networks) is equivalent to a combination of subnets (models) with shared parameters*"

It is commonly used in FC NNs, more than in convolutional ones

![[Pasted image 20241204152603.png]]

![[Pasted image 20241204152614.png]]in training 
- in each network update (one batch processing), in the affected layer, each unit can be disabled with probability 𝑝 (along with its connections) 
- therefore 𝑝 would be the expected fraction of units disabled (on average)
- disabled units are not involved in gradient computation

in test 
- all units and connections are involved in data processing
- activations 𝑦 scale according dropout rate 𝑝: 𝑦 = 1 − 𝑝 · 𝑦 
  <u>goal</u>: to adapt each unit’s activation to the percentage of times (on average) the unit was involved in updating the weights (mini-batch)

<u>SUMMARY</u>
- dropout forces the network to learn more robust features, obtained from multiple network configurations
- configuration ≡ subnet with a particular combination of units 
- given 𝐻 hidden units, there are 2𝐻 possible subnets/models 
- the result is equivalent to a combination of subnets (ensemble) 
- an ensemble is generally more effective than individual models 
- dropout requires more iterations to converge (more unstable process), but each one requires less computational time

<u>GUIDELINES</u>
- Not commonly used in convolutional networks (batch normalization is better for those applications)
	- less need: CNNs have fewer parameters than fully connected networks
	- inappropriate: a feature map contains information with strong spatial dependencies; dropout affects these relationships 
	- however, it sometimes helps improve a model CNN
- recommended in fully connected networks 
- dropout rates are recommended between 0,2 and 0,5 
- the larger the network, the more effective the use of dropout 
- the original article recommends learning rates higher than usual

### Example in Keras:

```python
… 
model.add(Conv2D(128, (3, 3), padding='same')) model.add(Activation('relu')) model.add(Conv2D(128, (3, 3), padding='same')) model.add(Activation('relu')) model.add(keras.layers.MaxPooling2D(pool_size=(2, 2))) model.add(GlobalMaxPooling2D()) model.add(keras.layers.Dropout(0.3)) model.add(Dense(64, activation='relu')) model.add(keras.layers.Dropout(0.3)) model.add(Dense(num_classes, activation='softmax'))
```


## Batch normalization

![[Pasted image 20241204155609.png]]

Batch normalization in this context, is not normalizing the data entering the Network, but each one of the deep layers.

Batch normalization here is done by normalizing the output of each activation function. This translates into learning both a weight and a normalization for each neuron.

![[Pasted image 20241204155926.png]]
![[Pasted image 20241204160631.png]]

$\beta$ and $\gamma$ are learned, but $\mu$ and $\sigma$ are calculated for each batch, what do we do with that? -> Moving average
### During training

![[Pasted image 20241204161158.png]]
![[Pasted image 20241204161205.png]]

## During inference

![[Pasted image 20241204161224.png]]

### Summary
![[Pasted image 20241204161408.png]]

### Code example

```python
model = Sequential() 
model.add(Conv2D(32, (3, 3), padding='same', input_shape=train_X.shape[1:])) # input_shape=(32,32,3)
model.add(keras.layers.BatchNormalization()) model.add(Activation('relu')) model.add(Conv2D(32, (3, 3), padding='same')) model.add(keras.layers.BatchNormalization()) model.add(Activation('relu')) model.add(keras.layers.MaxPooling2D(pool_size=(2, 2))) model.add(Conv2D(64, (3, 3), padding='same')) model.add(keras.layers.BatchNormalization()) model.add(Activation('relu')) model.add(Conv2D(64, (3, 3), padding='same')) model.add(keras.layers.BatchNormalization()) model.add(Activation('relu')) model.add(keras.layers.MaxPooling2D(pool_size=(2, 2))) …
```
![[Pasted image 20241204161557.png]]

