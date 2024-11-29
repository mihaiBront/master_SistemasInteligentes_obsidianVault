---
Subject: Computer Vision
Teacher: "@FilibertoPlaBañón"
Topic: 
date: 2024-11-18
tags:
  - SJK002-ComputerVIsion
---
#pending ***COMPLETE FROM NOTEBOOK PAPER***

# Image registration

- Multiview analysis: Mosaicking, Satellite images, ...
- Temporal analysis: Video segmentation, tracking, ...
- Multimodal analysis: Different sensors (CT-MR, ...)
- Scene-to-model: GIS, Medical imaging, ...

## From 2D to 3D
![[Pasted image 20241125152041.png]]

**Depending on the kind of motions we want to account for:**
- **Translation** (takes **2 parameters**; x, y):
- **Similarity** (takes **4 parameters**; *2 translation + 1 rotation + 1 scale*):
	- Rotation around the center
	- Translation + Scale
- **Affine Transformation** (takes **6 parameters**; *2 translations; 2 scaling, 2 skew/sheer*)
- **Projective transformation** (takes **8 parameters**)

>[!note] Note that:
>A bigger modelling (larger parameter set) does not necessarily bring a better resulting image or bring the best results to our application
>The smaller the model, the more controllable, and it should adjust to our needs.

Some models' equations:
![[Pasted image 20241125152625.png]]

## The criterion function

Define a function from which to extract the parameters for the transformation:
$$F_i(\kappa, L_i)$$
Where:
- $L_i$ Is the input information (position and value, for example)
$$L_i = (x_i, y_i, I(x_i, y_i))$$
$$I(x_i, y_i) = value$$
- $\kappa$ is the vector of parameters for our model

To **estimate the parameters** we need to optimize a criterion function, which is usually a *least squares* function which needs to be minimized (although the optimizing function can be defined by the user):
$$\Theta = \sum_i(F_i(\kappa, L_i))^2$$
>[!warning] Be careful...
>...if the transformation is linear, the "*least squares*" approx. is very precise, but for non-linear transformations, it can be less precise.
>

### Grey-level based registration functions:

- Brightness Constancy Assumption
  $$\Theta_{BCA} = \sum_{x_i, y_i \in \Re} (I_1(x'_i, y'_i) - I_2(x_i, y_i))^2$$
  Where: 
	- $I_1$ is the desired point
	- $I_2$ is the actual point

- Optical Flow equation:
  $$\Theta_{OF} = \sum_{x_i, y_i \in \Re}(I_t + u_{x_i}I_x + u_{y_i}I_y)^2$$
	Affine approximation Linearization:
  ![[Pasted image 20241125154208.png]]
	  (*remember that the values we want to extract are $a_n, b_n$ and $c_n$; the rest of the values are known*)
	  Now you need to replace $u_{x_i}$ and $u_{y_i}$ in the OF equation and obtain:
	  $$\begin{bmatrix}   
			  a_1 & b_1 & c_1\\   
			  a_2 & b_2 & c_2
		 \end{bmatrix}$$
	  ![[Pasted image 20241125154905.png]]
	Where:
	$$A_{p,n} => Function => \frac{\partial \Theta_{OF}}{\partial P_n} = 0$$
	- $P_n$ Is the parameter we want to know (matrix below)
	- It is equalized to 0 to obtain a function that relate $P_n$ with the other known parameters

>[!warning] Be carful...
>Outliers affect "*least squares*" a lot!

### Other Optimization Methods
![[Pasted image 20241125155628.png]]
![[Pasted image 20241125155644.png]]
		
![[Pasted image 20241125155708.png]]