# data-mining

In this repository, we provide implementations of
several data mining algorithms, including:
- [1. MapReduce for finding nodes with many mutual neighbours](#1-mapreduce-for-finding-nodes-with-many-mutual-neighbours)
- [2. A-Priori algorithm for frequent itemset mining](#2-a-priori-algorithm-for-frequent-itemset-mining)
- [3. MinHash-based LSH for finding similar documents](#3-minhash-based-lsh-for-finding-similar-documents)
- [4. k-Means algorithm using Spark](#4-k-means-algorithm-using-spark)
- [5. Collaborative filtering](#5-collaborative-filtering)
- [6. PageRank algorithm using Spark](#6-pagerank-algorithm-using-spark)
- [7. Efficient triangle counting](#7-efficient-triangle-counting)
- [8. Gradient Descent SVM](#8-gradient-descent-svm)
- [9. DGIM algorithm](#9-dgim-algorithm)

The implementations are written from scratch in Python and Spark.

*Notes*

Due to privacy issues, this repository only includes the final code
and documentation without any commit history.

## Install pre-requisites

Some of the implementations in this repository require
Spark, which can be installed with the following steps:
1. Download Spark
```
wget https://archive.apache.org/dist/spark/spark-3.1.2/spark-3.1.2-bin-hadoop2.7.tgz
```
2. Unzip the downloaded file
```
tar -zxvf spark-3.1.2-bin-hadoop2.7.tgz
```
3. Check that the Spark binary `/spark-3.1.2-bin-hadoop2.7/bin/spark-submit` exists
4. Install Open JDK 8
```
$ sudo apt-get update
$ sudo apt-get install openjdk-8-jdk
```

## Documentation

In this section, we describe the implemented algorithms
and provide example usage.

### 1. MapReduce for finding nodes with many mutual neighbours 

Given a graph, we want to find pairs of nodes that have
the highest number of mutual neighbours. We implement
this algorithm using MapReduce.

#### Input and output format

The input file is a text file with each line representing
the list of neighbours of a node:
```
<Node><TAB><Neighbours>
```
We provide a sample dataset in `data/soc-LiveJournal1Adj.txt`.

The output contains the 10 pairs with the highest
number of mutual neighbours, where each line is in the format 
```
<Node1><TAB><Node2><TAB><CountOfMutualNeighbours>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
bin/spark-submit code/mutual_neighbour.py data/soc-LiveJournal1Adj.txt
```

You can compare the result with an alternative, non-MapReduce algorithm
we implemented by running the following command:

```
python code/mutual_neighbour_scipy.py data/soc-LiveJournal1Adj.txt
```

### 2. A-Priori algorithm for frequent itemset mining

Given a collection of itemsets, we find frequent itemsets
using the A-Priori algorithm.

#### Input and output format

Each line in the input file is a set of items separated by a spaces
```
<Item1><SPACE><Item2><SPACE><Item3> ...
```
We provide a sample dataset in `data/browsing.txt`.

The output contains the number of frequent items, the 
number of frequent pairs, and the 10 most frequent pairs:
```
<NumberOfFrequentItems>
<NumberOfFrequentPairs>
<Item><TAB><Item><TAB><Count>
...
<Item><TAB><Item><TAB><Count>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/a_priori.py data/browsing.txt
```

### 3. MinHash-based LSH for finding similar documents

Given a collection of documents, we find similar documents
using the MinHash-based locality-sensitive hashing (LSH) algorithm.

#### Input and output format

Each line in the input file has the following format:
```
<DocumentID><SPACE><Document>
```
We provide a sample dataset in `data/articles.txt`.

The output contains document pairs with similarity greater than 0.9,
where each line has the format
```
<DocumentID1><TAB><DocumentID2>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/lsh.py data/articles.txt
```

### 4. k-Means algorithm using Spark

We implement the k-Means algorithm on large datasets using Spark.

#### Input and output format

Each line in the input file has the following format:
```
<FEATURE 1><SPACE><FEATURE 2>...
```
We provide a sample dataset in `data/kmeans.txt`.

The output contains a single line with the average diameter of 
the created clusters:
```
<AverageDiameter>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
bin/spark-submit code/kmeans.py data/kmeans.txt k_value
```

### 5. Collaborative filtering

We implement item-based and user-based collaborative filtering.

#### Input and output format

Each line in the input file has the following format:
```
<USER ID>,<MOVIE ID>,<RATING>,<TIMESTAMP>
```
We provide a sample dataset in `data/ratings.txt`.

The output contains 10 lines, where the first 5 lines
and the last 5 lines
contain the top 5 recommendations for the user with ID 600
using item-based and user-based collaborative filtering, respectively:
```
<MOVIE ID><TAB><PREDICTED RATING>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/cf.py data/ratings.txt
```

### 6. PageRank algorithm using Spark

We implement the PageRank algorithm using Spark.

#### Input and output format

Each line in the input file has the following format:
```
<Source Node><TAB><Destination Node>
```
We provide a sample dataset in `data/graph.txt`.

The output contains 10 lines, corresponding to 
the top 10 nodes with the highest PageRank score:
```
<Node><TAB><PageRank Score>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
bin/spark-submit code/pagerank.py graph.txt
```

### 7. Efficient triangle counting

We implement an efficient algorithm for counting triangles in a 
large graph, where brute-force algorithms are not feasible.

#### Input and output format

Each line in the input file has the following format:
```
<Node1><TAB><Node2><TAB><TimeStamp>
```
We provide a sample dataset in `data/facebook.txt`.

The output contains a single line with the number of triangles:
```
<NumberOfTriangles>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/triangle.py data/facebook.txt
```

### 8. Gradient Descent SVM

We implement a gradient descent SVM classifier.

#### Input and output format

There is one file containing the features and one file containing the labels.
Each line in the features file has the following format:
```
<Feature 1>,<Feature 2>...
```
Each line in the labels file contains a single label that is either 1 or -1.
We provide a sample dataset in `data/svm-features.txt` and `data/svm-labels.txt`.

The output contains a single line with the 10-fold cross validation 
accuracy of the model:
```
<Accuracy>
```

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/svm.py data/svm-features.txt data/svm-labels.txt
```

### 9. DGIM algorithm

We implement the Datar-Gionis-Indyk-Motwani algorithm for estimating
the number of 1s in the last k elements of a binary stream.

#### Input and output format

Each line of the input contains either a 0 or a 1. 
We provide a sample dataset in `data/stream.txt`.

Each line of the output contains the estimated number of
1s in the last k elements, where k is a parameter of the algorithm
to be specified by the user (see example usage)

#### Example usage

In order to run the algorithm on our provided dataset, run the following command:

```
python code/dgim.py data/dgim.txt k1 k2 k3 ...
```
Each `ki` is a parameter of the algorithm.

## References

Anand Rajaraman, Jure Leskovec, and Jeffrey D. Ullman. *Mining of Massive Datasets*