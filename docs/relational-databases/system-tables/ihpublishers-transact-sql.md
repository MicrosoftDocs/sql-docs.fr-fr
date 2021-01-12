---
description: IHpublishers (Transact-SQL)
title: IHpublishers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublishers
- IHpublishers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishers system table
ms.assetid: 77007246-f10b-4b87-8edf-7afc3c2096af
author: cawrites
ms.author: chadam
ms.openlocfilehash: 037778000e56bf49e99d46d560fd6fda8f4ce484
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092432"
---
# <a name="ihpublishers-transact-sql"></a>IHpublishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHpublishers** contient une ligne pour chaque serveur de publication non-SQL Server à l’aide du serveur de distribution actuel. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Identifie un serveur de publication non SQL Server.|  
|**vendor**|**sysname**|Nom du fournisseur de la base de données non SQL Server.|  
|**publisher_guid**|**uniqueidentifier**|GUID qui identifie le serveur de publication non SQL Server.|  
|**flush_request_time**|**datetime**|Indique les date et heure auxquelles a eu lieu la dernière modification des métadonnées d'article ayant amené l'Agent de lecture du journal à mettre à jour son cache de métadonnées.|  
|**version**|**sysname**|Chaîne de texte qui caractérise la version du serveur de publication non SQL Server.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)  
  
  
