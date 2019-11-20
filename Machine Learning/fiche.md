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

Scikit-learn API:

1. **Estimators**: has a `fit()` method.
2. **Transformers**: has a `transform()` method.
3. **Predictors**: has a `predict()` method.

> All **predictors** are **estimators** but the reverse(la réciproque) is not true.

**All the estimator's hyper-parameters are accesible directly via public instance variables !!!**

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
- `StratifiedKFold`

## Some useful functions:

- `cross_val_score`: returns the evaluation scores on each test fold.
- `cross_val_predict`: returns the predictions made on each test fold.
- `confusion_matrix`: returns true/false positives/negatives.

> **A perfect classifier would have only true positives and true negatives**, so its confusion matrix would have nonzero values only on its main diagonal (top left to bootom right).

___

**Some learning algorithms are sensitive to the order of the training instances, and they perform poorly if they get many similar instances in a row.**

___

The Scikit-Learn’s _Stochastic Gradient Descent Classifier_ (`SGDClassifier`) has the advantage of being capable of handling very large datasets efficiently. 

> This is in part because SGD deals with training instances independently, one at a time _(which also makes SGD well suited for online learning)_.

___

Scikit-Learn does not let you **set the threshold directly**, but it does give you access to the **decision scores** that it uses to make predictions.

**A high-precision classifier is not very useful if its recall is too low!**
___