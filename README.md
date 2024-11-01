java c
Introduction to ML - Decision Tree Coursework 
COMP70050 
October 2023
Overview In this assignment, you will implement a decision tree algorithm and use it to determine one of the indoor locations based on WIFI signal strengths collected from a mobile phone.  See Figure 1 for an illustration of the experimental scenario. The results of your experiments should be discussed in the report. You should also deliver the code you have written.
Setup Please see the guidelines provided on Scientia to set up your machines for the coursework.   It is your own responsibility to ensure that your code runs on the DoC lab machines.  We reserve the right to reduce your marks by 30% for any bits of your code that cannot be run.
Coursework 
Step 1: Loading data 
You can load the datasets from the files ”WIFI db/clean dataset.txt” and
”WIFI db/noisy dataset.txt” .  They contain a 2000x8 array.  This array represents a dataset of 2000 samples. Each sample is composed of 7 WIFI signal strength while the last column indicates the room number in which the user is standing (i.e., the label of the sample). All the features in the dataset are continuous except the room number. You can load the text file with the ”loadtxt” function from Numpy.  Given the nature of the dataset, you will have to build decision trees capable of dealing with continuous attributes and multiple labels.
For the report: 
There is nothing to add in the report for this section. 
Step 2:  Creating Decision Trees To create the decision tree, you will write a recursive function called  decision tree   learning(), that takes as arguments a matrix containing the dataset and a depth variable (which is used to compute the maximal depth of the tree, for plotting purposes for instance).  The label of the training dataset is assumed to be the last column of the matrix.  The pseudo-code of this function is described below.  This pseudo-code is taken from Artificial Intelligence:  A  Modern Approach  by Stuart Russell and Peter Norvig (Figure 18.5), but modified to take into account that the considered dataset contains continuous attributes (see section 18.3.6 of the book).The function FIND SPLIT chooses the attribute and the value that results in the highest information gain. Because the dataset has continuous attributes, the decision-tree learning algorithms search for the split point (defined by an attribute and a value) that gives the highest information gain.  For instance, if you have two attributes (A0 and A1) with values that both range from 0 to 10, the algorithm might determine that splitting the dataset according to ”A1>4” gives the most information. An efficient method for finding good split points is to sort the values of the attribute, and then consider only split points that are between two examples in sorted order, while keeping track of the running totals of examples of each class for each side of the split point.To evaluate the information gain, suppose that the training dataset Sall  has K different labels. We can define two subsets (Sleft  and Sright ) of the dataset depending on the splitting rule (for instance ”A1>4”) and for each dataset and subset, we can compute the distribution (or probability) of each label. For instance, {p1p2 ...pK }Figure 1:  Illustration  of the scenario.  The WIFI signal strength from 7 emitters is recorded from a mobile phone.  The objective of this coursework is to learn a decision tree that predicts in which of the 4 rooms the user is standing.
Algorithm 1 Decision Tree creating 
1: procedure decision tree learning(training dataset, depth) 
2: if all samples have the same label then 
3: 
return (a leaf node with this value, depth) 
4: 
else 
5: 
split← find split(training dataset) 
6: 
node← a new decision tree with root as split value 
7: 
l branch, l depth ← DECISION TREE LEARNING (l dataset, depth+1) 
8: 
r branch, r depth ← DECISION TREE LEARNING (r dataset, depth+1) 
9: 
return (node, max(l depth, r depth)) 
10: 
end if 
11: 
end procedure 

Figure 2: example of Decision tree visualization (not all the tree is displayed, and this tree has been trained on a different dataset.)
(pk  is the number of samples with the label k divided by the total number of samples from the initial dataset). The information gain is defined by using the general definition of the entropy as follow:

