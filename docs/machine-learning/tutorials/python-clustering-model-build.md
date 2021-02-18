---
title: 'Tutoriel Python : Créer un modèle de cluster'
titleSuffix: SQL machine learning
description: Dans la troisième partie de cette série de tutoriels qui en compte quatre, vous allez créer un modèle K-moyennes pour effectuer le clustering dans Python avec l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a397e23c9e2bc4e297efe7d85a1822a3056daf0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349073"
---
# <a name="python-tutorial-build-a-model-to-categorize-customers-with-sql-machine-learning"></a>Tutoriel Python : Créer un modèle pour classer les clients par catégorie avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans la troisième partie de cette série de tutoriels qui en compte quatre, vous allez créer un modèle K-moyennes dans Python pour effectuer le clustering. Dans la prochaine partie de cette série, vous déploierez ce modèle dans une base de données avec SQL Server Machine Learning Services ou sur des clusters Big Data.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans la troisième partie de cette série de tutoriels qui en compte quatre, vous allez créer un modèle K-moyennes dans Python pour effectuer le clustering. Dans la prochaine partie de cette série, vous déploierez ce modèle dans une base de données avec SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans la troisième partie de cette série de tutoriels qui en compte quatre, vous allez créer un modèle K-moyennes dans Python pour effectuer le clustering. Dans la partie suivante de cette série, vous déploierez ce modèle dans une base de données avec Azure SQL Managed Instance Machine Learning Services.
::: moniker-end

Dans cet article, vous allez apprendre à :

> [!div class="checklist"]
> * Définir le nombre de clusters pour un algorithme K-moyennes
> * Effectuer le clustering
> * Analyser les résultats

Dans la [première partie](python-clustering-model.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](python-clustering-model-prepare-data.md), vous avez préparé les données d’une base de données pour effectuer le clustering.

Dans la [quatrième partie](python-clustering-model-deploy.md), vous allez créer une procédure stockée dans une base de données capable d’effectuer un clustering en Python sur la base des nouvelles données.

## <a name="prerequisites"></a>Prérequis

* La troisième partie de ce tutoriel part du principe que vous avez satisfait les prérequis de la [**première partie**](python-clustering-model.md) et effectué les étapes décrites dans la [**deuxième partie**](python-clustering-model-prepare-data.md).

## <a name="define-the-number-of-clusters"></a>Définir le nombre de clusters

