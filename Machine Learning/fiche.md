# Batch learning *(apprentissage par lot)*

The system is trained with all available data and just applies what it has learned &rarr; *offline|batch learning*

> **Disadvantage:** need to train a new version of the system from scracth on the full dataset

# Online learning *(apprentissage en ligne)*

The system can learn incrementally.

Data has to be given sequentially, either individually or by mini groups called *mini-batches*.

> Not to be confused with **cross-validation**.
___

- Technique de [MapReduce](https://fr.wikipedia.org/wiki/MapReduce): diviser l'apprentissage sur plusieurs serveurs.
___

# Scikit-learn API:

1. **Estimators**: has a `fit()` method.
2. **Transformers**: has a `transform()` method.
3. **Predictors**: has a `predict()` method.

> All **predictors** are **estimators** but the reverse(la réciproque) is not true.

**All the estimator's hyper-parameters are accesible directly via public instance variables !!!**

**Some learning algorithms are sensitive to the order of the training instances, and they perform poorly if they get many similar instances in a row.**

## Some useful classes:

- `OneHotEncoder` & `OrdinalEncoder`: for handling text and categorical attributes.
  
> `OrdinalEncoder` will assume that two nearby values are more similar thant two distant values

- `SimpleImputer`: take care of missing values.
- `TransformerMixin` & `BaseEstimator`: for creating a **Transformer** class.
- `Pipeline`: merge Data Tranformers + model
- `ColumnTransformer`: applying the appropriate transformations to each column.
- `GridSearchCV`: (brute-force) finding the bests hyper-parameters _(and so the best(s) estimator(s))_.

> If GridSearchCV is initialized with `refit=True` (which is the default), then once it finds the best estimator using cross-validation, **it retrains it on the whole training set**.

- `RandomizedSearchCV`: instead of trying out all possible combinations, it evaluates a given number of random combinations by selecting a random value for each hyper-parameter at every iteration.
- `StandardScaler` : used for standardization _(feature scaling)_.
- `StratifiedKFold` : provides train/test indices to split data in train/test sets.


## Some useful functions:

- `cross_val_score`: returns the evaluation scores on each test fold.
- `cross_val_predict`: returns the predictions made on each test fold.
- `confusion_matrix`: returns true/false positives/negatives.

> **A perfect classifier would have only true positives and true negatives**, so its confusion matrix would have nonzero values only on its main diagonal (top left to bootom right).


## Scikit-Learn's Datasets dictionary structure

- **DESCR**: dataset description.
- **data**: array with one row per instance and one column per feature.
- **target**: array with the labels.

>  **skewed datasets**: some classes are much more frequent than others.

## Some notes and tips

- The Scikit-Learn’s _Stochastic Gradient Descent Classifier_ (`SGDClassifier`) has the advantage of being capable of handling very large datasets efficiently. 

> This is in part because SGD deals with training instances independently, one at a time _(which also makes SGD well suited for online learning)_.

- Scikit-Learn does not let you **set the threshold directly**, but it does give you access to the **decision scores** that it uses to make predictions.

- When the positive class is rare, prefer the PR curve.
- When we care more about false positives than false negatives, prefer the PR curve also.  

> the ROC curve otherwise.

- **Scikit-Learn detects when you try to use a binary classification algorithm for a multiclass classification task, and it automatically runs OvA or OvR.**
___

# Classification

**A high-precision classifier is not very useful if its recall is too low!**

> **Note**: increasing precision reduces recall, and vice versa.

A perfect classifier will have a **ROC AUC equal to 1**, whereas a purely random classifier will have a ROC AUC equal to 0.5. 

> **ROC** curve: receiver operating characteristic.

> **AUC**: area under the curve.

## Binary classification

Detecting positive class and negative class.

Examples:
- Spam and non-spam.
- Cat or not ?
- 5 or non-5 ?

## Multiclass classification

Some algorithms can handle multiple classes natively (e.g, NAive Baye, SGD, Random Forests...).

But, there are two main strategies used to **perform multiclass classification using multiple binary classifiers**:

- One versus All
- One versus One

> If you want to force Scikit-Learn to use one-versus-one or one-versus-all, you can use the `OneVsOneClassifier` or `OneVsRestClassifier`

### One-versus-All (OvA) == One-versus-the-rest

1. Train N binary classifiers for each class.
2. For a given instance, get the decision score from each classifier. 
3. Then, select the class whose classifier outputs the highest score.

### **One-versus-One (OvO)**:

Train a binary classifier for every pair of class.
   
> If there are **N** classes, you need to train **N × (N – 1) / 2** classifiers.

## Multilabel classification

outputs multiple binary tags.

> **TODO:** give 2 examples

## Multioutput classification

Multiclass classification where each label can be multiclass.
___