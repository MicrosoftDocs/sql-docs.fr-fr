---
description: MSsnapshotdeliveryprogress (Transact-SQL)
title: MSsnapshotdeliveryprogress (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsnapshotdeliveryprogress_TSQL
- MSsnapshotdeliveryprogress
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshotdeliveryprogress system table
ms.assetid: 9164bfe2-6fc4-4b52-946a-09ea3cf67041
author: cawrites
ms.author: chadam
ms.openlocfilehash: b95ea6af571cc4691850ae73619a765668aa6669
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094747"
---
# <a name="mssnapshotdeliveryprogress-transact-sql"></a>MSsnapshotdeliveryprogress (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSsnapshotdeliveryprogress** est utilisée pour suivre les fichiers qui ont été remis à l’abonné lorsqu’un instantané est en cours d’application. Ces données permettent de reprendre la remise des fichiers si l'Agent de fusion ne parvient pas à fournir tous les fichiers pendant la session, de manière à ce que les mêmes fichiers ne soient pas de nouveau remis lors de la prochaine exécution de l'Agent. Cette table est stockée dans la base de données d'abonnement de l'Abonné.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_token**|**nvarchar(260)**|Identifie le chemin d'accès du dossier d'instantané à partir duquel le fichier a été remis. Pour les publications qui utilisent des filtres paramétrés, la chaîne **dynsnap** sera ajoutée à la valeur.|  
|**progress_token_hash**|**int**|Valeur de hachage générée en fonction de la valeur de *progress_token* qui est utilisée pour améliorer l’efficacité de la recherche pour une valeur de *progress_token* donnée.|  
|**progress_token**|**nvarchar (500)**|Identifie un fichier remis, sous la forme d'une combinaison du nom du fichier et de son chemin d'accès.|  
|**progress_timestamp**|**datetime**|Valeur **DateTime** qui indique quand un fichier d’instantané a été remis avec succès.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
