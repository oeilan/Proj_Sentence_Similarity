# Sentence similarity prediction model

This model uses English-English sentence pairs with similarity score data from the Semantic Evaluation competition. Data is downloaded from http://ixa2.si.ehu.es/stswiki/images/4/48/Stsbenchmark.tar.gz

The model objective is to predict a similarity score between 0 and 5 given a pair of english sentences, and achieve a high pearson correlation for predicted scores and benchmark scores given in the dataset.

This is a first cut model (using keras) to attempt the above objective.

Model uses a pretrained Glove embedding to represent words in the sentences. A training sample comprising of a sentence pair is fed into 2 separate networks, one for each sentence. This network has 2 layers: a non-trainable embedding layer followed by a LSTM layer.

The vector output of the LSTM for each sentence is normalized and cosine distance is computed for the 2 unit vectors to generate a similarity score.