Pour regrouper vos données client en cluster, vous allez utiliser l’algorithme de clustering **K-moyennes**, l’un des moyens les plus simples et les plus connus de regrouper des données.
Pour plus d’informations sur K-moyennes, consultez [A complete guide to K-means clustering algorithm](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

L’algorithme accepte deux entrées : les données elles-mêmes et un nombre prédéfini « *k* » représentant le nombre de clusters à générer.
En sortie, vous obtenez *k* clusters et les données d’entrée sont partitionnées entre les clusters.

L’objectif de K-moyennes est de regrouper les éléments en k clusters de telle sorte que tous les éléments d’un même cluster présentent une grande similarité entre eux et une grande différence avec les éléments des autres clusters.

Pour déterminer le nombre de clusters que doit utiliser l’algorithme, utilisez un tracé de la somme des carrés intra groupe (« within groups sum of squares ») par rapport au nombre de clusters extraits. Le nombre approprié de clusters à utiliser se trouve au niveau de la courbe ou du « coude » du tracé.

```python
################################################################################################
## Determine number of clusters using the Elbow method
################################################################################################

cdata = customer_data
K = range(1, 20)
KM = (sk_cluster.KMeans(n_clusters=k).fit(cdata) for k in K)
centroids = (k.cluster_centers_ for k in KM)

D_k = (sci_distance.cdist(cdata, cent, 'euclidean') for cent in centroids)
dist = (np.min(D, axis=1) for D in D_k)
avgWithinSS = [sum(d) / cdata.shape[0] for d in dist]
plt.plot(K, avgWithinSS, 'b*-')
plt.grid(True)
plt.xlabel('Number of clusters')
plt.ylabel('Average within-cluster sum of squares')
plt.title('Elbow for KMeans clustering')
plt.show()
```

![Graphique coudé](./media/python-tutorial-elbow-graph.png)

D’après le graphique, il semble que *k = 4* serait une bonne valeur à tester. Cette valeur *k* regroupe les clients dans quatre clusters.

## <a name="perform-clustering"></a>Effectuer le clustering

Dans le script Python ci-dessous, vous allez utiliser la fonction KMeans du package sklearn.

```python
################################################################################################
## Perform clustering using Kmeans
################################################################################################

# It looks like k=4 is a good number to use based on the elbow graph.
n_clusters = 4

means_cluster = sk_cluster.KMeans(n_clusters=n_clusters, random_state=111)
columns = ["orderRatio", "itemsRatio", "monetaryRatio", "frequency"]
est = means_cluster.fit(customer_data[columns])
clusters = est.labels_
customer_data['cluster'] = clusters

# Print some data about the clusters:

# For each cluster, count the members.
for c in range(n_clusters):
    cluster_members=customer_data[customer_data['cluster'] == c][:]
    print('Cluster{}(n={}):'.format(c, len(cluster_members)))
    print('-'* 17)
print(customer_data.groupby(['cluster']).mean())
```

## <a name="analyze-the-results"></a>Analyser les résultats

Maintenant que vous avez effectué le clustering à l’aide de K-moyennes, la prochaine étape consiste à analyser le résultat et à voir si vous pouvez trouver des informations exploitables.

Examinez les valeurs moyennes du clustering et les tailles de cluster imprimées à partir du script précédent.

```results
Cluster0(n=31675):
-------------------
Cluster1(n=4989):
-------------------
Cluster2(n=1):
-------------------
Cluster3(n=671):
-------------------

         customer  orderRatio  itemsRatio  monetaryRatio  frequency
cluster
0        50854.809882    0.000000    0.000000       0.000000   0.000000
1        51332.535779    0.721604    0.453365       0.307721   1.097815
2        57044.000000    1.000000    2.000000     108.719154   1.000000
3        48516.023845    0.136277    0.078346       0.044497   4.271237
```

Les moyennes des quatre clusters sont fournies à partir des variables définies dans la [première partie](python-clustering-model-prepare-data.md#separate-customers) :

* *orderRatio* = retourne le taux de commandes (nombre total de commandes partiellement ou entièrement retournées par rapport au nombre total de commandes)
* *itemsRatio* = retourne le taux d’éléments (nombre total d’éléments retournés par rapport au nombre d’éléments achetés)
* *monetaryRatio* = retourne le taux des montants (montant monétaire total des éléments retournés par rapport au montant acheté)
* *frequency* = fréquence de retour

L’exploration de données avec K-moyennes demande souvent une analyse plus approfondie des résultats ainsi que des étapes supplémentaires pour mieux comprendre chaque cluster, mais cela peut donner de bonnes pistes.
Voici différentes interprétations possibles de ces résultats :

* Le cluster 0 semble correspondre à un groupe de clients non actifs (toutes les valeurs sont égales à zéro).
* Le cluster 3 semble être un groupe qui se distingue pour ce qui est des retours.

Le cluster 0 est un ensemble de clients qui ne sont manifestement pas actifs. Peut-être pouvez-vous axer vos actions marketing sur ce groupe de façon à l’inciter à acheter. À l’étape suivante, vous allez adresser une requête à la base de données pour obtenir les adresses e-mail des clients du cluster 0 dans le but de leur envoyer des e-mails marketing.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne poursuivez pas ce tutoriel, supprimez la base de données tpcxbb_1gb.

## <a name="next-steps"></a>Étapes suivantes

Dans la troisième partie de cette série de tutoriels, vous avez effectué les étapes suivantes :

* Définir le nombre de clusters pour un algorithme K-moyennes
* Effectuer le clustering
* Analyser les résultats

Pour déployer le modèle de Machine Learning que vous avez créé, suivez la quatrième partie de cette série de tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel Python : Déployer un modèle de clustering](python-clustering-model-deploy.md)