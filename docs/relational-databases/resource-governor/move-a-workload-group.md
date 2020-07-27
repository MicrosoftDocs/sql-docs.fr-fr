---
title: Déplacer un groupe de charge de travail | Microsoft Docs
description: Découvrez comment déplacer un groupe de charge de travail de Resource Governor vers un pool de ressources différent à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.rg.properties_moveworkloadgroup.f1
helpviewer_keywords:
- workload groups [SQL Server], move
- Resource Governor, workload group move
ms.assetid: f2068636-6e53-486a-a6fc-c12de2a38424
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: a3ff2eff8f72499506aa129b064690693f1b14ff
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86456641"
---
# <a name="move-a-workload-group"></a>Déplacer un groupe de charge de travail
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Vous pouvez déplacer un groupe de charge de travail de Resource Governor vers un pool de ressources différent à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de Transact-SQL.  
  
-   **Avant de commencer :**  [Limitations et restrictions](#LimitationsRestrictions), [Autorisations](#Permissions)  
  
-   **Pour déplacer un groupe de charge de travail avec :**  [SQL Server Management Studio](#MoveWGSSMS), [Transact-SQL](#MoveWGTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
 Vous ne pouvez pas déplacer un groupe de charge de travail s'il existe une opération en attente de configuration de Resource Governor.  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Vous ne pouvez pas déplacer un groupe de charge de travail s'il existe une opération en attente de configuration de Resource Governor. Vous pouvez déterminer s’il existe une configuration en attente en interrogeant la vue de gestion dynamique [sys.dm_resource_governor_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md) pour obtenir l’état en cours d’is_configuration_pending.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Le déplacement d'un groupe de charge de travail nécessite l'autorisation CONTROL SERVER.  
  
##  <a name="move-a-workload-group-using-sql-server-management-studio"></a><a name="MoveWGSSMS"></a> Déplacer un groupe de charge de travail à l'aide de SQL Server Management Studio  
 **Pour déplacer un groupe de charge de travail à l'aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]**  
  
1.  Dans l'Explorateur d'objets, développez de manière récursive le nœud **Gestion** vers le bas jusqu'à **Resource Governor**.  
  
2.  Cliquez avec le bouton droit sur **Resource Governor** , puis sélectionnez **Propriétés**afin d’ouvrir la page **Propriétés de Resource Governor** .  
  
3.  Dans la fenêtre **Pools de ressources** , cliquez sur pool de ressources qui contient le groupe de charge de travail à déplacer. La fenêtre **Groupes de charge de travail** répertorie maintenant les groupes de charge de travail de ce pool de ressources.  
  
4.  Dans la fenêtre **Groupes de charge de travail** , cliquez avec le bouton droit sur la flèche vers la droite à gauche du groupe de charge de travail à déplacer, puis sélectionnez **Déplacer vers**. Cela affiche une fenêtre **Déplacer le groupe de charge de travail** .  
  
5.  Les pools de ressources disponibles sont affichés dans la fenêtre. Cliquez sur le nom du pool de ressources vers lequel vous souhaitez déplacer le groupe de charge de travail, puis cliquez sur **OK** pour effectuer cette action.  
  
6.  Cette action n'est pas effectuée tant que vous n'avez pas cliqué sur **OK**. Lorsque vous cliquez sur **OK**, l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE est exécutée.  
  
7.  Si l'opération de création ou de reconfiguration du pool de ressources ou du groupe de charge de travail échoue, un message d'erreur récapitulatif apparaît sous le titre de la page de propriétés. Pour consulter le message d'erreur détaillé, cliquez sur la flèche vers le bas du message d'erreur.  
  
##  <a name="move-a-workload-group-using-transact-sql"></a><a name="MoveWGTSQL"></a> Déplacer un groupe de charge de travail à l'aide de Transact-SQL  
 **Pour déplacer un groupe de charge de travail à l'aide de Transact-SQL**  
  
1.  Exécutez l’instruction **ALTER WORKLOAD GROUP** en spécifiant le nom du groupe de charge de travail à déplacer, ainsi que le pool de ressources vers lequel il doit être déplacé.  
  
2.  Exécutez l'instruction **ALTER RESOURCE GOVERNOR RECONFIGURE** .  
  
### <a name="example-transact-sql"></a>Exemple (Transact-SQL)  
 L'exemple suivant déplace un groupe de charge de travail nommé `groupAdhoc` vers le pool de ressources par défaut.  
  
```  
ALTER WORKLOAD GROUP groupAdhoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Créer un groupe de charge de travail](../../relational-databases/resource-governor/create-a-workload-group.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
