# Toxic-Comment-Classification
## Text classification using Convolutional Neural Network and Glove Embeddings

### Abstract
Classification of comments is a problem of Natural Language Processing (NLP). NLP is a branch of computer science which helps the computer to read, understand and interpret human speech. A maximum testing accuracy of 99.8% was achieved using pre-trained Glove embeddings and Convolutional Neural Network. A training accuracy of 83% was observed. Accuracy of the model significantly increased after optimising the batch size and number of epochs using grid search. The learning rate, dropout rate and filter size were also optimised. Additionally, under sampling the data improved the classification of texts.  It was observed that the classes with very less samples in the training data were difficult to predict correctly. Confusion matrix were plot separately for each of the six labels and the performance was noted.

### Introduction
Recognising the nature comments online is an important objective to eradicate online harassment and abuse. There are a large number of social networking platforms available today where people can share their content and personal views without any moderation. There is a need to develop algorithms which can identify negative or toxic nature of a comment to prevent the possibility of any harm to the community . In recent years, there have been some cases where social media users were arrested because of their negative content or comments towards the society or a government authority on social media sites.
Classification of toxic comments is a problem of Natural Language Processing (NPL). NLP helps computers read, understand and interpret human language. It is being widely used for sentiment analysis, language translation, text extraction, chatbots etc.  Nowadays, deep learning has become a popular method for modelling human speech. Deep learning algorithms can be effectively used for the problem of classification of online comments. Classifying comments accurately is a step towards providing a safer online community for everyone.

### Dataset 
The dataset used for this project is available on Kaggle under the title, “Toxic Comment Classification Challenge”. This challenge is a part of Conversation AI team’s (founded by Jigsaw and Google) initiative to improve their current model performance to classify online comments. The dataset is a collection of comments from Wikipedia Talk page edits. The “train.csv” file contains the training data and “test.csv” contains the testing data. The file “test_labels.csv” contains the testing labels. The training data has 159571 rows and 8 columns. The columns names are “id”, “comment_text”, “toxic”, “severe_toxic”, “obscene”, “threat”, “insult” and “identity_hate”. The id column lists all the ids of the comments. The column “comment_text” contains all the comments which need to be classified. The rest of the six columns are the labels for a comment. This problem is a multi-class classification problem as there are six classes. The training labels have a value of either 0 or 1. A value of 1 in the label column for a text comment corresponds to presence of that label in that comment whereas a value of 0 means that the label is not present. The “test.csv” contains comments to be used to perform testing and predict the test labels. 

### Implementation
A Jupyter notebook with Python kernel was used to perform all the tests. TensorFlow and Keras were the primary libraries used for importing neural network layers, loss function, activation function, embeddings and some pre-processors. Sklearn, Pandas and NumPy libraries were also frequently used for data loading, manipulation and transformation. Seaborn and matplotlib library were used to plot visualisations.
The Id column was dropped from the training and testing data as it is not useful for predictions. No null values were found in the dataset. A text cleaning function was implemented which was used to remove symbols like dash, colon, equal to or a newline or tab. These symbols were not useful for making predictions for a comment. Since there was an imbalance distribution of labels, under sampling was performed. It was observed that approximately 90 percent of all the comments are not any kind of toxic as 143346 rows out of 159571 rows of training data did not have any label classified as 1. Oversampling could also be performed but under sampling the data was chosen due to the presence of very few comments in certain labels. Under sampling was performed by dropping 35 percent of those rows in which comments of the rows had all zero values for the labels. This percentage was chosen so as to not lose too much of the training data.
The text data was tokenized using Tokenizer and padded using Sequence. The most frequent 20k words in the dataset were kept with a maximum text length of 400 letters. Glove algorithm was used to obtain vector representations of words. Glove is an unsupervised learning algorithm which can map words together in a space using semantic similarity. It is open sourced and can be downloaded from “http://nlp.stanford.edu/data/glove.6B.zip”. The 100-dimension glove embedding was loaded for this project. This embedding contained 400 thousand word vectors. The vector representations were then stored in an embedding matrix.
The training features and labels were split into training and validation sets with an 80:20 ratio. Some utility functions were defined to help with making plots, modelling and testing. A function to plot confusion matrices for each label was also defined.

### Deep Learning
For this project, a convolutional neural network (CNN) architecture was implemented. Convolutional neural network was chosen as it performs well when there is a spatial relationship between the data. Therefore, it usually has a good performance with text classification problems. The convolutional neural network model architecture was inspired from the model proposed by Saif et al. [4] in their research. The first layer which takes the input is an embedding layer. The constant initialiser was used for this layer. The input layer is followed by a dropout layer with drop rate set to 0.2. A convolutional 1 D layer follows this with a kernel size of 3 and 250 filter size. The next layer is a max pooling layer. Second convolutional layer follows this, with a kernel size of 5 and 250 filter size. After this, there is a global max pooling layer which reduces dimensionality across all the convolutional layers. This layer is followed by a dense layer with 250 filters and Relu activation. A dropout layer with a dropout rate of 0.2 follows this. Then there is a final dense output layer which has six output channels and sigmoid activation. Loss was evaluated using the binary cross entropy function. This loss was used since we have a classification problem with binary predictions. Adam optimizer was used to minimise the loss function with the learning rate set to default value. Adam optimization algorithm is an extension of the Stochastic Gradient Descent algorithm.

