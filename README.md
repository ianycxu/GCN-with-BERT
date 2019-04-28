# RGCN-with-BERT
Gated-Relational Graph Convolutional Networks (RGCN) with BERT for Coreference Resolution Task

## Introduction
We propose a series connection architecture of BERT and Gated-RGCN. This architecture allows model to encode each token with both its contextual information and its syntactic neighbour's information. The overall architecture is shown below.
![](https://i.imgur.com/aAK43SM.png)

## What is R-GCN?
### Graph Neural Network
Graph neutral networks are deep learning models that operates on graphs. In GNN, node's neighborhood defines a computation graph, each edge in the graph is a transformation or aggregation function, and nodes will aggregate information from neighbors using neural networks. 


### Graph Convolutional Network
One way to train GNN is naively concatenating adjacency matrix and feature matrix, feeding them into deep neural network. But it's very expensive to train GNN that all the nodes have different weights. And we have to retrain GNN if number of nodes changes. To avoid these problems, we can use Graph convolutional network. "Convolutional" means that all locations in the graph share the same filter parameters. 

![](https://i.imgur.com/EaUO7a4.png)

For each node $v_i$, the feed-forward process or message-passing processing then can be written as:
$$h_i^{(l+1)} = ReLU\left(\sum_{u\in \mathcal{N}(v_i)}\frac{1}{c_i} W^{(l)} h_u^{(l)}\right)$$



### Gated Relational Graph Convolutional Networks
The final GCN structure we used is called R-GCN, which is an extension of GCNs to large-scale relational data. In our task, we have three relations in our edges.


![](https://i.imgur.com/fR6LqEJ.png)

(van den Oord et al., 2016; Dauphin et al., 2016; Marcheggiani1 et al., 2017) introduce a gate mechanism to GCN. The idea is calculating a gate value range from $0$ to $1$, and multiplying it with incoming message. The gate value is computed by:
$$g^{(l)}_{u,v} = sigmoid\left( h_{u}^{(l)} \cdot W_{r,g} \right)$$
The final forward structure of Gated-RGCN is:
$$h_i^{(l+1)} = ReLU\left(\sum_{r\in R}\sum_{u\in \mathcal{N}(v_i)^r}g^{(l)}_{u,v_i}\frac{1}{c_{i,r}}W_r^{(l)}h_u^{(l)}\right)$$




### Concatenation
We concatenate GCN's hidden states of two mentions and the pronoun with contextual embeddings of two mentions and the pronoun. Then we feed the concatenation into a MLP for classification. This concatenation is proven to be helpful for final performance.

