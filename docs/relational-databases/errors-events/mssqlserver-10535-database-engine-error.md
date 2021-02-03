---
description: MSSQLSERVER_10535
title: MSSQLSERVER_10535 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10535 (Database Engine error)
ms.assetid: 478fd978-11d9-4155-8329-f599fdbec14b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 430773e5dcd0d96b8b22359f07fc1ec1a39ffedf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197374"
---
# <a name="mssqlserver_10535"></a>MSSQLSERVER_10535
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|10535|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|PG_NO_PLAN|  
|Texte du message|Impossible de créer le repère de plan '%.*ls', car aucun plan n'a été trouvé dans le cache du plan qui correspond au handle de plan spécifié. Indiquez un handle de plan mis en cache. Pour obtenir la liste des handles de plan mis en cache, interrogez la vue de gestion dynamique sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Explication  
Aucun plan correspondant au handle de plan spécifié n'a été trouvé dans le cache du plan.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Indiquez un handle de plan mis en cache. Pour obtenir la liste des handles de plan mis en cache, interrogez la vue de gestion dynamique sys.dm_exec_query_stats.  
  
## <a name="see-also"></a>Voir aussi  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
