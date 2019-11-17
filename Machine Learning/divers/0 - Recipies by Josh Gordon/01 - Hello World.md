
# <u>Hello World</u>

**Machine Learning** is a subfield of <u>artificial intelligence</u>.

It is the __study of algorithms__ that can <u>learn from examples and experience __instead of relying on hard-coded rules__</u>.

> These algorithms figure out the rules for us, so we don't write them by hand.

## Supervised Learning

Create a <u>classifier</u> by finding patterns in examples by following standards steps:
1. collect training data
2. train classifier
3. make predictions

Supervised algorithms have to take the <u>characteristics</u> / <u>metadatas</u> of our datas.

In Machine Learning <u>these measurements</u> are called __features__.

A good feature makes it <u>easy to discriminate</u>.

### Classifier

It takes some data as input and assigns a label to it as output.

__Examples:__ 
- a picture as input &rarr; classify it as cat or not cat.
- an email as input &rarr; classify it as spam or not spam.

#### 1. Collect training datas


```python
from sklearn import tree


features = [[140, "smooth"], [130, "smooth"], [150, "bumpy"],[170, "bumpy"]]
labels = ["apple", "apple", "orange", "orange"]

```

To keep things simple, we'll think of a classifier as a box of rules.

In `sklearn`, `.fit(features, labels)` for *find patterns in data* is the <u>classifier object</u>.

#### 2. Train classifier


```python
features = [[140, 1], [130, 1], [150, 0],[170, 0]]
labels = [0, 0, 1, 1]

clf = tree.DecisionTreeClassifier() 
clf = clf.fit(features, labels)
```

#### 3. Make predictions


```python
print clf.predict([[150, 0]]) # 1 for orange, 0 for apple
```

    [1]


We can create a new classifier for a new problem just by changing the training data.

That makes <u>__this approach far more reusable__</u> than *writing new rules for each problem*.

## Conclusion

The neat thing is that programming with Machine Learning isn't hard.

But to get it right, you need to understand a few important concepts like :

- *how much training data do you need ?*
- *how is the tree created ?*
- *What makes a good feature ?*

... and so on.
