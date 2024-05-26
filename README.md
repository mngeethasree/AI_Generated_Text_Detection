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
Generated a model architecture using 3 hidden layers and an output layer 





