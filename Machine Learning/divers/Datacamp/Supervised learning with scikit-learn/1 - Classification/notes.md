ensuring your data adheres to the format required by the scikit-learn API.

The features need to be in an array where each column; is a feature and each row a different observation or data point.

## Metric for measuring model performance

In **classification**, <u>accuracy is a commonly used metric</u>.
> **accuracy:** fraction (number of correct predictions divided by the total number of data points) of correct predictions

__*Which data do we use to compute accuracy ?*__

What we really interested in is <u>how well our model will perform on new data</u> *(that is samples that the algorithm has never seen before)*.

*We could compute accuracy on data used to fit classifier, don't we ?* Yes however as this data was used to train it, they will be NOT indicative of ability to generalize.
> the classifier's performance will not be indicative of how well it can generalize to unseen data.

For this reason, it is common practice to split your data into two sets :
1. **training set** (~90%).
2. **test set** (~10%) *(we make predictions on the labeled test set and compare these predictions with the know labels)*.

### Moel complexity

#### Overfitting

Complex models run the risk of being sensitive to noise in the specific data that we have, rather than reflecting general trends in the data.

#### Underfiting

Simple models can perform less well on both test and training sets.
> Check **model complexity curve**.