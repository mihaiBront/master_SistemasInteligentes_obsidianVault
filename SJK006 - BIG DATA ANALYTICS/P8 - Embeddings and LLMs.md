# 8.0 Natural Language Processing Tasks

![[Pasted image 20241106181335.png]]

## Traditional machine learning applied to texts:
![[Pasted image 20241106181416.png]]

- Bag of Words: Text is split in words and words are weighted by some indexing scheme (TF, TF-IDF, …) 
- Feature selection (vocab reduction) 
- Each task has its own labeled data: 
	- Sentiment analysis (+/-) 
	- Topic classification 
	- Regression for rankings 
	- etc. 
- Each task adopts the most suited model for predictive analysis
	- Logistic regression 
	- XG-Boost 
	- Decision trees (Random Forests) 
	- Naïve Bayes 
	- etc.

**Drawbacks**:
1. "*Long Tail*"
	![[Pasted image 20241106181649.png]]
	- The “Long tail” issue has several implications: 
		- We can always find a new word not seen before (OOV - Out of Vocabulary) 
		- We only have enough statistics for a small set of words 
		- Overfitting in traditional machine learning methods 
	- Semantics are difficult to capture with traditional models
		- Synonymy, antonymy, etc. 
		- Frequent terms are often ambiguous 
		- They are used in many different senses in different contexts. 
		- Combinations of frequent terms can also be ambiguous 
	- Any collection will be heavily biased towards certain topics
		- Large latent class imbalance 
		- Few very big topics, many very small topics

## Neural-Based word embeddings

-  Main Methods: 
	- Word2Vec [seer DEMO Projector TF](https://projector.tensorflow.org/)
	- [GloVe](https://nlp.stanford.edu/projects/glove/)
	- [FastText](https://fasttext.cc/) (embeddings sub-word level) 
- Addresses some main problems of traditional text representation: 
	- Long tail: semantics come from the context regardless the word frequencies
	- Dense vectors instead of sparse vectors (suited for linear algebra ML methods). 
- But other issues still present: 
	- Averaging word embedding do not account for word ordering 
	- An ambiguous word (e.g. bank) is only associated to one vector 
	- Words out of vocabulary have no vector (OOV)

>[!question] What if...
>...there is a word the model has never seen? 
>- That's when "*tokenizing*" comes in place -> Dividing words into parts used as a "radical" for words, so, if the new word is a verbal form, it will be able to determine its meaning without retraining the whole model.
>- If the new word is totally unexpected (such as the scientific name of a plant), we could also find a synonyms dictionary and indexing them the same way

## Linear substructures
*From GloVe*:
>The similarity metrics used for nearest neighbor evaluations produce a single scalar that quantifies the relatedness of two words. This simplicity can be problematic since two given words almost always exhibit more intricate relationships than can be captured by a single number. For example, _man_ may be regarded as similar to _woman_ in that both words describe human beings; on the other hand, the two words are often considered opposites since they highlight a primary axis along which humans differ from one another.
>
>In order to capture in a quantitative way the nuance necessary to distinguish _man_ from _woman_, it is necessary for a model to associate more than a single number to the word pair. A natural and simple candidate for an enlarged set of discriminative numbers is the vector difference between the two word vectors. GloVe is designed in order that such vector differences capture as much as possible the meaning specified by the juxtaposition of two words.

![[Pasted image 20241106183659.png]]
![[Pasted image 20241106183705.png]]![[Pasted image 20241106183711.png]]![[Pasted image 20241106183715.png]]
# 8.1 Deep learning architectures

![[Pasted image 20241106184046.png]]

- **CONVOLUTIONAL**: Based on a multiplication of two filters
	- Drawbacks: Order of inputs is not important
	
- **RECURRENT**: sequential calculations from first to last word (non parallelizable)
	
- **TRANSFORMERS** !
	- Encoders
	- Decoders


## Attention 

![[Pasted image 20241106185133.png]]

KEY POINTS: 
- Divide words into tokens (tokenizer) 
- Self-supervised + Fine tuning
- Token position embeddings
- Self-attention layers 
- Predict with all → encoder 
	- Predict with mask → decoder

![[Pasted image 20241106190532.png]]

Where:
- Q = query
- K = 