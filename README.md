# Universal Language Model Fine-tuning for Text Classification

A brief overview of the paper by Howard and Ruder (2018) that brought transfer learning to NLP. 

## Overview

**Context**: 

* At the time of publishing, transfer learning was a practice used in computer vision, but had not yet been *successfully* brought to NLP. 
* Existing language models required millions of documents to fine-tune on a different domain and existing fine-tuning approaches only worked on the very last layer of these models.

<p align="center">
    An abbreviated history of NLP
</p>

![nlphist](img/nlp_history_2.png)


In Howard and Ruder (2018),

* The authors proposed the **Universal Language Model Fine-tuning**, a three-step process to incorporate transfer learning into NLP. 
* This model was said to be *universal* because it provided a foundational language model that could be fine-tuned on any corpus for any text classification task (ex: sentiment analysis, topic classification, etc.)

The three steps are as follows:

![ulmfitsteps](img/ulmfit_steps.png)

1. **Pretrain a large language model**: Captures general features of language
   - Requires large, general corpus. Here, they use WikiText with 28k articles, 103M tokens
   - The model used was AWD-LSTM (Average SGD Weight-Dropped Long Short-Term Memory). LSTM models had sequential processing and the Markov property; they pre-date transformers
   - 1x cost
2. **Fine-tune language model**: Learns data-specific features
   - Requires texts from domain of interest
   - Works with even "small" datasets
3. **Target task classifier fine-tuning**: Transfer learning to a target task
   - Fine-tune the model from step 2) on a classification task
   - Adds two linear blocks and a final RELU activation and softmax; only params in the model that are trained from scratch
   - *Gradual unfreezing*: Instead of fine-tuning all classification layers at once, they propose freezing all layers and gradually unfreezing starting from the layer with the least general knowledge (the last layer)

Results below suggest that this approach works far better than working from scratch, even when the dataset is very small (~100 observations).
![ulmfitres](img/ulmfit_results.png)

Does this sound familiar? It should if you have background with transformers! This process in conceptually similar, and this paper was released only mere months before BERT. 


Further, the authors introduced two novel techniques to improve fine-tuning (steps 2 and 3 above):
* **Discriminative fine-tuning**: 
    - Different layers of the model capture different types of information, so these layers should be fine-tuned to different extents
    - Each layer is fine-tuned with different learning rates, where 𝜂<sup>L-1</sup> = 𝜂<sup>L</sup>/26
![discft](img/disc_ft.png)

* **Slanted Triangular Learning Rates**:
    - The desired fine-tuning behavior is for the model to quickly converge to a desirable parameter space, and then spend the rest of the time refining
    - Suggested solution is to linearly increase the learning rate and then linearly decay it, resulting in the behavior graphed below: 
![slantedlr](img/slanted_lr.png)

Critical Analysis:
* _What is overlooked?_ This model is unidirectional and focuses on text classification. How would this be expanded for something like text generation or translation?
* _Have others disputed the findings?_ Not necessarily, but their approach to this setup (fine-tune language model, fine-tune on dataset, transfer knowledge to another classification task) is obsolete. LSTM not as efficient or accurate as transformers. 

## Discussion 1
How could this be altered to use transformers?

## Discussion 2
How would slanted learning rates and discriminative fine-tuning combine to acheive robust fine-tuning? (*Hint:*)

## Discussion 3


## Resource Links

- Link to the paper, [Universal Language Model Fine-tuning for Text Classification](https://arxiv.org/abs/1801.06146)
- ULMFiT, [the Layman's introduction](https://nlp.fast.ai/classification/2018/05/15/introducing-ulmfit.html) 
- [BERT vs. ULMFiT]()
- [Example code from fast.ai]()
