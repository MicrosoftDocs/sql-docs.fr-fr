---
description: Tâche de création d’un cluster Azure HDInsight
title: Création de cluster Azure HDInsight, tâche | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpcreatecltask.f1
- sql14.dts.designer.afpcreatecltask.f1
ms.assetid: a8ec413a-38d3-45df-887e-6f5f4d9f8465
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f86a35316de80caa1a6db487518f0080f6097a17
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88496076"
---
# <a name="azure-hdinsight-create-cluster-task"></a>Tâche de création d’un cluster Azure HDInsight

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


La **tâche de création de cluster Azure HDInsight** permet à un package SSIS de créer un cluster Azure HDInsight dans le groupe de ressources et l’abonnement Azure spécifiés.
  
La **tâche de création de cluster Azure HDInsight** est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
  
> [!NOTE]  
> - La création d’un cluster HDInsight peut prendre 10 à 20 minutes.  
> - Il existe un coût associé à la création et à l’exécution d’un cluster Azure HDInsight. Pour plus d’informations, consultez [Tarification HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).  
  
Pour ajouter une **tâche de création de cluster Azure HDInsight**, faites-la glisser sur le concepteur SSIS, double-cliquez dessus ou cliquez dessus avec le bouton droit, puis cliquez sur **Modifier** pour afficher la boîte de dialogue **Éditeur de la tâche de création de cluster Azure HDInsight** .  
  
Le tableau suivant décrit les champs de cette boîte de dialogue.  
  
|Champ|Description|  
|-|-|  
|AzureResourceManagerConnection|Sélectionnez un gestionnaire de connexions Azure Resource Manager existant ou créez-en un qui sera utilisé pour créer le cluster HDInsight.|  
|AzureStorageConnection|Sélectionnez un gestionnaire de connexions Azure Storage existant ou créez-en un nouveau qui fait référence à un compte Azure Storage qui sera associé au cluster HDInsight.|
|SubscriptionId|Spécifiez l’ID de l’abonnement dans lequel le cluster HDInsight sera créé.|
|ResourceGroup|Spécifiez le groupe de ressources Azure dans lequel le cluster HDInsight sera créé.|
|Emplacement|Spécifiez l’emplacement du cluster HDInsight. Le cluster doit être créé au même emplacement que le compte de stockage Azure spécifié.|  
|ClusterName|Spécifiez un nom pour le cluster HDInsight à créer.|  
|clusterSize|Spécifiez le nombre de nœuds à créer dans le cluster.|  
|BlobContainer|Spécifiez le nom du conteneur de stockage par défaut à associer au cluster HDInsight.|  
|UserName|Spécifiez le nom d’utilisateur à utiliser pour la connexion au cluster HDInsight.|  
|Mot de passe|Spécifiez le mot de passe à utiliser pour la connexion au cluster HDInsight.|
|SshUserName|Spécifiez le nom d’utilisateur utilisé pour accéder à distance au cluster HDInsight à l’aide de SSH.|
|SshPassword|Spécifiez le mot de passe utilisé pour accéder à distance au cluster HDInsight à l’aide de SSH.|
|FailIfExists|Spécifiez si la tâche doit échouer si le cluster existe déjà.|  
  
> [!NOTE]  
> L’emplacement du cluster HDInsight et celui du compte de stockage Azure doivent être identiques.
