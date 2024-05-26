# AI_Generated_Text_Detection

## Objective:

This project aims to classify if a text is AI generated utilizing sentence embeddings of 768 dimensions created from text. The text used to generate these embeddings are not disclosed as a part of this challenge. But the text is encoded into 768 dimensionsal vector. The sentence embeddings are similar to word embeddings. Just as words that are quite similar to each other have word embeddings that are closer to each other, similar sentences are encoded into embeddings that are closer to each other in 768 dimensional space (Distance between embeddings of similar sentences is lower compared to that of dissimilar sentences).

## Data:

Our Dataset contains total of 772 features. <br>
a. ID : unique identifier for each row of text data. <br>
b. feature0 to feature 767 : 768 Dimensional sentence embedding representation. <br>
c. word_count : number of words in the sentence. <br>
d. punc_num : count of punctuations used in sentence. <br>
e. ind : indicator flag for identifying AI generated text. Target variable for our binary classification problem. <br>

## Exploratory Data Analysis:

**1. Distribution of Mean and Standard deviation of 768D embeddings:** <br>
<div align="center"><img width="518" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/fef09778-1a8a-4f57-86a5-37290f54aa2a"></div> <br>

Mean of embedding features is at 0 and standard deviation is centered around 0.5

**2. Distribution of Target variable:** <br>
<div align="center"><img width="327" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/0f5f137a-1b13-4173-a946-f5213aa52489"></div> <br>

Target value is highly imbalanced, as number of rows corresponding to AI generated text are quite less in the entire dataset.

**3. Comparison of word count between Humand and AI generated texts:** <br>

<div align="center"><img width="521" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/f707070c-6953-4bb4-a4d6-4943bc51100c"></div> <br>

Human generates sentences are slightly longer than AI generated ones. However, there doesn't seem to be lot of seperation between both the distributions.

**4. Comparison of Punctuation count between Humand and AI generated texts:** <br>

<div align="center"><img width="492" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/7c51af02-43eb-4226-94cf-980e02502eb3"></div> <br>

Punctuation count used by Humans are widely distributed than that of AI generated texts.

**5. Top 2 Principal Components:** <br>

We can utilzie PCA to reduce 768 dimensional vector to top 2 principal components that can explain almost 25% of variance in the actual data and then visualize these principal components for Human vs AI generated texts. <br>

<div align="center"><img width="488" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/8b005354-60bd-4319-8870-a154a6d39644"></div> <br>

Data points for both the classes are mostly overlapping and there is no clear seperation between both the classes. However, this is expected, as top 2 principal components alone would not be sufficient enough for the classification model to be able to clearly distibguish between both the classes. <br>


**6. t-SNE visualization :** <br>

Using t-SNE to visualize 768 dimensional embeddings in 2 dimensions for both the classes. <br>

<div align="center"><img width="483" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/d50db5ed-cbfb-4465-ab58-594994ddef20"></div> <br>

## Feature Engineering:

Since the 768 embedding features are not directly interpretable, they cannot be utilized to create any new features that can help in the classification problem. We have created three new featuers from word_count and punc_num features. <br>

**1. word_per_punc:** This feature is calculated by dividing word count by punctuation count. It represents average number of words used per each punctuation character. A small epsilon is added in the denominator as it is possible to have a sentence with zero punctuation characters. <br>
**2. punc_per_word:** This feature is calculated by dividing punctuation count by word count. It represents average number of punctuation characters used per word. An epsilon is not required here as word count cannot be zero. <br>
**3. word_plus_punc:** This feature is calculated by sum of word count and punctuation character count.

## Train Test Split:
Used 90-10 train-test split on the entire data.

## Data Balancing:
Since the target variable is highly imbalanced, balancing both the classes before training the model could help in better classification. After trying out various techniques like SMOTE, SMOTETomek, Random OverSampler, Random UnderSampler from imblearn, the performance metric doesn't seem to improve with the balanced data.

## Standard Scaler:
Standard scaling all the features before modeling to normalize ranges of all the features to have zero mean and unit standard deviation.

## Dimensionality Reduction on 768 D embedding features
After applying dimensionality reduction techniques like PCA and SVD to project embeddings to lower dimensional space explaining 95% of original variance, there doesnt seem to be any improvement in performance metric. 

## Model Architecture: 
Generated a model architecture using 3 hidden layers and an output layer. <br>
Added Batch Normalization layers to ensure that network trains faster and to ensure stable learning during the model training. <br>
Adding dropout layers helped avoid overfitting by randomly excluding some nodes at each training run, thereby introducing randomness and hence improving the network's generalization on new data. <br>

Model is trained with Early Stopping Callback to stop training the network when validation accuracy doesnt seem to improve even after 20 training epochs. <br>

Upon fitting the model, the training process stopped after 50 iterations when test f1-score reached 0.75. 

## Learning Curves for Loss and Accuracy

<div align="center"><img width="335" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/62202e2a-1ef5-4981-b194-0dd0a59015c8"> 
<img width="331" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/016c97a2-b00e-4d25-aa51-d5dacf9e5516"></div> <br>

Upon examining the learning curves for accuracy and loss, it can be observed that the seperation between train and validation metrics slowly starts to decrease as training process continues, and then increases after certain number of epochs. However, since early stopping callback is used, best weights are automatically stored in the model.

## Final Model Performance metrics:

Comparison of Confusion Matrix for Train and Test Datasets
<div align="center"><img width="473" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/474d3a19-51fa-46a0-a3c9-00375e005973"></div> <br>

## Model Interpretation:
Although neural network is not directly interpretable, we can use techniques like permutation importance to estimate the importance of features. After running permutation importance, results are as follows. <br>

<img width="363" alt="image" src="https://github.com/mngeethasree/AI_Text_Detection/assets/68059811/35752ee3-0fee-4d5d-8d3c-7fbb81a42d0b"> <br>

word_count, word_plus_punc features emerge as top 2 features indicating that the impact on model performance is highest by changing these two features. <br>




