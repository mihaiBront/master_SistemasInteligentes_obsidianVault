---
Subject: Machine Learning
Teacher: "@RamónAlbertoMollinedaCárdenas @JoséSalvadorSánchezGarreta"
Topic: 
date: 2024-12-04
tags:
  - SJK003-MachineLearning
---
# Context

- deep learning inherits this practice… 
- promise – deep networks are universal approximators, capable of solving any problem 
- fine print – they depend on millions of parameters to optimize! 
- <s>Myth – therefore, you will not be able to create a DL solution unless you have sufficient amounts of data from the target task</s>
- truth – it's possible to… 
	- transfer learned representations to related tasks 
	- learn good representations from unlabeled data 
	- learn representations common to different domains


## Actuality

learn from scratch with insufficient data => overfitting

## Utopia
learn from scratch with unlimited resources… 
- unlimited labeled data 
- unlimited computing power 
- unlimited time 
`(optimal solution at a prohibitive cost)`


# Definition and popular strategy

"*Transfer learning … refers to the situation where what has been learned in one setting … is exploited to improve generalization in another setting.*"

![[Pasted image 20241204174940.png]]

## Formal definition

![[Pasted image 20241204175024.png]]

# TL Methods in Deep Learning

- transfer of pre-trained features: 
	- similar domains $D_S \approx D_T$
	- different tasks $T_S \ne T_T$
- domain adaptation: 
	- similar tasks $T_S \approx T_T$
	- different domains $D_S \ne D_T$

![[Pasted image 20241217151325.png]]
## Pre-trained CNN features
![[Pasted image 20241217151431.png]]

### Strategies
![[Pasted image 20241217151714.png]]

1. Keeping just the *convolutional* part of the NN and adding only a traditional classifier specialized for the task
	- You can do more sophisticated things like
		- Keeping the last few layers result from the convolutional part, and feeding them to three different models -> Combine them to predict more accurately
2. Keeping just the *convolutional* part of the NN and adding a neural network trained for the task
	- Useful when the tasks are vastly different ($classification \ne segmentation$)
3. Keeping part of the *convolutional* part, modifying or training just the last few layers form that convolutional part; then, adding a trained classifier for that task.
	- Conservative with the amount of layers to modify from theconvolutional part of the set. Adjusting and redirectioning  the weights just a little 
	- Modifying the added dense layers is always needed, they will rarely adapt to the new task.
	- An *example* of usage is applying a general image segmentation model to a speciffic ambit like medical image.

### Typical workflow

![[Pasted image 20241217152818.png]]

>[!abstract] Investigate
>[Feature Pyramid Network](https://paperswithcode.com/method/fpn)
>
>![[Pasted image 20241217153149.png]]
>
>Based on going back from each layer to provide more information about the images; "enrich larger, previous and less speciffic layers with data from more speciffic ones".
>
>Applications like image segmentation: returns info aboyt where the objects were found.

>[!reminder] Sample Question #exam 
>Given a problem, its data and its constraints, provide a model that could solve the problem.
### Examples

A. **Freezing the whole convolutional model:**

```python
# instantiate a base model with pre-trained weights
base_model = keras.applications.Xception(
		 weights='imagenet', # load weights pre-trained on ImageNet. 
		 input_shape=(150, 150, 3), 
		 include_top=False) # do not include the ImageNet classifier at the top. 

# freeze the base model 
base_model.trainable = False 

# create a new model on top 
inputs = keras.Input(shape=(150, 150, 3)) 

# make sure that base_model is running in inference mode by passing `training=False` 
x = base_model(inputs, training=False) 

# convert features to vectors, although this can be specified in the constructor
# the size of the globalAveragePooling layer has the same size as the last layers convolution
# after this layer, we got a SPATIAL DISTRIBUTION of the CHARACTERISTICS of the INPUT IMAGE
x = keras.layers.GlobalAveragePooling2D()(x) 

# a Dense classifier with a single unit (binary classification) 
outputs = keras.layers.Dense(1)(x) 
model = keras.Model(inputs, outputs) 

# train the model on new data 
model.compile(optimizer=keras.optimizers.Adam(), 
			  loss=keras.losses.BinaryCrossentropy(from_logits=True), 
			  metrics=[keras.metrics.BinaryAccuracy()]) 

# this will, then, only train the layers from:
#    - After the GlobalMaxPulling
#    - Up to the end
model.fit(new_dataset, epochs=20, callbacks=..., validation_data=...) 
```

>[!example] **Options for transferring the model to a new case:** (case of Imagenet -> MedicalImage)
>
>**Initializing weifht like**
>A) M(random)
>B) M(imagenet) + Retrain(all)
>C) M(imagenet) + Freeze(imagenet) + Retrain(classifier subnet)
>D) M(imagenet) + Freeze(imagenet[:-N]) + Retrain(imagenet[-N:] + classifier_subnet) 

