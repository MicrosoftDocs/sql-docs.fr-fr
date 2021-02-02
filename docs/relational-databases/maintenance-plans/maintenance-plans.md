---
title: Plans de maintenance | Microsoft Docs
description: Découvrez les plans de maintenance, qui créent un flux de travail pour s’assurer que votre base de données SQL Server est optimisée, sauvegardée régulièrement et exempte d’incohérences.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.AG.MAINTPLAN.LEGACY.F1
helpviewer_keywords:
- maintenance plans [SQL Server], about database maintenance plans
- maintenance plans [SQL Server], database compatibility level displayed in designer
- maintenance plans [SQL Server]
ms.assetid: 5982ca65-74fe-44e3-aef9-00a65a0db169
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6eb1586c38283d695e96d4640d37c261314a339f
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048915"
---
# <a name="maintenance-plans"></a>Plans de maintenance
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Les plans de maintenance vous permettent de créer un flux de travail des tâches nécessaires à l'optimisation de votre base de données, à la création d'une sauvegarde régulière et à la recherche des incohérences. L'Assistant Plan de maintenance permet aussi de créer les principaux plans de maintenance, mais la création manuelle de ces plans offre beaucoup plus de souplesse.  
  
## <a name="benefits-of-maintenance-plans"></a>Avantages des plans de maintenance  
 Dans le [!INCLUDE[ssDECurrent](../../includes/ssdecurrent-md.md)], les plans de maintenance créent un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui est exécuté par un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les plans de maintenance peuvent s'exécuter manuellement ou automatiquement à intervalles planifiés.  
  
 Les plans de maintenance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] fournissent les fonctionnalités suivantes :  
  
-   Création d'un flux de travail à l'aide d'une série de tâches de maintenance standard. Vous pouvez également créer vos propres scripts [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Hiérarchies conceptuelles. Chaque plan permet de créer ou de modifier les flux de travail des tâches. Les tâches de chaque plan peuvent être regroupées en sous-plans, qui peuvent être planifiés pour s'exécuter à différents moments.  
  
-   Prise en charge des plans multiserveurs destinés à des environnements serveur maître/serveur cible.  
  
-   Prise en charge de l'enregistrement de l'historique de plan sur des serveurs distants.  
  
-   Prise en charge de l'authentification Windows et de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="maintenance-plan-functionality"></a>Fonctionnalité de plan de maintenance  
 Vous pouvez créer des plans de maintenance pour effectuer les tâches suivantes :  
  
-   Réorganisation des données sur les pages de données et d'index en reconstruisant les index à l'aide d'un nouveau taux de remplissage. La reconstruction des index à l'aide d'un nouveau taux de remplissage permet de garantir que les données et l'espace libre sont uniformément répartis entre les pages de la base de données. Elle permet également une croissance plus rapide dans le futur. Pour plus d’informations, consultez [Spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
-   Compression des fichiers de données en supprimant les pages de base de données vides.  
  
-   Mise à jour des statistiques d'index pour s'assurer que l'optimiseur de requête dispose des informations à jour quant à la répartition des valeurs de données dans les tables. L'optimiseur de requête est ainsi en mesure de mieux apprécier la meilleure façon d'accéder aux données puisqu'il dispose d'informations supplémentaires sur les données stockées dans la base. Bien que les statistiques d'index soient automatiquement et régulièrement mises à jour par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette option peut imposer la mise à jour immédiate de ces statistiques.  
  
-   Exécution de vérifications de la cohérence interne des données et des pages de données de la base de données, de façon à s'assurer qu'un problème système ou logiciel n'a pas endommagé des données.  
  
-   Sauvegarde des fichiers de base de données et des journaux de transactions. Les sauvegardes de la base de données et des journaux peuvent être conservées pendant une période donnée. Vous pouvez ainsi créer un historique des sauvegardes à utiliser si vous devez restaurer la base de données à un point dans le temps antérieur à la dernière sauvegarde de la base de données. Vous pouvez également effectuer des sauvegardes différentielles.  
  
-   Exécution des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette tâche peut servir à créer des travaux effectuant diverses actions, ainsi que les plans de maintenance pour exécuter les travaux.  
  
 Les résultats générés par les tâches de maintenance peuvent être écrits en tant que rapport dans un fichier texte ou dans les tables du plan de maintenance, **sysmaintplan_log** et **sysmaintplan_logdetail**, de la table **msdb**. Pour consulter les résultats dans la visionneuse du fichier journal, cliquez avec le bouton droit sur **Plans de maintenance**, puis cliquez sur **Afficher l’historique**.  
  
## <a name="related-tasks"></a>Tâches associées  
 Utilisez les rubriques suivantes pour commencer à utiliser les plans de maintenance.  
  
|Description|Rubrique|  
|-|-|  
|Paramétrez l’option de configuration de serveur **Agent XPs** pour activer les procédures stockées étendues de l’Agent SQL Server.|[Agent XPs (option de configuration de serveur)](../../database-engine/configure-windows/agent-xps-server-configuration-option.md)|
|Explique comment créer un plan de maintenance à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].|[Créer un plan de maintenance](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)|  
|Explique comment créer un plan de maintenance à l'aide de l'aire de conception du plan de maintenance.|[Créer un plan de maintenance &#40;aire de conception de plan de maintenance&#41;](../../relational-databases/maintenance-plans/create-a-maintenance-plan-maintenance-plan-design-surface.md)|  
|Documente la fonction de plan de maintenance disponible dans l'Explorateur d'objets.|[Nœud Plans de maintenance &#40;Explorateur d’objets&#41;](../../relational-databases/maintenance-plans/maintenance-plans-node-object-explorer.md)|  
  
  
