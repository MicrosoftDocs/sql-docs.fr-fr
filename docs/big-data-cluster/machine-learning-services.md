---
title: Machine Learning Services (Python, R)
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment exécuter des scripts Python et R sur l’instance maître de clusters Big Data SQL Server avec Machine Learning Services.
author: dphansen
ms.author: davidph
ms.date: 11/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: af0611396dcd50fcbea41e96ecc9d651b56a91d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047526"
---
# <a name="run-python-and-r-scripts-with-machine-learning-services-on-sql-server-big-data-clusters"></a>Exécuter des scripts Python et R avec Machine Learning Services sur des clusters Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Vous pouvez exécuter des scripts Python et R sur l’instance maître de [clusters Big Data SQL Server](big-data-cluster-overview.md) avec [Machine Learning Services](../machine-learning/index.yml).

> [!NOTE]
> Vous pouvez également exécuter du code Java sur l’instance maître des clusters Big Data SQL Server avec l’[extension de langage Java](../language-extensions/java-overview.md). Suivez les étapes ci-dessous pour activer également les [extensions de langage SQL Server](../language-extensions/language-extensions-overview.md).

## <a name="enable-machine-learning-services"></a>Activer Machine Learning Services

Machine Learning Services est installé par défaut sur les clusters Big Data et nécessite une installation distincte.

Pour activer Machine Learning Services, exécutez l’instruction suivante sur l’instance maître :

```sql
EXEC sp_configure 'external scripts enabled', 1
RECONFIGURE WITH OVERRIDE
GO
```

Vous êtes désormais prêt à exécuter les scripts Python et R sur l’instance maître de clusters Big Data. Consultez les guides de démarrage rapide sous [Étapes suivantes](#next-steps) pour exécuter votre premier script.

>[!NOTE]
>Le paramètre de configuration ne peut pas être défini sur une connexion d’écouteur de groupe de disponibilité. Si vous déployez des clusters Big Data avec une haute disponibilité, définissez `external scripts enabled` sur chaque réplica. Consultez [Activer sur un cluster haute disponibilité](#enable-on-cluster-with-high-availability).

## <a name="enable-on-cluster-with-high-availability"></a>Activer sur un cluster haute disponibilité

Quand vous [déployez un cluster Big Data SQL Server haute disponibilité](deployment-high-availability.md), le déploiement crée un groupe de disponibilité pour l’instance maître. Pour activer Machine Learning Services, définissez `external scripts enabled` sur chaque instance du groupe de disponibilité. Pour un cluster Big Data, vous devez exécuter `sp_configure` sur chaque réplica de l’instance maître SQL Server.

La section suivante décrit comment activer des scripts externes sur chaque instance.

### <a name="create-an-external-load-balancer-for-each-instance"></a>Créer un équilibreur de charge externe pour chaque instance

Pour chaque réplica du groupe de disponibilité, créez un équilibreur de charge pour vous permettre de vous connecter à l’instance. 

`kubectl expose pod <pod-name> --port=<connection port number> --name=<load-balancer-name> --type=LoadBalancer -n <kubernetes namespace>`

Nous utilisons les valeurs suivantes dans les exemples de cet article :

- `<pod-name>`: `master-#`
- `<connection port number>`: `1533`
- `<load-balancer-name>`: `mymaster-#`
- `<kubernetes namespace>`: `mssql-cluster`

Mettez à jour le script suivant en fonction de votre environnement et exécutez les commandes suivantes :

```bash
kubectl expose pod master-0 --port=1533 --name=mymaster-0 --type=LoadBalancer -n mssql-cluster 
kubectl expose pod master-1 --port=1533 --name=mymaster-1 --type=LoadBalancer -n mssql-cluster
kubectl expose pod master-2 --port=1533 --name=mymaster-2 --type=LoadBalancer -n mssql-cluster 
```

`kubectl` retourne la sortie suivante.

```bash
service/mymaster-0 exposed
service/mymaster-1 exposed
service/mymaster-2 exposed
```

Chaque équilibreur de charge est un point de terminaison de réplica maître.

### <a name="enable-script-execution-on-each-replica"></a>Activer l’exécution du script sur chaque réplica

1. Obtenir l’adresse IP du point de terminaison de réplica maître.

   La commande suivante retourne l’adresse IP externe du point de terminaison de réplica. 

   `kubectl get services <load-balancer-name> -n <kubernetes namespace>`

   Pour obtenir l’adresse IP externe de chaque réplica dans ce scénario, exécutez les commandes suivantes :

   ```bash
   kubectl get services mymaster-0 -n mssql-cluster
   kubectl get services mymaster-1 -n mssql-cluster
   kubectl get services mymaster-2 -n mssql-cluster
   ```

   >[!NOTE]
   > Vous devrez peut-être attendre un peu avant que l’adresse IP externe ne soit disponible. Exécutez le script précédent régulièrement jusqu’à ce que chaque point de terminaison retourne une adresse IP externe.

1. Connectez-vous au point de terminaison de réplica maître et activez l’exécution du script.

    Exécutez l’instruction suivante :

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    GO
    ```

   Par exemple, vous pouvez exécuter la commande précédente avec `sqlcmd`. L’exemple suivant se connecte au point de terminaison de réplica maître et permet l’exécution du script. Mettez à jour les valeurs du script en fonction de votre environnement.

   ```bash
   sqlcmd -S <IP address>,1533 -U <user name> -P <password> -Q "EXEC sp_configure 'external scripts enabled', 1; RECONFIGURE WITH OVERRIDE;"
   ```

   Répétez l’étape pour chaque réplica.

### <a name="demonstration"></a>Démonstration

L’image suivante illustre ce processus.

[![Démonstration](media/machine-learning-services/example-kube-enable-scripts.png "Démontrer l’activation de la fonctionnalité sur Kubernetes")](media/machine-learning-services/example-kube-enable-scripts.png#lightbox)

Vous êtes désormais prêt à exécuter les scripts Python et R sur l’instance maître de clusters Big Data. Consultez les guides de démarrage rapide sous [Étapes suivantes](#next-steps) pour exécuter votre premier script.

### <a name="delete-the-master-replica-endpoints"></a>Supprimer les points de terminaison de réplica maître

Sur le cluster Kubernetes, supprimez le point de terminaison de chaque réplica. Le point de terminaison est exposé dans Kubernetes en tant que service d’équilibrage de charge.

La commande suivante supprime le service d’équilibrage de charge.

`kubectl delete svc <load-balancer-name> -n mssql-cluster`

Pour les exemples de cet article, exécutez les commandes suivantes.

```bash
kubectl delete svc mymaster-0 -n mssql-cluster
kubectl delete svc mymaster-1 -n mssql-cluster
kubectl delete svc mymaster-2 -n mssql-cluster
```

## <a name="next-steps"></a>Étapes suivantes

+ [Exécuter des scripts Python simples](../machine-learning/tutorials/quickstart-python-create-script.md?toc=/sql/toc.json)
+ [Apprentissage et scoring d’un modèle prédictif en Python](../machine-learning/tutorials/quickstart-python-train-score-model.md?toc=/sql/toc.json)
+ [Exécuter des scripts R simples](../machine-learning/tutorials/quickstart-r-create-script.md?toc=/sql/toc.json)
+ [Apprentissage et scoring d’un modèle prédictif en R](../machine-learning/tutorials/quickstart-r-train-score-model.md?toc=/sql/toc.json)
