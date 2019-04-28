# RGCN-with-BERT
Gated-Relational Graph Convolutional Networks (RGCN) with BERT for Coreference Resolution Task

## Introduction
We propose a series connection architecture of BERT and Gated-RGCN. This architecture allows model to encode each token with both its contextual information and its syntactic neighbour's information. The overall architecture is shown below.
![](https://i.imgur.com/aAK43SM.png)



## Dataset we have
The data set is Gendered Ambiguous Pronouns (GAP), which is a gender-balanced dataset containing 8908 coreference-labeled pairs sampled from Wikipedia. The dataset contains samples Each sample contains a small paragraph that mentions the potential subject's names later refered by a target pronoun. It also came up with two candidate names for the resolver to choose from. Columns contains: 

|  Header        | Description     | 
| :------------- | :----------: |
|  <strong>ID</strong> | ID for this sample   | 
|  <strong>Text</strong> | Text containing pronoun and two names   | 
|  <strong>Pronoun</strong> | Target pronoun in text   |
|  <strong>Pronoun-offset</strong> | Character offset in text   |
|  <strong>A</strong> | Name A in text   |
|  <strong>A-offset</strong> | Position of A in the text |
|  <strong>A-coref</strong> | Whether A confers this pronoun |
|  <strong>B</strong> | Name B in text |
|  <strong>B-offset</strong> | Position of B in the text |
|  <strong>A-coref</strong> | Whether B confers this pronoun |


## What is R-GCN?
### Graph Neural Network
Graph neutral networks are deep learning models that operates on graphs. In GNN, node's neighborhood defines a computation graph, each edge in the graph is a transformation or aggregation function, and nodes will aggregate information from neighbors using neural networks. 
### Graph Convolutional Network
One way to train GNN is naively concatenating adjacency matrix and feature matrix, feeding them into deep neural network. But it's very expensive to train GNN that all the nodes have different weights. And we have to retrain GNN if number of nodes changes. To avoid these problems, we can use Graph convolutional network. "Convolutional" means that all locations in the graph share the same filter parameters. 
### Gated Relational Graph Convolutional Networks
The final GCN structure we used is called R-GCN, which is an extension of GCNs to large-scale relational data. In our task, we have three relations in our edges.



### Concatenation
We concatenate GCN's hidden states of two mentions and the pronoun with contextual embeddings of two mentions and the pronoun. Then we feed the concatenation into a MLP for classification. This concatenation is proven to be helpful for final performance.

## Data Preprocessing

We use SpaCy as our syntactic denpendency parser. DGL is used to transfer each dependency tree into a graph object. This DGL graph object then can be used as the input for GCN model which is also implemented by DGL. Several graphs are grouped together as a larger DGL batch-graph object for batch training setting.

## Ensemble method
Five-fold ensemble is used to achieve a better generalization performance. We divide our training dataset into 5 folds. Each time of training we train our model on 4 folds and choose the model which has the best validation performance on the left fold. This best model then is used to predict the test dataset. At the end of 5-fold, we average 5 predicted results as the final result. All of our experiments has been conducted 5-fold 

## Conclusions
We present a noval approach for coreference resolution task by connecting Gated-RGCN with BERT seriesly. R-GCN is used for digesting syntactic denpendency tree and we assume that this syntactic information will help our semantic task. We use this architecture in the Google AI GAP competition and rank top 8%.