>[!abstract] Investigate
>[EfficientNetwork](https://paperswithcode.com/method/efficientnet) class in [python](https://keras.io/api/applications/efficientnet/)

>[!important] How to select MaxPooling vs AveragePooling
>
>- If the characteristic I want to find is localized in the image, which means it will have high values in small regions, we should use Max Pooling.
>  E.g. finding a bird in a forest image
>-  If the characteristic I want to find is ditributed on all the image (or most of it), we should use Average Pooling.
>  E.g. a beach in an image, will fill the whole image.

B) **A + fine tuning the whole model:**

```python
# optional fine-tuning of the whole base model 

# unfreeze the base model 
base_model.trainable = True 

# it's important to recompile your model after you make any changes to the `trainable` attribute of any inner layer, so that your changes are take into account 
# the optimizer does not have to be the same as in the first example
model.compile(optimizer=keras.optimizers.Adam(1e-5),# very low l.rate
			  loss=keras.losses.BinaryCrossentropy(from_logits=True),
			  metrics=[keras.metrics.BinaryAccuracy()]) 
			  
# train end-to-end; be careful to stop before you overfit!
# Very low amount of epochs
model.fit(new_dataset, epochs=10, callbacks=..., validation_data=...)
```


X) **OTHER REUSABLE MODELS**:
![[Pasted image 20241217162012.png]]
Z) **Rule of thumb**:
![[Pasted image 20241217162151.png]]

>[!example] Case study: car detection
>
>![[Pasted image 20241217163657.png]]
>
>- Start from the imagenet weights:
>  ![[Pasted image 20241217163726.png]]
>- Use VGG16:
>  ![[Pasted image 20241217163751.png]]
>- Keep the part inside the dashed square (convolutional subnet)
>- Leave the last MaxPooling
>	- We need an output with the same shape as the input
>	- So keep the last layer (512 = 32x32)
>	- Rescale to source somehow (previously mentioned feature pyramid network)
>	- For that, use de-convolution layers (one for each pooling in the model) (lower example below)
>	  ![[Pasted image 20241217164115.png]]

## Domain Adaptation

"*Task is similar, the domain is diferent*"

![[Pasted image 20241217165818.png]]

- Target domain is not annotated
![[Pasted image 20241217170443.png]]
![[Pasted image 20241217170959.png]]
We want to make the model confuse the domains, not differentiate them, so we propagate the domain classifier with an inverted gradient.

![[Pasted image 20241217171046.png]]
![[Pasted image 20241217171201.png]]
```python
(Xs,Ys) #source, 
Xt #target 
	<- training_data() 

#general model | domain predictir | classifier 
model, model_domain, model_source <- models(𝜆)

#create labels for the domain 
do_label = array([0]*batch_size + [1]*batch_size)
do_label_adv = array([1]*batch_size + [0]*batch_size) 

for each update: 
	Xs_b, Ys_b <- next(batch_generator(Xs, Ys)) 
	Xt_b <- next(batch_generator(Xt)) 
	save(𝜃𝑓) 
	
	#train the model chain (green-pink) to classify domains
	#then, lose the weights for the convolution part
	model_domain.train([Xs_b, Xt_b], do_label) 
	restore(𝜃𝑓) 
	save(𝜃𝑑) 
	
	#train the model chain (green-blue-pink), now using the adv labels
	#for the pink one
	model.train([Xs_b, Xt_b], [Ys_b, zeros_like(Ys_b)], do_label_adv) 
	restore(𝜃𝑑)
```

Other examples:
![[Pasted image 20241217165931.png]]
![[Pasted image 20241217172641.png]]