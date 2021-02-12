---
title: Qu’est-ce que le contrôleur ?
titleSuffix: SQL Server big data clusters
description: Cet article décrit le contrôleur d’un cluster Big Data SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9e2a7413578a8625231ffa0dad1750e98445f339
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039439"
---
# <a name="what-is-the-controller-on-a-sql-server-big-data-cluster"></a>Qu’est-ce que le contrôleur sur un cluster Big Data SQL Server ?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Le contrôleur héberge la logique de base utilisée pour déployer et gérer un cluster Big Data SQL Server. Il se charge de toutes les interactions avec Kubernetes, des instances SQL Server qui font partie du cluster et d’autres composants tels que HDFS et Spark.

Le service de contrôleur fournit les fonctionnalités de base suivantes :

- Gérer le cycle de vie du cluster : amorçage et suppression du cluster, mise à jour des configurations
- Gérer les instances SQL Server maîtres
- Gérer les pools de calcul, de données et de stockage
- Exposer les outils de supervision pour observer l’état du cluster
- Exposer les outils de dépannage pour détecter et réparer les problèmes inattendus
- Gérer la sécurité du cluster :
  - Garantir la sécurité des points de terminaison de cluster
  - Gérer les utilisateurs et les rôles
  - Configurer les informations d’identification pour la communication intra-cluster

## <a name="deploying-the-controller-service"></a>Déploiement du service de contrôleur

Le contrôleur est déployé et hébergé dans le même espace de noms Kubernetes où le client souhaite créer un cluster Big Data. Ce service est installé par un administrateur Kubernetes durant l’amorçage du cluster à l’aide de l’utilitaire de ligne de commande **azdata**. Pour plus d’informations, consultez [Prise en main des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).

Le workflow de création place sur Kubernetes un cluster Big Data SQL Server entièrement fonctionnel qui regroupe tous les composants décrits dans l’article [Vue d’ensemble](big-data-cluster-overview.md). Le workflow d’amorçage crée d’abord le service de contrôleur. Une fois déployé, il coordonne l’installation et la configuration du reste des services dans les pools (maître, calcul, données et stockage).

## <a name="managing-the-cluster-through-the-controller-service"></a>Gestion du cluster par le biais du service de contrôleur

Vous pouvez gérer le cluster par le biais du service de contrôleur à l’aide des commandes **azdata**. Si vous déployez d’autres objets Kubernetes comme des pods dans le même espace de noms, ils ne sont ni gérés ni supervisés par le service de contrôleur. Vous pouvez également utiliser des commandes **kubectl** pour gérer le cluster au niveau Kubernetes. Pour plus d’informations, consultez [Supervision et résolution des problèmes des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

Le contrôleur et les objets Kubernetes (ensembles avec état, pods, secrets, etc.) créés pour un cluster Big Data résident dans un espace de noms Kubernetes dédié. Le service de contrôleur est autorisé par l’administrateur de cluster Kubernetes à gérer toutes les ressources au sein de cet espace de noms.  La stratégie RBAC pour ce scénario est configurée automatiquement dans le cadre du déploiement initial du cluster avec **azdata**.

### <a name="azdata"></a>azdata

**azdata** est un utilitaire de ligne de commande écrit en Python qui permet aux administrateurs de clusters d’amorcer et de gérer des clusters Big Data par le biais d’API REST exposées par le service de contrôleur.

## <a name="controller-service-security"></a>Sécurité du service de contrôleur

Toutes les communications à destination du service de contrôleur sont effectuées par le biais d’une API REST sur HTTPS. Un certificat auto-signé est automatiquement généré pour vous au moment de l’amorçage. 

L’authentification auprès du point de terminaison du service de contrôleur utilise une identité Active Directory ou est basée sur un nom d’utilisateur et un mot de passe. Ces informations d’identification sont provisionnées au moment de l’amorçage du cluster à l’aide de l’entrée pour les variables d’environnement `AZDATA_USERNAME` et `AZDATA_PASSWORD`.

> [!NOTE]
> Vous devez fournir un mot de passe conforme aux [exigences en matière de complexité des mots de passe SQL Server](../relational-databases/security/password-policy.md).

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/microsoft/sqlworkshops-bdc)