Where |S| represents the number of samples in subset S.
You are only allowed to use Numpy, Matplotlib, and the standard Python libraries 
(https://docs.python.org/3/library/index.html) for this coursework. Any other library, like scikit learn is NOT allowed. For the implementation of the tree  (in Python), it is advised to use dictionaries to store nodes as a single object, for instance by using this kind of structure {’attribute’, ’value’, ’left’, ’right’}. In this case, ’left’ and ’right’ will also be nodes. You might also want to add a Boolean field name ”leaf” that indicates whether or not the node is a leaf (terminal node) or not.  However, this is not strictly necessary, as there are other methods to determine if a node 代 写COMP70050 Introduction to ML - Decision Tree CourseworkPython
代做程序编程语言is terminal or not.
For the report: 
There is nothing to add in the report for this section. 
Bonus part (5 points): Write a function to visualize the tree. See Figure 2 for an example. You can only use Numpy, Matplotlib, and the standard Python libraries for this question as well. Show in your report the output of this function with a tree trained on the entire clean dataset. 
Step 3 - Evaluation Now that you have an automatic process to train decision trees, you can evaluate the accuracy of your tree on the provided datasets.  For that, evaluate your decision tree using a 10-fold cross-validation on both the clean and noisy datasets. You should expect that slightly different trees will be created with each fold since the training data that you use each time will be slightly different. Use your resulting decision trees to classify your data in your test sets.
Implementation: 
Implement an evaluation function that takes a trained tree and a test dataset:  evaluate(test   db, trained tree) and that returns the accuracy of the tree.
For the report: 
Cross validation classification metrics By computing the average over all the test folds, report the fol- lowing cross-validation classification metrics for both clean and noisy data:
• Confusion matrix.  (Hint: you should get a single 4x4 matrix)
•  The accuracy (Hint: you can derive the metrics directly from the previously computed confusion matrix).
•  The recall and precision rates per class.
• The F1-measures derived from the recall and precision rates of the previous step.
Result analysis Comment for both datasets on which rooms are correctly recognized, and which rooms are confused with others. 5 lines max. 
Dataset differences: Is there any difference in the performance when using the clean and noisy datasets?  If yes/no explain why. 5 lines max. 
Step 4 - Pruning (and evaluation again) In order to reduce the performance difference of our decision tree between the clean and noisy dataset, you will implement a pruning function based on lowering the validation error. This approach works as follows: for each node directly connected to two leaves, evaluate the benefits of the validation error of substituting this node with a single leaf (defined according to the training set).  If a single leaf reduces or does not change the validation error, then the node is pruned and replaced by a single leaf.  The tree needs to be parsed several times until there is no more node connected to two leaves such that combining them into one leaf will improve the model’s performance (HINT: when you prune a node, the parent node might now verify this condition).
For the report: Cross validation classification metrics after pruning Report the performances of your trees after pruning by using a nested 10-fold cross-validation (”option 2”) to compute the metrics defined in the previous section for both datasets.
Result analysis after pruning Comment on the difference in performance before and after pruning for both datasets. Briefly explain these performance differences. 5 lines max. Depth analysis Comment on the average depth of the trees you generated for both datasets, before and after pruning.  What can you tell about the relationship between maximal depth and prediction accuracy? 5  lines max. 
Deliverables 
For the completion of this part of the coursework, the following has to be submitted electronically via CATE:
• All the code you have written
• A README file (in .txt, .md, or .pdf) to explain how to run your code on the lab machines.
• A short report with the following structure:
– (Bonus points: Output of the tree visualisation function.)
– Step 3 - Evaluation
*  Cross validation classification metrics.
*  Result analysis (5 lines max).
*  Dataset differences (5 lines max).
– Step 4 - Pruning (and evaluation again)
*  Cross validation classification metrics after pruning.
*  Result analysis after pruning (5 lines max).
*  Depth analysis (5 lines max).
Grading scheme 
Final Grade = Report content + Code quality + Report quality 
• Code (total : 20)
– Results on secret test dataset: 10 
Make sure that your code runs. If not, you will lose most of the code mark
– Presentation of the code (comments, indentation, structure): 5 
– README instructions: 5 
• Report content (total : 70)
– Output of the tree visualisation function: BONUS 5 
– Step 3 - Evaluation
*  Cross validation classification metrics (total 15) ·  Confusion matrix: 10 
·  The accuracy: 1 
·  The recall and precision per class: 2 ·  The F1-measures: 2 
*  Result analysis (5 lines max): 10 
*  Dataset differences (5 lines max): 10 
– Step 4 - Pruning (and evaluation again)
*  Cross validation classification metrics after pruning (total 15) ·  Confusion matrix: 10 
·  The accuracy: 1 
·  The recall and precision per class: 2 ·  The F1-measures: 2 
*  Result analysis after pruning (5 lines max): 10 
*  Depth analysis (5 lines max): 10 
• Report quality (total :  10)
– Quality of presentation: 10 





         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
