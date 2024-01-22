
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
A **classifier is a function that takes feature values as input and outputs a label. <br>
A **test dataset is used to evaluate a classifier’s predictions. <br>
The **error **rate is the proportion of data points where the prediction is wrong <br>

### Types of Machine Learning Classifiers
**Majority **vote **classifier: always predicts the most common label in the **training dataset. <br>
**Memorizer: if a set of features exists in the training dataset, predict its corresponding label; otherwise, predict the majority vote <br>

### A typical Machine Learning Routine
· Step 1 – training<br>
    Input: a labeled training dataset <br>
    Output: a classifier<br>
· Step 2 – Testing<br>
    Inputs: a classifier, a test dataset<br>
    Output: predictions for each test data point<br>
· Step 3 – evaluation<br>
    Inputs: predictions from step 2, test dataset labels<br>
    Output: some measure of how good the predictions are;<br>
    usually (but not always) error rate<br>

## Machine Learning as Functional Approximation
### Notations
· Feature Space: x<br>
· Label Space: y<br>
· Unknown target function: c<sup>*</sup>: x -> y<br>
· Training Dataset: D = {x<sup>(1)</sup>, c<sup>*</sup>}
