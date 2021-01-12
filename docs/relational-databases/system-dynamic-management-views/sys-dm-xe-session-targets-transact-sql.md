---
description: sys.dm_xe_session_targets (Transact-SQL)
title: sys.dm_xe_session_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_session_targets
- dm_xe_session_targets_TSQL
- dm_xe_session_targets
- sys.dm_xe_session_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_session_targets dynamic management view
- extended events [SQL Server], views
ms.assetid: 76fbc3e1-ad88-4a47-8bf1-471c3bee5ad8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 041cf14bcefff0d294cd7c4f4f037870d1513552
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098867"
---
# <a name="sysdm_xe_session_targets-transact-sql"></a>sys.dm_xe_session_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur les cibles d’une session d’événements.  
  
  |Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary (8)**|Adresse mémoire de la session d'événements. A une relation plusieurs-à-un avec sys.dm_xe_sessions.address. N'accepte pas la valeur NULL.|  
|target_name|**nvarchar(60)**|Nom de la cible au sein d'une session. N'accepte pas la valeur NULL.|  
|target_package_guid|**uniqueidentifier**|GUID du package qui contient la cible. N'accepte pas la valeur NULL.|  
|execution_count|**bigint**|Nombre d'exécutions de la cible pour la session. N'accepte pas la valeur NULL.|  
|execution_duration_ms|**bigint**|Temps d'exécution total de la cible, en millisecondes. N'accepte pas la valeur NULL.|  
|target_data|**nvarchar(max)**|Données gérées par la cible, par exemple les informations sur l'agrégation d'événements. Autorise la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
### <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_session_targets.event_session_address|sys.dm_xe_sessions.address|Plusieurs-à-un|  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Correction du type de données pour la colonne target_data.|  
|Correction de la description de la colonne target_data afin d'indiquer qu'elle accepte la valeur Null.|  
|Correction du tableau « Cardinalités des relations ».|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

