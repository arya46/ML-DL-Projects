# Visual Question Answering
You can find the deployable Flask app [here](https://github.com/arya46/VQA_HieCoAtt).

## Table of Contents:
- Overview
- Architecture
- Dataset
- Implementation Details
- Results
- Repository Files
- Blog
- References / Useful Resources

## Overview
Recent developments in Deep Learning has paved the way to accomplish tasks involving multimodal learning. Visual Question Answering [(VQA)](http://www.visualqa.org/) is one such challenge which requires high-level scene interpretation from images combined with language modelling of relevant Q&A. Given an image and a natural language question about the image, the task is to provide an accurate natural language answer. This is a implementation of one such end-to-end system to accomplish the task.

This repository contains the implementation of "Hierarchical Question-Image Co-Attention for Visual Question Answering" paper in Keras/Tensorflow. Paper: https://arxiv.org/pdf/1606.00061

## Architecture

- STEP 1: Extract image features from a pre-trained CNN (VGG19 is used here).

- STEP 2: Extract features from the questions using the following architecture:

![question features extractor](https://i.imgur.com/D0gJj4M.jpg)

- STEP 3: Combine the images and questions features extracted in STEP 1 and 2:

![combined features](https://i.imgur.com/QClzhta.jpg)

- STEP 4: Use the features from STEP 3 to make final predictions:

![generate answers](https://i.imgur.com/pMg4jYy.jpg)

## Dataset

Data source: https://visualqa.org/download.html

The dataset consists of images and open-ended questions about these images. These questions require an understanding of vision, language and commonsense knowledge to answer.
- 82,783 images (COCO)
- At least 3 questions (5.4 questions on average) per image (443,757 questions)
- 10 ground truth answers per question (443,7570 answers) from unique workers

## Implementation details
- We don’t directly use the image as input into the model. The image is scaled to 224× 224, and then the activations from the last CONV layer of VGGNet19 are extracted. These activations of shape [512 x 7 x 7] are used as the input features for the images.

- We use the KERAS tokenizer to extract question features.

The final input features are: 
- image features of shape [512,49], and 
- question features of shape [22, ], where 22 is the sequence length of the questions after pre-processing.

## Results:
<table>
  <tr>
    <th> Paper </th>
    <th> My Implementation </th>
  </tr>
  <tr>
    <td> 54% Accuracy </td>
    <td> 49.20% Accuracy </td>
  </tr>  
</table>

The following metric was used to calculate the accuracy:


![accuracy](https://render.githubusercontent.com/render/math?math=accuracy%20%3D%20min%28%7B%5Ctext%7B%23%20humans%20that%20provided%20the%20predicted%20answer%7D%20%5Cover%203%7D%2C%201%29&mode=display)

- __Loss Plot__:

![loss plot](https://camo.githubusercontent.com/eeaf20f94efb3c2d8f023c18630ca817713d3e57/68747470733a2f2f692e696d6775722e636f6d2f4e44316e7865332e6a7067)

- __F1 score / Categorical Accuracy Plot__:

![score plot](https://camo.githubusercontent.com/b671a7b29e0e5b115e0c3ac707683a9032d39e27/68747470733a2f2f692e696d6775722e636f6d2f715059577737482e6a7067)

## Repository Files:
```
├── REAMDE.md
├── VQA 1 About Data.ipynb
├── VQA 2 EDA.ipynb
└── VQA 3 Model.ipynb
```
- VQA 1 About Data.ipynb: It contains info about the data and the business problem statements.
- VQA 2 EDA: It contains EDA on the data set.
- VQA 3 Model: It contains the implementation of the paper.

## Blog:
An article about this project: https://towardsdatascience.com/visual-question-answering-with-deep-learning-2e5e7cbfdcd4

## References / Useful Resources:
- Jiasen Lu, Jianwei Yang, Dhruv Batra, and Devi Parikh, [Hierarchical Question-Image Co-Attention for Visual Question Answering](https://arxiv.org/pdf/1606.00061) (2016)
- Stanislaw Antol and Aishwarya Agrawal and Jiasen Lu and Margaret Mitchell and Dhruv Batra and C. Lawrence Zitnick and Devi Parikh, [VQA: Visual Question Answering](https://arxiv.org/pdf/1505.00468.pdf) (2015)
- https://github.com/VT-vision-lab/VQA_LSTM_CNN
- https://github.com/ritvikshrivastava/ADL_VQA_Tensorflow2
