## Qu'est-ce que le feature scaling ?

> À quelques exceptions près, les algos de ML ne fonctionnent pas bien lorsque les attributs numériques en entrée ont des échelles très différentes.

* La **mise à l'échelle** des attributs. Il existe deux techniques courantes pour obtenir la même échelle pour tous les attributs:
  + l'échelle **_min-max_** &rarr; voir `MinMaxScaler` de Scikit-learn

    ![`MinMaxScaler`](images/MinMaxScaler.png)

  + la **_normalisation Z-score_** (_standardization._ en anglais) &rarr; voir `StandardScaler` de Scikit-learn

    ![Standardization Z-score](images/Standardization_Z-score.png)
___

## Quels sont les différents types de distribution _(Gaussian, uniform, logarithmic, etc.)_ ?

___

## Quels sont les différents types de bruit _(stochastic, outliers, rounding errors, etc.)_ ? 

___

## Quels sont les différents types d'erreurs ?

* Pour les problèmes de régresions, il existe plusieurs mesures de performance. Parmi lesquelles:
  + l'erreur **RMSE** (**R**oot **M**ean **S**quared **E**rror) (&rarr; distance euclidienne):
    - _m_ est le nombre d'instances.
    - **x**^_i_ est la _i_-ème instance &rarr; vecteur de caractéristiques.  
    - **y**^_i_ est l'étiquette de la _i_-ème instance.
    - **h** est la fonction de prédiction, _aussi appelée l'**hypothèse**_. Il s'agit tout simplement du modèle utilisé. Le résultat de cette fonction est souvent noté **ŷ**.
  
    ![RMSE formula](images/RMSE.png)
  
  + l'erreur **MAE** (**M**ean **A**bsolute **E**rror) (&rarr; distance de manhattan):
  
    ![MAE formula](images/MAE.png)

___

## Qu'est-ce que le stratified sampling ?

___

## Qu'est-ce que la descente de gradient ? 

## Qu'est ce que la _backpropagation_ ? 

## Qu'est-ce qu'_epoch_ signifie ?

## Qu'est-ce qu'une fonction d'activation ?
___

(Doubt) All machine learning algorithms can be train using the online/offline way ? I think that yes because it's oly a way to train and deploy a model.