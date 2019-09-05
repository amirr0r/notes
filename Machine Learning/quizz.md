# [Hands on Machine Learning and Deep Learning in Python using Scikit-Learn, Keras and TensorFlow](https://github.com/ageron/handson-ml2) *by Aurélien Géron* (2nd secondition)

## 01 - the machine learning landscape

### 01.  Comment définiriez-vous l'apprentissage automatique ?

L'*apprentissage automatique* (de l'anglais **Machine learning**) est une branche de l'**intelligence artificielle**.

Il s'agit d'un domaine d'études des algorithmes permettant aux ordinateurs d'**apprendre à faire des <u>prédictions</u>** sur des données (à partir d'exemples) sans les programmer explicitement.

> Dans une <u>approche traditionnelle, on aura tendance à écrire **des règles en durs**</u> tandis qu'un programme basé sur des techniques de Machine learning s'adaptera automatiquement aux données *(à condition d'avoir été correctement entraîné au préalable)*.

À noter qu'on parle d'**apprentissage** pour une machine dès lors que sa performance **P** pour une tâche **T** s'améliore avec l'expérience **E**. (Tom M. Mitchell, Machine learning book 1998)
+ Exemple: jeu d'échecs
    * T: jouer
    * E: le nombre de parties  
    * P: le nombre de parties remportées

### 02. Pouvez-vous nommer quatre types d'applications du Machine learning ?

Il existe de nombreux problèmes où le ML s'avère être très utile: 
  + 1. Classification d'images *(cf. **Mobilenet**)*
  + 2. Recommandations de produits *(**Amazon**, **Netflix**, **Youtube** par exemple)*
  + 3. Détection de fraudes financières
  + 4. Apprentissage des robots *(drônes autonomes ou encore dans les jeux vidéos &rarr; **AlphaGo**)*

> CF. https://www.journaldunet.com/solutions/reseau-social-d-entreprise/1191979-machine-learning-12-secteurs/

> **Corrigé:** L'apprentissage automatique est idéal pour les problèmes complexes pour lesquels nous n'avons pas de solution algorithmique, pour remplacer de longues listes de règles rédigées manuellement, pour construire des systèmes qui s'adaptent aux environnements fluctuants et pour aider enfin les humains à apprendre (par exemple, l'exploration de données)

### 03. Qu'est-ce qu'un ensemble d'entraînement étiqueté ?

Un ensemble d'entraînement étiqueté *(ou Dataset labellisé)* est un jeu de données pour lequel on a annoté par avance les prédictions que l'on souhaite obtenir sur chacun des échantillons.

### 04. Quelles sont les deux tâches supervisées les plus courantes ?

La **classification** et la **régression**.

### 05. Pouvez-vous nommer quatre tâches courantes non supervisées ?

Le **clustering** (regroupement), la **réduction de la dimension** la **détection d'anomalies et de nouveautés**. 

> Il y aussi l'apprentissage des règles d'association et la visualisation... (j'comprends pô trop)

### 06. Quel type d'algorithme d'apprentissage automatique utiliseriez-vous pour permettre à un robot de marcher sur différents terrains inconnus ?

L'<u>**apprentissage par renforcement**</u>: *Monte carlo, Policy gradient, Q-learning... etc.*

### 07. Quel type d'algorithme utiliseriez-vous pour segmenter vos clients en plusieurs groupes ?

Les algorithmes de **clustering**: *k-means, dbscan, ... etc* (apprentissage non supervisé) si je ne sais pas dans quel groupe je souhaite les segmenter.

Si au contraire je connais par avance les catégories dans lesquelles je souhaite les segmenter alors j'utiliserai probablement des algos de **classification supervisée**: *k-nn, decision trees, logistic regression, naive bayes...*.

### 08. Diriez-vous le problème de la détection du spam comme un problème d'apprentissage supervisé ou non supervisé ?

Naïvement, je dirais <u>les deux</u> !

Ma première intruition pencherait davantage pour l'**apprentissage supervisé** car il existe de nombreuses instances de spams et de non spams.

Seulement, dans le cas de campagne de phishing par exemple, les spams évoluent et peuvent ressembler de plus en plus aux mails "normaux". De ce fait, la détection de spams peut égalemet un problème d'apprentissage non supervisé (détection d'anomalies, clustering).

### 09. Qu'est-ce qu'un système d'apprentissage en ligne ?

Un système d'apprentissage en ligne *(**online learning**)* peut apprendre au fur et à mesure durant plusieurs sessions d'entraînements contrairement à un système d'apprentissage par lots *(**batch learning**)* qui ne peut être entraîné qu'une fois.

> Cf. [What is the Difference Between a Batch and an Epoch in a Neural Network ?](https://machinelearningmastery.com/difference-between-a-batch-and-an-epoch/)

### 10. Qu'est-ce que l'apprentissage non conforme ?

Ce qu'on appelle aussi les algorithmes **out-of-core** sont utilisés lorsque le Training set est trop volumineux pour la mémoire du système et que par conséquent plusieurs sessions d'apprentissage sont réalisées.

> Il s'agit donc obligatoirement d'un apprentissage en ligne avec des mini batches. 
> Toutefois, l'apprentissage en ligne peut être fait hors ligne (i.e. pas sur le système déployé)

### 11. Quel type d'algorithme d'apprentissage s'appuie sur une mesure de similarité pour faire des prédictions ?

*Si l'algorithme est <u>basé sur des instances</u>, il apprend simplement les exemples par cœur et se généralise aux nouvelles instances en les comparant aux instances apprises à l'aide d'une **mesure de similarité**."*

C'est donc les algorithmes basés sur des instances qui s'appuient sur une mesure de similarité.

> **TODO:** citer des algos.

### 12. Quelle est la différence entre un paramètre de modèle et l'hyperparamètre d'un algorithme d'apprentissage ?

La "magie" d'un algorithme d'apprentissage consiste à **ajuster les paramètres d'un modèle** de sorte à l'adapter aux données d'apprentissage et par extension aux données futures. 

Ceci implique que les paramètres évoluent et sont donc le résultat de l'apprentissage|entraînement. 

Un **hyperparamètre** d'un algorithme d'apprentissage est constant. Il définit comment se passe l'entraînement. 

> **TODO:** illustrer avec un exemple (θ, régularisation...)
> **TODO:** ajouter image GIF ajustement des paramètres régression linéaire

### 13. Que recherchent les algorithmes d'apprentissage basés sur des modèles ? Quelle est la stratégie la plus commune qu'ils utilisent pour réussir ? Comment font-ils des prédictions ?

*"Si l'algorithme est <u>basé sur un modèle</u>, il ajuste certains paramètres pour l'ajuster à l'ensemble d'apprentissage (c'est-à-dire, pour faire de bonnes prédictions sur l'entraînement lui-même), puis espère qu'il pourra également faire de bonnes prédictions sur les nouveaux cas.* 

**L'objectif des algos basés sur des modèles est de généraliser.**

### 14. Pouvez-vous nommer quatre des principaux défis *(principales difficultés)* de l'apprentissage automatique ?

1. Manque de données
2. Données non représentatives
3. Caractéristiques peu pertinentes
4. Sous-apprentissage et sur-apprentissage

### 15. Que se passe-t-il si votre modèle fonctionne bien sur les données d'apprentissage, mais se généralise mal aux nouvelles instances ? Pouvez-vous nommer trois solutions possibles ?

Il s'agit d'un problème de sur-apprentissage *(**overfitting**)*. Pour y remédier:

- Réduire le nombre de features
- Collecter plus de données d'entraînement
- Supprimer les valeurs aberrantes dans le training set
- Simplifier le modèle *(en choisir un avec moins de paramètres)*

### 16. Qu'est-ce qu'un ensemble de test et pourquoi voudriez-vous l'utiliser ?

Avant de déployer le modèle, il est important d'évaluer son taux d'erreur sur de nouvelles instances.

C'est la raison pour laquelle on utilise un **test set** pour vérifier ses résultats avant sa mise en production.

### 17. Quel est le but d'un jeu de validation ?

Le but d'un **validation set** est de comparer les modèles.

Il permet également d'ajuster les hyper-paramètres.

### 18. Qu'est-ce que le train-dev set, quand en avez-vous besoin et comment l'utilisez-vous ?

Ce qu'Andrew Ng appelle le "**train-dev set**" permet de vérifier la **cohérence des données** *(data mismatch)* et in fine la qualité d'apprentissage.

En effet, si le modèle réalise de bonnes prédictions sur le training set mais pas sur le training-dev set, on est dans un cas de sur-apprentissage.

Si le modèle réalise de bonnes prédictions sur le Training Set et sur le Training-dev Set mais pas sur le Validation Set, il y a clairement un problème de cohérence dans la séparation des données entre les différents sets. 

### 19. Qu'est-ce qui peut mal tourner si vous réglez les hyperparamètres à l'aide de l'ensemble de test ?

On risque de sur-entraîner le modèle sur le test set.
___

## 02 - end to end machine learning project
## 03 - classification
## 04 - training linear models
## 05 - support vector machines
## 06 - decision trees
## 07 - ensemble learning and random forests
## 08 - dimensionality reduction
## 09 - unsupervised learning
## 10 - neural nets with keras
## 11 - training deep neural networks
## 12 - custom models and training with tensorflow
## 13 - loading and preprocessing data
## 14 - deep computer vision with cnns
## 15 - processing sequences using rnns and cnns
## 16 - nlp with rnns and attention
## 17 - autoencoders and gans
## 18 - reinforcement learning
## 19 - training and deploying at scale