---
description: Tâche Réorganiser l'index
title: Réorganiser l’index, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.reorganizeindextask.f1
helpviewer_keywords:
- reorganizing indexes
- Reorganize Index task [Integration Services]
- indexes [Integration Services]
ms.assetid: 9ed87861-e5c3-4fcd-8760-d112f4c0af0c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 307854616b6257c00bbcf9d5b66df6d00211b7b5
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92197163"
---
# <a name="reorganize-index-task"></a>Tâche Réorganiser l'index

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche Réorganiser l'index réorganise les index dans les vues et les tables de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur la gestion des index, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 La tâche Réorganiser l'index permet à un package de réorganiser les index dans une ou plusieurs bases de données. Si la tâche réorganise uniquement les index d'une base de données, vous pouvez choisir les vues ou les tables dont les index sont à réorganiser. En outre, la tâche Réorganiser l'index comprend une option qui permet de compacter les données d'objets volumineux. Les données d’objets volumineux peuvent avoir les types de données suivants : **image**, **text**, **ntext**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** ou **xml** . Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
 La tâche Réorganiser l'index encapsule l'instruction Transact-SQL ALTER INDEX. Si vous choisissez de compacter les données d'objet volumineux, l'instruction utilise la clause REORGANIZE WITH (LOB_COMPACTION = ON), sinon l'option LOB_COMPACTION a pour valeur OFF. Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
> [!IMPORTANT]  
>  Le temps que prend la tâche à créer l'instruction Transact-SQL qu'elle exécute est proportionnel au nombre d'index qu'elle réorganise. Si la tâche est configurée de manière à réorganiser les index dans toutes les tables et vues d'une base de données avec de nombreux index ou à réorganiser les index de plusieurs bases de données, elle peut prendre beaucoup de temps pour générer l'instruction Transact-SQL.  
  
## <a name="configuration-of-the-reorganize-index-task"></a>Configuration de la tâche Réorganiser l'index  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Cette tâche se trouve dans la section **Tâches du plan de maintenance** de la **boîte à outils** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Tâche Réorganiser l’index &#40;Plan de maintenance&#41;](../../relational-databases/maintenance-plans/reorganize-index-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Tâches associées  
 Pour plus d’informations sur la façon de définir ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Définir les propriétés d’une tâche ou d’un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
