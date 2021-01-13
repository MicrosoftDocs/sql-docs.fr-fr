---
title: Mise à niveau du moteur de base de données | Microsoft Docs
description: Cet article décrit quelques étapes supplémentaires que vous devrez peut-être effectuer une fois la mise à niveau du Moteur de base de données de SQL Server terminée.
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 3f08087e-e532-416c-8caa-e0ec88c57596
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c309c86f4f6609f1bd5383d311e1ce5781350fd9
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975434"
---
# <a name="complete-the-database-engine-upgrade"></a>Mise à niveau du moteur de base de données

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Une fois effectuée la mise à niveau de SQL Server, vous devrez probablement exécuter quelques tâches supplémentaires. Ces options en question sont les suivantes :  
  
Après la mise à niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)], effectuez les tâches suivantes :  
  
- **Sauvegardez vos bases de données :** effectuez une sauvegarde complète de chaque base de données.  

- **Activez les nouvelles fonctionnalités :** dans SQL Server 2016, 2017 et 2019, certaines modifications sont activées uniquement une fois que le niveau DATABASE_COMPATIBILITY d’une base de données est passé à 130 ou plus.  Pour obtenir plus d’informations et accéder au flux de travail recommandé, consultez [Modifier le mode de compatibilité de base de données et utiliser le magasin des requêtes](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Si votre base de données comporte des tables à mémoire optimisée créées dans SQL Server 2014, passez en revue les [Statistiques pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).
  
- **Integration Services :**  
  
     Migrez les packages Integration Services vers le format [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Pour plus d’informations, voir [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
- **Reporting Services :** pour la mise à niveau d’une nouvelle installation, restaurez les clés de chiffrement de Reporting Services. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- **Master Data Services :**  mettez à niveau le schéma de la base de données MDS et créez l’application web de SQL Server 2019. Pour plus d’informations, consultez [Upgrade Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md).  
  
- **Data Quality Services :** mettez à niveau le schéma de bases de données DQS et vérifiez la mise à niveau du schéma de bases de données DQS. Pour plus d’informations, consultez [Upgrade Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
  
- **Recherche en texte intégral :** Alimentez à nouveau les catalogues de texte intégral pour garantir la cohérence sémantique dans les résultats de la requête. Pour plus d’informations, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).  
  
