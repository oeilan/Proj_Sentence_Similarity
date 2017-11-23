# Sentence similarity prediction model

This model uses English-English sentence pairs with similarity score data from the Semantic Evaluation competition 2017.
Data is downloaded from http://ixa2.si.ehu.es/stswiki/images/4/48/Stsbenchmark.tar.gz

The model objective is to predict a similarity score between 0 and 5 given a pair of english sentences, and achieve a high pearson correlation for predicted scores and benchmark scores given in the dataset.

Model uses a pretrained Glove embedding to represent words in the sentences.
Several configurations of the 3-layered model is tested and results for 4 configurations are attached below.

The first layer is a non-trainable embedding layer loaded with pretrained glove vectors of 50 and 300 dimension.

This is followed by a middle LSTM, Bidirectional LSTM or stacked Bidirectional LSTM layer of different output dimensions 

The output layer computes a (normalised) dot product of the 2 output vectors from the middle layer for the sentence pair to yield a predicted score. As this predicted score is between 0 and 1, the 0-5 benchmark scores were normalised for comparison and cost/loss function computation.

Two loss functions were used: (1) minimizing mean square errors and (2) maximizing pearson correlation.

Some observations:
- Birectional LSTM appears to be more quicker (ie after less epochs) at reaching best results
- Little increase in train time when glove vectors of 300 dimensions are used instead of 50, 30% more train time instead of 6x
- 2 stacked LSTM did not better results of one LSTM
- best pearson correlation obtained among all configurations of this simple model is 0.67,
  substantially below competition best of 0.85, 
  which used significant text preprocessing to obtain more word/sentence features
  before feeding into an SVM regression model

Results:
                                                    
    1. Glove embedding 50d  -> shared LSTM(256)   -> dot                :   314,368 param : 0.64 p_corr, 0.95 mae
    2. Glove embedding 50d  -> shared BiLSTM(256) -> dot                :   628,736 param : 0.65 p_corr, 1.00 mae
    3. Glove embedding 300d -> shared BiLSTM(128) -> dot                :   439,296 param : 0.67 p_corr, 0.85 mae
    4. Glove embedding 300d -> shared BiLSTM(128) -> BiLSTM(128) -> dot :   833,536 param : 0.66 p_corr, 0.90 mae
    5. Glove embedding 300d -> shared BiLSTM(256) -> dot                : 1,140,736 param : 0.70 p_corr, 0.85 mae
    6. Glove embedding 300d -> shared BiLSTM(600) -> dot                : 4,324,800 param : 0.69 p_corr, 0.90 mae

				MS pre-trained		BiLSTM(256)
				Sent2Vec for windows	max pearson
pearson correlation		0.74			0.74
mean error			0.09			0.32
mean absolute error		0.79			0.83
		
% prediction within [] points out of 5		
0.5				37.66%			40.83%
1				68.28%			66.50%
1.5				87.30%			83.76%
2				95.96%			92.82%
2.5				99.02%			97.39%