### Hyperparameter Tuning
- The initial model achieved an accuracy of approximately 83 percent. The learning curve in figure 5 shows that the model might be over fitting the data. The model can be optimised further using various different settings and thus find the optimum parameters for this method. Grid Search CV was used to optimise batch size, number of epochs, optimizer, learning rate, filter size for convolutional layer and the dropout rate. From the initial learning curve of the model, shown in Figure 5, it can be observed that the model converges with just a few epochs. The validation accuracy quickly reaches a very high accuracy. Firstly, the number of epoch and batch size was optimised. The model was fit into Keras classifier and Grid Search was used to find the optimum values. Grid search is quite time exhaustive.  Therefore, only a small number of cross validation score was chosen, and the first 30,000 samples of training data were used for computations. The number of epochs to fit into the Grid Search was chosen low as the model converges very fast. Values between 3 to 6 were chosen to be used in grid search. Batch sizes between 32 to 100 picked. A batch size larger than 100 might generalise the data and the predictions might not be accurate. From Table 1 it can be observed that a batch size of 32 and 4 epochs achieved the highest accuracy. These are the same values which were used in the initial model. From the other batch size and epoch combinations, it can be noted that the model mostly performs well with number of epochs set to 3 or 4.

- After optimising the batch size and epoch value, different optimizers were used to fit the model. The optimizers SGD, Adagrad, Adadelta, Adam,Adamax and Nadam were used to fit the model. After performing a grid search, it was observed that the SGD optimiser performs best with an accuracy of 98.54. Therefore, SGD optimiser was used for further optimisation.

#### Batch size,	Number of Epoch,	Optimizer,	Accuracy 
#### 32              	4	             Adam       	0.990
#### 40	6	Adam	0.989
#### 50	4	Adam	0.988
#### 80	3	Adam	0.988
#### 100	3	Adam	0.986

- Different learning rates between 0.0001 and 0.1 were used to fit the model. The learning rate of 0.1 achieved an accuracy of 99.09 %.  However, choosing a very high learning rate can lead to the model learning too quickly from the data. Therefore, the second-best accuracy of 99.07% obtained with a learning rate of 0.02 was selected for further optimisation.

#### Batch size,	Number of Epoch,	Optimizer,	Dropout Rate,	Number of filters in CNN layers,	Accuracy 
#### 32	4	SGD	0.0	400	0.99060
#### 32	4	SGD	0.1	250	0.99063
#### 32	4	SGD	0.2	200	0.990867
#### 32	4	SGD	0.3	250	0.99083
#### 32	4	SGD	0.4	100,250	0.990867

- Different combination of dropout rate and filter size in convolutional layers were selected and fit into Grid Search to find the optimum values. Dropout rate between 0 to 0.4 were selected and filter sizes between 100 to 400 was chosen.  From Table 2, it can be noted that the highest accuracy obtained was with a dropout rate of 0.2 and 0.4. The dropout rate of 0.4 and filter size of 250 was selected from this Table. This setting was chosen as the filter size of 250 mostly resulted in high accuracy with other combinations of dropout rate.
After optimising the beforementioned parameters, the optimised values were fit into a new model. Figure 5 shows the learning curve plot for the final model. 

- Confusion matrices are a very informative performance measure metric. Confusion matrices for each of the label were plot separately. The left diagonal boxes represent the number of correctly classified samples. True negative means that the comment was actually not present in the respective class and it was classified correctly by the model whereas false negative means that the class was present in the comment text, but it was misclassified by the model. It can be observed that the classes threat and identity hate are not being classified correctly for true positive. The classes toxic, severe toxic obscene and insult are classified more accurately but still have many misclassifications.

### Evaluation
Accuracy was the primary metric used for evaluating the model for classification of toxic comments. The accuracy measures how many of the comments were classified correctly. A testing accuracy of 0.9989 and a loss of 0.16 was achieved. Validation accuracy is also a good indicator of a model’s performance ability in testing. There is a scope of improving the model’s performance further. The final model was not able to predict the classes threat and identity hate correctly. A larger training data and further optimisation can be helpful for predicting these classes correctly. Hard sample mining technique could also be used for selecting the hard samples for a batch and train the model using the hard samples (the samples for which predictions were mostly incorrect) after a few initial epochs.

