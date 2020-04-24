This repo contains all the project files for DataScience nano degree program of Udacity as per the requirement of final Capstone Project (StarBucks)

The accompanying medium blog post can be found [here](https://medium.com/@hardikbalar101/starbucks-customer-classification-using-catboost-c3026d1785d7).

### Table of Contents

1. [Installation](#installation)
3. [Project Organization](#org)
4. [Evaluation Strtegy & Results](#results)
5. [Licensing, Authors, and Acknowledgements](#licensing)

## Installation <a name="installation"></a>

Libraries used

scikit-learn==0.22.2.post1
nltk==3.4.5
numpy==1.18.2
pandas==1.0.3
joblib==0.13.2
progressbar==2.5
catboost==0.20.2
matplotlib==3.2.1
seaborn==0.10.0
scikit-plot==0.3.7
scipy==1.4.1
notebook==6.0.3
jupyterlab==2.1.0
jupyter==1.0.0
bokeh==2.0.1
cookiecutter==1.7.0
ipywidgets==7.5.1
nbconvert==5.6.1
setuptools==46.0.0

I have used Python version 3.7.7 in my local machine and prepared project notebook on safari web-browser.

## Project Organization <a name="org"></a>
------------

    ├── LICENSE
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── notebooks          <- Jupyter notebooks
    │   |__ project_notebook.ipynb <- final version of project notebook containing end to end approach
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │   |__ resources-citation.md <- collection of resources I find useful (not exhaustive)
    ├── reports            <- Generated analysis as HTML
    |__ project_notebook-v2.html <- HTML version of latest project notebook for quick viewing
-----------

## Evaluation Strategy & Results<a name="results"></a>

**Everything is included in the jupyter notebook provided in the notebooks folder**
**Alternatively, you can view html version of notebook in project root directory.** <br>

**Choice of Measurement Metrics for Multiclass classification** <br>

Our problem falls into the category of multi-class classification. The multi-class classification problem can be summarised as below:
<br>
Given
a dataset with instances 𝑥𝑖 together with 𝑁 classes where every instance 𝑥𝑖 belongs precisely to one class 𝑦𝑖
is a problem targeted for a multiclass classifier. <br>
After the training and testing, we have a table with the correct class 𝑦𝑖 and the predicted class 𝑎𝑖 for every instance 𝑥𝑖 in the test set. So for every instance, we have either a match (𝑦𝑖=𝑎𝑖) or a miss (𝑦𝑖≠𝑎𝑖).
<br>
Assuming we have balanced class distribution in our training set, evaluation using a confusion matrix together with the average accuracy score should be sufficient. However, F1-score can also be used for the evaluation of the multi-class problem. <br>

**Since the cost of misclassification is not high in our case (sending an offer to the non-responsive customer doesn't cost the company extra money), F1-score is not necessary. <br>
In this project, I prefer to use the confusion matrix and the average score as our evaluation measures.**
<br>
**Confusion Matrix:** <br>
A confusion matrix shows the combination of the actual and predicted classes. Each row of the matrix represents the instances in a predicted class, while each column represents the instances in an actual class. It is a good measure of whether models can account for the overlap in class properties and understand which classes are most easily confused.<br>
**Accuracy:** <br>
Percentage of total items classified correctly- (TP+TN)/(N+P)
 + TP: True Positive
 + TN: True Negative
 + N: Negative
 + P: Positive

For unbalanced class distribution, I have provided weights to each class label, and CatBoost automatically handles it.
<br>

**Evaluation Strategy:** <br>

**1. Initial Model Evaluation with fixed values of hyperparameters**

- I evaluate CatBoost Classifier with following fixed hyperparameters on all classification problems (class_2, class_3, class_4, class_5)

    - number of iterations = 2000
    - loss_function = 'MultiClass'
    - early_stopping_rounds = 50
    - eval_metric = 'Accuracy'

   and the rest of the parameter values as default provided by CatBoost. <br>

**2. Model Evaluation with finding best values of hyperparameters using GridSearch**

- In this round of experiments, I wrote a customer GridSearch function, which finds the best values for each given hyperparameter ranges and returns the model hyperparameters with the best average accuracy on training data. <br>

- For CatBoost Multiclassifier, there is numerous hyperparameter to tune. An extensive list can be found here: https://catboost.ai/docs/concepts/parameter-tuning.html <br>

- Here I selected only the following hyperparameters and specified the recommended range(found via some research on CatBoost website and Kaggle)for each of these parameters and find the model with the best score. <br>

    - iterations = [1000, 2000, 3000] no. of boosting iterations
    - loss_function = ['Logloss','MultiClass','MultiClassOneVsAll']
    - depth = [4,6,8] maximum depth of the tress
    - early_stopping_rounds = [10, 20, 50] parameter for fit() - stop the training if one metric of a validation data does not   improve in last early_stopping_rounds rounds. </br> </br>

- I perform training on the model using those identified parameters.Results are visualised as well.


## Licensing, Authors, Acknowledgements<a name="licensing"></a>

Must give credit to StackOverflow and Bokeh documentation for the all general information. You can find the Licensing for the data and other descriptive information at Udacity website.
View License terms of this project in LICENSE file.

Signed-off by @Author: Hardik B.
