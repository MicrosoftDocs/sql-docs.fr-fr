---
description: MSmerge_history (Transact-SQL)
title: MSmerge_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_history_TSQL
- MSmerge_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_history system table
ms.assetid: 936195ad-ca07-41a8-a1a0-6699b6e63403
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 348df963920d35aeb874a83cad83701995d563cd
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89540867"
---
# <a name="msmerge_history-transact-sql"></a>MSmerge_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_history** contient des lignes d’historique avec des descriptions détaillées des résultats des sessions de travail précédentes agent de fusion. Cette table contient une ligne par ligne de sortie de l'Agent. Cette table est utilisée dans la base de données de distribution et dans chaque base de données d'abonnement. Dans la base de données de distribution, elle contient l'historique de toutes les publications de fusion et de tous les abonnements de fusion qui utilisent le serveur de distribution. Dans chaque base de données d'abonnement, il contient l'historique des publications auxquelles l'Abonné a souscrit.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID du travail d'Agent de fusion|  
|**agent_id**|**int**|ID de l'Agent de fusion|  
|**commentaires**|**nvarchar(255)**|Texte du message.|  
|**error_id**|**int**|ID d’une erreur dans la table système [MSrepl_errors](../../relational-databases/system-tables/msrepl-errors-transact-sql.md) .|  
|**timestamp**|**timestamp**|Colonne timestamp de cette table|  
|**updatable_row**|**bit**|Défini sur **1** si la ligne de l’historique peut être remplacée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
