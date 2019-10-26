# Project checklist

## Main steps

- [ ] 1. Définir l'objectif principal et les contraintes.
- [ ] 2. Récolte de données.
- [ ] 3. Étudier les données pour mieux les comprendre.
- [ ] 4. Préparer les données pour les transmettre aux algorithmes de Machine learning.
- [ ] 5. Sélectionner différents modèles prometteurs.
- [ ] 6. Tester les modèles et les combiner en une solution optimale.
- [ ] 7. Présenter la solution.
- [ ] 8. Déployer, surveiller, et maintenir la solution.

___

### Étape 1: définir l'objectif principal et les contraintes

1. Définition de l'objectif en une phrase.
2. Comment la solution sera utilisée ?
3. Quelles sont les solutions existantes _(s'il y en a)_ ?
4. De quel type de problématique s'agit-il _(apprentissage supervisé ou non, online/offline, etc.)_ ?
5. Quelles performances doivent-être mesurées ? Comment ? 
6. Les performances mesurées ont-elles un lien avec l'objectif principal ?
7. Quelles performances souhaitent-on atteindre _(en terme de vitesse de prédiction, de consommation CPU/RAM, de vrais/faux positifs/négatifs)_ ? 
8. Existe-t-il des problèmes similaires ?
9. L'expertise humaine est-elle disponible ?
10. Comment résoudre le problème manuellement ?
~~11. Énumérez les hypothèses que vous (ou d'autres) avez faites jusqu'à présent.~~
~~12. Vérifier les hypothèses si possible.~~

___

### Étape 2: récolter des données

> **Note**: automatisez autant que possible afin d'obtenir facilement de nouvelles données.

1. Définir le type de données dont on a besoin et combien.
2. Trouver ou construire les données et documenter comment obtenir ces données.
3. Noter la quantité d'espace disque nécessaire.
4. Vérifier les obligations légaleset et obtenez une autorisation si nécessaire.
5. Créer un espace de travail (avec suffisamment d'espace de stockage).
6. Convertir les données en un format facilement manipulate (sans changer les données en elle-même).
7. S'assurer que les informations sensibles sont protégées ou supprimées _(ou anonymisées)_.
8. Construire un test set.

___

### Étape 3: étudier les données pour mieux les comprendre.

> **Note**: essayer d'obtenir l'aide d'un expert pour ces étapes.

1. Faire une copie des données à des fins d'exploration.
2. Créer un Jupyter notebook pour conserver une trace de l'exploration.
3. Étudier chaque attributs et ses caractéristiques:
   - Nom
   - Type (texte, categorical variable, int/float, borné/illimité, etc.)
   ~~- Pourcentage des valeurs manquantes~~
   - Caractère bruyant et le type de bruit (stochastic, outliers, rounding errors, etc.)
   - Utile pour la tâche ?
   - Type de distribution (Gaussian, uniform, logarithmic, etc.)
4. Pour les algorithmes supervisés, identifier le(s) label(s) cible(s).
~~5. Visualiser les données.~~
6. Étudier la corrélations entre les attributs.
7. Réfléchir à une manière de résoudre le problème manuellement.
8. Identifier les éventuelles transformations que vous souhaitez appliquer.
9. Identifier/Énumérer les données supplémentaires qui pourraient être utiles (retournez à « step 1: récupérer les données» ).
10. Documenter les résultats de l'exploration.

___

### Étape 4: préparer les données

**Note**: penser à travailler sur une copie du Dataset et à énumérer les transformations réalisées de sorte à ne refaire plusieurs fois le même travail.

1. Nettoyer les données:
   - Supprimer les valeurs aberrantes (optionnel).
   - Ajouter les valeurs manquantes.
2. Choix des features:
   - Supprimer les attributs qui ne fournissent aucune information utile pour la tâche (optionnel).
3. Feature engineering:
   - Discrétiser les features à valeurs continues. [sklearn discretization](https://scikit-learn.org/stable/modules/preprocessing.html#discretization)
   - Features decomposition (e.g., categorical, date/time, etc.). [sklearn.decomposition](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.decomposition) ?
   - Énumérer les transformations souhaitées _(log(x), sqrt(x), x^2, etc.)_.
   - Transformer les features.
4. Feature scaling _(mise à l'échelle)_: 
   - standardize or normalize features ? O_o ?

___

### Étape 5: lister les modèles prometteurs

> **Note**: si les données sont volumineuses, vous pouvez échantillonner des mini-batches afin d’entraîner de nombreux modèles dans un délai raisonnable (sachez que cela pénalise les modèles complexes tels que les grands réseaux de neurones ou les random forests).
1. Entraînez de nombreux modèles de différentes catégories _(linéaire, naive bayes, SVM, random forests, réseaux de neurones, etc.)_ avec leurs paramètres standards.
2. Mesurer et comparer leurs performances.

> Pour chaque modèle, utilisez la N-fold cross-validation et calculez la moyenne et l'écart type de la mesure de performance sur les N folds.

3. Analysez les variables les plus significatives pour chaque algorithme.
4. Analysez les types d’erreurs commises par les modèles.

> Quelles données aurait utilisé un humain pour éviter ces erreurs?

5. Sauvegarder les résultats
6. Jeter un rapide coup d'oeil aux features selection et features engineering.
7. Reprendre les cinq premières itérations et comparer les résultats.
8. Faites une courte liste des modèles les plus prometteurs _(prioriser les modèles qui commettent différents types d’erreurs)_.

___


### Étape 6: tester les modèles et les combiner en une solution optimale

1. Ajustez les hyperparamètres en utilisant la cross validation.
2. Essayer les méthodes ensemblistes.  La combinaison de vos meilleurs modèles donnera souvent de meilleurs résultats que leur exécution individuelle.
3. Mesurer les performances sur le test set pour estimer l'erreur de généralisation.

> **WARNING**: Ne pas régler les hyper-paramètres après avoir mesuré l'erreur de généralisation, autrement le modèle sera sur-entraîné sur le test set.

___

### Étape 7: présenter la solution

1. Documenter votre travail.
2. Prépaper de beaux slides.
3. Expliquer en quoi votre solution répond au besoin.
4. Penser à évoquer les points intéressants you noticed along the way.
   - Décrire ce qui a fonctionné et ce qui n'a pas fonctionné.
   - Lister les limitations de votre modèle et les perspectives d'amélioration.
5. Assurez-vous que vos résultats clés sont communiqués au moyen de superbes visualisations ou d'énoncés faciles à retenir (<u>exemple</u>: _"the median income is the number-one predictor of housing prices"_).

___

### Étape 8: Déployer!

1. Préparer la solution pour la production (plug into production data inputs, write unit tests, etc.).
2. Préparer des scripts pour surveiller les performances du modèles.
3. Si nécessaire, ré-entraîner le(s) modèle(s) régulièrement sur des données récentes _(automatiser autant que possible)_.
