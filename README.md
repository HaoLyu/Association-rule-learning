# Association-rule-learning
Introduce the Python implementation of Association rule learning, including Apriori algorithm and FP-growth algorithm

## Apriori algorithm
Apriori uses a breadth-first search strategy to count the support of itemsets and uses a candidate generation function which exploits the downward closure property of support.

### Run by
```
python Apriori.py input_file result_file sigma row_size(optional)
```

## FP-growth algorithm
FP stands for frequent pattern.This is a much more efficient and fast way to mine association-rule. Because this algorithm compresses the large dataset into a compact Frequent-Pattern tree. This way avoids repeated database scans and calculate on a compact tree struture. More information, pls see *[Wikipedia](https://en.wikipedia.org/wiki/Association_rule_learning#Apriori_algorith) 

### Run by
```
python FP_Tree.py input_file result_file sigma row_size(optional)
```

* Here I prepare a 20k transaction file 'input_file_example.da' as our example input. 
* Our output will be 'result_file.csv'.
* Sigma is our minimun support number, which means only the itemset appearing more than sigma times will we think it is frequent. 
* Row_size are the size of rows that will be throw into our model. You can skip it and the default value will scan all the rows in the input file.
* Here I define the minimum frequent itemset should contain at least three times.

### Author
Hao Lyu, UT Austin, School of Information, email: lyuhao@utexas.edu
