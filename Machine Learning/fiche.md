# Batch learning *(apprentissage par lot)*

The system is trained with all available data and just applies what it has learned &rarr; *offline|batch learning*

> **Disadvantage:** need to train a new version of the system from scracth on the full dataset

# Online learning *(apprentissage en ligne)*

The system can learn incrementally.

Data has to be given sequentially, either individually or by mini groups called *mini-batches*.

___

- Technique de [MapReduce](https://fr.wikipedia.org/wiki/MapReduce): diviser l'apprentissage sur plusieurs serveurs.