---
Subject: Machine Learning
Teacher: "@RamónAlbertoMollinedaCárdenas @JoséSalvadorSánchezGarreta"
Topic: 
date: 
tags:
  - SJK003-MachineLearning
---
# Concepts
Base principle:
![[Pasted image 20241127152632.png]]

Architectural patterns:
![[Pasted image 20241127152642.png]]
a) **\[UP\]** Classification
b) **\[DOWN\]** Reggression


# Convolutions in images

![[Pasted image 20241127152953.png]]
![[Pasted image 20241127153004.png]]![[Pasted image 20241127153024.png]]
## Stride and padding
![[Pasted image 20241127153138.png]]
![[Pasted image 20241127153147.png]]
![[Pasted image 20241127153157.png]]

#KNBN-Investigate


![[Pasted image 20241127153213.png]]

## Convolutions in 3D maps
![[Pasted image 20241127153246.png]]
(where R is generally the number of channels)

# Neural Network structure

![[Pasted image 20241127153433.png]]
Structured in 2 sets:
- First part is a set of convolutional layers, to be able to relate all data in the image (next image). 
  CHARACTERISTICS:
	- Shared parameters
	- Nr of Params depends on the kernels as well as previous images
	- Fewer calculations
	- Connections and parameters don't deppend on input size
	  
- Second part is a set of fully connected layers (traditional NN for ML)
  CHARACTERISTICS:
	  - one parameter per connection 
	  - the number of parameters depends on the units of the connected layers
	
- Last layer would be a SoftMax with as many neurons as wanted classes.

About the Convolutional Lyers:
![[Pasted image 20241127154304.png]]


>[!example] Example: Convolutional neural network for the MNIST task
>![[Pasted image 20241127154554.png]]
>
>**a)** How would the dimensions decrease for each convolutional layer assuming NO PADDING?
>*Note the kernels are 5x5, which means the first pixel where we could apply the convolution would be $(3,3)$ (the center of the kernel without the kernel going out the image), losing $4 px/dimension·convolution$.*
>*This way, the 1st layer would have dimensions of 24(px)x24(px)x20(layers/neurons) and the second would be 20(px)x20(px)x40(layers/neurons)*
>![[Pasted image 20241127155038.png]]
>
>**b)** How many parameters would each layer have?
>**c)** And the whole model?
>Clayer 1:
>
>Clayer 2:
>
>FClayer 1:
>
>FClayer 2:
>
>OutLayer:
>
>![[Pasted image 20241127155802.png]]
>


# Optimizers

## ADAM (Adaptative Moment Estimation)

Gradient Descent:
$$
\Theta_{t+1} = \Theta_t - \gamma \frac{\partial L}{\partial\Theta}(\Theta_t)
$$

ADAM:
![[Pasted image 20241127160127.png]]
- Typical values being
  $$\beta_1 = 0.9$$
  $$\beta_2 = 0.999$$
  $$\epsilon = 10^{-8}$$
- $m_t$ is the **first order moment**
- $v_t$ is the **second order moment**, which is directly related to the variance
- Both moments are computed by **moving averages**
- The weight of the previous moments is reduced exponentially somewhat like (knowing that $\beta < 1.0$):
  $m_n = (1-\beta)(g_1\beta_1^{n-1} g_2\beta_1^{n-2}...+g_{n-1}\beta_1^2+g_n\beta_1)$ 

## ADAMW (Adaptive Moment Estimation with Weight Decay)

![[Pasted image 20241127161935.png]]