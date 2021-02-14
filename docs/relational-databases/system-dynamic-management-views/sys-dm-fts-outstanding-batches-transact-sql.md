---
description: sys.dm_fts_outstanding_batches (Transact-SQL)
title: sys.dm_fts_outstanding_batches (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_outstanding_batches
- dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches
dev_langs:
- TSQL
helpviewer_keywords:
- troubleshooting [SQL Server], full-text search
- sys.dm_fts_outstanding_batches dynamic management view
ms.assetid: c4d697ed-c906-4c28-b137-036a25e13c84
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2932f123ac2b7f1125d187c943645c15d09e52d0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347932"
---
# <a name="sysdm_fts_outstanding_batches-transact-sql"></a>sys.dm_fts_outstanding_batches (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations à propos de chaque lot d'indexation de texte intégral.  
  
  |Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données|  
|catalog_id|**int**|ID du catalogue de texte intégral|  
|table_id|**int**|ID de l'ID de table qui contient l'index de texte intégral|  
|batch_id|**int**|ID de lot|  
|memory_address|**varbinary (8)**|Adresse mémoire de l'objet de lot|  
|crawl_memory_address|**varbinary (8)**|Adresse mémoire de l'objet d'analyse (objet parent)|  
|memregion_memory_address|**varbinary (8)**|Adresse mémoire de la région mémoire de la mémoire de partage sortante de l'hôte de démon de filtre (fdhost.exe)|  
|hr_batch|**int**|Code d'erreur le plus récent pour le lot|  
|is_retry_batch|**bit**|Indique s'il s'agit d'une nouvelle tentative de lot :<br /><br /> 0 = Non<br /><br /> 1 = Oui|  
|retry_hints|**int**|Type de nouvelle tentative nécessaire pour le lot :<br /><br /> 0 = pas de nouvelle tentative<br /><br /> 1 = nouvelle tentative multithread<br /><br /> 2 = nouvelle tentative à thread unique<br /><br /> 3 = nouvelle tentative à thread unique et multithread<br /><br /> 5 = dernière tentative multithread<br /><br /> 6 = dernière tentative à thread unique<br /><br /> 7 = dernière tentative à thread unique et multithread|  
|retry_hints_description|**nvarchar(120)**|Description du type de nouvelle tentative nécessaire :<br /><br /> pas de nouvelle tentative<br /><br /> nouvelle tentative multithread<br /><br /> nouvelle tentative à thread unique<br /><br /> nouvelle tentative à thread unique et multithread<br /><br /> dernière tentative multithread<br /><br /> dernière tentative à thread unique<br /><br /> dernière tentative à thread unique et multithread|  
|doc_failed|**bigint**|Nombre de documents ayant échoué dans le lot|  
|batch_timestamp|**timestamp**|Valeur d'horodatage obtenue lorsque le lot a été créé|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant recherche le nombre de lots traités actuellement pour chaque table dans l'instance de serveur.  
  
```  
SELECT database_id, table_id, COUNT(*) AS batch_count FROM sys.dm_fts_outstanding_batches GROUP BY database_id, table_id ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
