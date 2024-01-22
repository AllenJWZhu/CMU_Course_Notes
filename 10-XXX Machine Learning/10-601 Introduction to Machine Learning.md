
## Supervised and Unsupervised learning
### Supervised models:<br>
· Decision Trees<br>
· KNN<br>
· Naïve Bayes<br>
· Perceptron<br>
· Logistic Regression<br>
· Linear Regression<br>
· Neural Networks<br>

### Definition of Machine Learning
Three components: Task T, Performance metric P, and Experience E.

### Definitions: Machine Learning Classifier
A <ins>**classifier**</ins> is a function that takes feature values as input and outputs a label. <br>
A <ins>**test dataset**</ins> is used to evaluate a classifier’s predictions. <br>
The <ins>**error rate**</ins> is the proportion of data points where the prediction is wrong <br>

### Types of Machine Learning Classifiers
<ins>**Majority vote classifier**</ins>: always predicts the most common label in the <ins>**training**</ins> dataset. <br>
<ins>**Memorizer**</ins>: if a set of features exists in the training dataset, predict its corresponding label; otherwise, predict the majority vote <br>
<ins>**Decision Stump**</ins>: based on a single feature, x<sub>d</sub>, predict the most common label in the <ins>**training**</ins> dataset among all data points that have the same value for x<sub>d</sub><br>

### A typical Machine Learning Routine
· Step 1 – <ins>**Training**</ins><br>
    Input: a labeled training dataset <br>
    Output: a classifier<br>
· Step 2 – <ins>**Testing**</ins><br>
    Inputs: a classifier, a test dataset<br>
    Output: predictions for each test data point<br>
· Step 3 – <ins>**Evaluation**</ins><br>
    Inputs: predictions from step 2, test dataset labels<br>
    Output: some measure of how good the predictions are;<br>
    usually (but not always) error rate<br>

## Machine Learning as Functional Approximation
### Notations
· <ins>**Feature Space**</ins>: x<br>
· <ins>**Label Space**</ins>: y<br>
· <ins>**Unknown target function**</ins>: c<sup>*</sup>: x -> y<br>
· <ins>**Training Dataset**</ins>: D = {x<sup>(1)</sup>, c<sup>\*</sup>(x<sup>(1)</sup>) = y<sup>(1)</sup>, (x<sup>(2)</sup>, y<sup>(2)</sup>)...,(x<sup>(N)</sup>, y<sup>(N)</sup>)}<br>
· <ins>**Hypothesis Space**</ins>: H<br>
· <ins>**Goal**</ins>: Find the classifier, h ∈ H, that best approximates c<sup>\*</sup><br>

## Loss function and error Rates
<ins>**Loss Function**</ins>: l : y x y -> ℝ<br>
This defines how "bad" predictions $\hat{y}$ = h(x), are compared to the true labels, y = c<sup>\*</sup>(x)<br>

### Common choices for loss functions:
1. Squared loss (for regression): l(y, $\hat{y}$ ) = (y - $\hat{y}$ )<sup>2</sup><br>
2. Binary or 0-1 loss (for classification): l(y, $\hat{y}$ ) = 1(y != $\hat{y}$ ) <br>

### Decision Stump Pseudocode:
def train(D<sub>train</sub>):<br>
    pick a feature to split on, x<sub>d</sub><br>
    # split D<sub>train</sub> according to x<sub>d</sub><br>
    for v in V(x<sub>d</sub>), all possible values for x<sub>d</sub>:<br>
        D<sub>v</sub> = {(x<sup>n</sup>, y<sup>n</sup>) ∈ D<sub>train</sub>(x<sup>n</sup><sub>d</sub> = v)}<br>
    # compute a majority vote for each split<br>
    for v in V(x<sub>d</sub>):<br>
        store $\hat{y}$ <sub>v</sub> = mode(labels in D<sub>v</sub>)<br>
def h(x<sup>'</sup>):<br>
    for v in V(x<sub>d</sub>):<br>
        if x<sup>'</sup><sub>d</sub> = v: return $\hat{y}$ <sub>v</sub><br>
