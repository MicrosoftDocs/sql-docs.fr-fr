---
description: MSmerge_genhistory (Transact-SQL)
title: MSmerge_genhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_genhistory_TSQL
- MSmerge_genhistory
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_genhistory system table
ms.assetid: 475d08ae-eb8b-49de-afd6-33c96ab8004d
author: cawrites
ms.author: chadam
ms.openlocfilehash: a56a77aebc8b7643a178d23b7f5b444270091106
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209619"
---
# <a name="msmerge_genhistory-transact-sql"></a>MSmerge_genhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_genhistory** contient une ligne pour chaque génération qu’un abonné connaît (dans la période de rétention). Elle permet d'éviter l'envoi de générations communes au cours des échanges et de resynchroniser les Abonnés restaurés à partir de sauvegardes. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**guidsrc**|**uniqueidentifier**|Identificateur global des modifications identifiées par la génération au niveau de l'Abonné.|  
|**pubid**|**uniqueidentifier**|Identificateur de la publication.|  
|**toute**|**bigint**|Valeur de génération.|  
|**art_nick**|**int**|Surnom de l’article.|  
|**nicknames**|**varbinary (1001)**|Liste des surnoms des autres Abonnés qui possèdent déjà cette génération. L'utilisation de cet argument permet d'éviter l'envoi d'une génération à un Abonné qui a déjà consulté ces modifications. Les surnoms de cette liste sont classés par ordre alphabétique afin d'augmenter l'efficacité des recherches. Si le nombre de surnoms dépasse les capacités de ce champ, ces derniers ne bénéficieront pas de l'optimisation.|  
|**coldate**|**datetime**|Date d'ajout de la génération actuelle à la table.|  
|**genstatus**|**tinyint**|Le statut de la génération est le suivant :<br /><br /> **0** = ouvert.<br /><br /> **1** = fermé.<br /><br /> **2** = fermé et provient d’un autre abonné.|  
|**changecount**|**int**|Nombre de modifications réfléchies dans une génération donnée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
