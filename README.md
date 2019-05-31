# Look Again at the Syntax: Relational Graph Convolutional Network for Gendered Ambiguous Pronoun Resolution

## Original Paper
 https://arxiv.org/abs/1905.08868

## Introduction
We propose an end-to-end resolver by combining pre-trained BERT with Relational Graph Convolutional Network (R-GCN). R-GCN is used for digesting structural syntactic information and learning better task-specific embeddings. Empirical results demonstrate that, under explicit syntactic supervision and without the need to fine tune BERT, R-GCN's embeddings outperform the original BERT embeddings on the coreference task. Our work obtains the state-of-the-art results on GAP dataset, and significantly improves the snippet-context baseline F1 score from 66.9% to 80.3%. We participated in the 2019 GAP Coreference Shared Task, and our codes are available online. The overall architecture is shown below.
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



## Data Preprocessing

We use SpaCy as our syntactic denpendency parser. DGL is used to transfer each dependency tree into a graph object. This DGL graph object then can be used as the input for GCN model which is also implemented by DGL. Several graphs are grouped together as a larger DGL batch-graph object for batch training setting.
