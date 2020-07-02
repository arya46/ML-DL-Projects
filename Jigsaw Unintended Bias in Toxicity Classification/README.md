# Jigsaw Unintended Bias in Toxicity Classification
![header image](https://miro.medium.com/max/788/1*U6grgS7jsycYNrKeF9cQKQ.jpeg)
- [Overview](#overview)
- [Dataset](#dataset)
- [Data Description](#data-description)
- [Project Goal](#project-goal)
- [Architecture](#architecture)
- [Implementation Details](#implementation-details)
- [Results](#results)
- [Directory Tree Structure](#directory-tree-structure)
- [Blog](#blog)
- [References / Useful Resources](#references--useful-resources)

## Overview
The invention of the World Wide Web connected the world in a way that was not possible before. It made much easier for people to get information, share and communicate. It allowed people to share their work and thoughts through social networking sites, blogs, video sharing, etc.

But while the internet and its offshoot technologies have improved society and daily life in many ways, they have also been an unmitigated disaster for the way people communicate online. It has become increasingly common for people to participate in online bullying, abuse and spreading hate. While freedom of speech is important, creating an inclusive platform for meaningful discussion is also necessary.

This is a implementation of one such system that can accomplish the task of tackling the menace of online bullying, and hate.

## Dataset
The dataset is taken from a competition hosted on Kaggle by The Conversation AI team (research initiated by Jigsaw and Google).

Source: https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/data

## Data Description
In the data supplied for this competition, the text of the individual comment is found in the `comment_text` column. Each comment in Train has a toxicity label (`target`), and models should predict the `target` toxicity for the Test data.

The data also has several additional toxicity subtype attributes. Models do not need to predict these attributes for the competition, they are included as an additional avenue for research. Subtype attributes are: `severe_toxicity`, `obscene`, `threat`, `insult`, `identity_attack`, `sexual_explicit`.

Additionally, a subset of comments have been labelled with a variety of identity attributes, representing the identities that are mentioned in the comment. Out of 24 such atrributes provided in the dataset, we consider only the following attributes: `male`, `female`, `homosexual_gay_or_lesbian`, `christian`, `jewish`, `muslim`, `black`, `white` and `psychiatric_or_mental_illness`.

## Project Goal

The goal of the project is to build a model that can classify the toxicity\* of a comment, ie we have to predict if a comment is toxic or non-toxic.

*\* Toxic comments are the comments which are offensive and sometimes can make some people leave the discussion (on public forums).*

## Architecture
![model arch](https://miro.medium.com/max/574/1*52az766UsePpIFzxsXqA1Q.png)

## Implementation Details
- We use predefined word embeddings, viz [Crawl](https://dl.fbaipublicfiles.com/fasttext/vectors-english/crawl-300d-2M.vec.zip) and [Glove](http://nlp.stanford.edu/data/glove.840B.300d.zip) to encode the text sequences.
- We use bi-directional LSTM in the model. **Why bi-directional?** Because they can capture information from both past and future time steps.
- The LSTM layers are coupled with Attention (Luong's) layers. **Why attention?** To select the most representative word in a sentence.
- We make two predictions with our model: target and aux_target. **Why?** Because, the data also has several additional toxicity subtype attributes (severe_toxicity, obscene, threat, insult, identity_attack) that are highly correlated to the target, we also use the toxicity probabilities of these auxiliary targets.
- As such we use two different loss functions: one is used to penalize the weighted target and other is used to penalize the aux_target.
- The model is compiled with `Adam` optimizer
- We use `LearningRateScheduler` to schedule a different learning rate at every epoch.
- We use batch size of 256.
- We train two models, each for 4 epochs. It took 5–6 hours to complete the training process with the free computing resources provided by Google Colab.

## Results
<table>
  <tr>
    <th>AUC Score</th>
    <th>Kaggle Private LB</th>
  </tr>
  <tr>
    <td>0.93623</td>
    <td>629/2633</td>
  </tr>
 </table>

## Directory Tree Structure
```
├── REAMDE.md
├── Jigsaw 0 About Data.ipynb
├── Jigsaw 1 EDA.ipynb
├── Jigsaw 2 Metric.ipynb
└── Jigsaw 3 Models_ LSTM w Luong Attention.ipynb
```
- Jigsaw 0 About Data.ipynb: It contains info about the data and the business problem statements.
- Jigsaw 1 EDA.ipynb: It contains EDA on the data set.
- Jigsaw 2 Metric.ipynb: It contains details about the metric that is used to measure the model performance.
- Jigsaw 3 Models_ LSTM w Luong Attention.ipynb: It contains the implementation of the model. Luong's Attention mechanism is used in the model.

## Blog
An article about this project: 

https://medium.com/swlh/jigsaw-unintended-bias-in-toxicity-classification-a-kaggle-case-study-f47938753347

## References / Useful Resources
[1] https://kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification

[2] https://blog.keras.io/using-pre-trained-word-embeddings-in-a-keras-model.html

[3] https://medium.com/@raghavaggarwal0089/bi-lstm-bc3d68da8bd0

[4] https://www.analyticsvidhya.com/blog/2019/11/comprehensive-guide-attention-mechanism-deep-learning/

[5] https://www.cc.gatech.edu/~dyang888/docs/naacl16.pdf

[6] https://arxiv.org/pdf/1508.04025.pdf

[7] https://brahma0545.github.io/ML/7-seq-seq-models/
