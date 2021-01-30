---
description: sys.dm_fts_active_catalogs (Transact-SQL)
title: sys.dm_fts_active_catalogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_active_catalogs_TSQL
- dm_fts_active_catalogs
- dm_fts_active_catalogs_TSQL
- sys.dm_fts_active_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_active_catalogs dynamic management view
ms.assetid: 40ab5453-040c-4d2e-bb49-e340cf90c3ee
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5c9534a522e53f94d0116b9276a9080933b57453
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195160"
---
# <a name="sysdm_fts_active_catalogs-transact-sql"></a>sys.dm_fts_active_catalogs (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations concernant les catalogues de texte intégral qui ont une activité de remplissage en cours sur le serveur.  
  
> [!NOTE]
>  Les colonnes suivantes seront supprimées dans une version ultérieure de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : is_paused, previous_status, previous_status_description, row_count_in_thousands, Status, status_description et worker_count. Évitez par conséquent d'utiliser ces colonnes dans un nouveau travail de développement et prévoyez la modification des applications qui les utilisent actuellement.  
  
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données contenant le catalogue de texte intégral actif.|  
|**catalog_id**|**int**|ID du catalogue de texte intégral actif.|  
|**memory_address**|**varbinary (8)**|Adresse des mémoires tampons allouées pour l'activité de remplissage liée à ce catalogue de texte intégral.|  
|**name**|**nvarchar(128)**|Nom du catalogue de texte intégral actif.|  
|**is_paused**|**bit**|Indique si le remplissage du catalogue de texte intégral actif a été suspendu.|  
|**statut**|**int**|État actuel du catalogue de texte intégral. Celui-ci peut avoir l'une des valeurs suivantes :<br /><br /> 0 = En cours d'initialisation<br /><br /> 1 = Prêt<br /><br /> 2 = En pause <br /><br /> 3 = Erreur temporaire<br /><br /> 4 = Doit être remonté<br /><br /> 5 = Arrêt<br /><br /> 6 = Suspendu pour sauvegarde<br /><br /> 7 = Sauvegarde en cours via le catalogue<br /><br /> 8 = Catalogue endommagé|  
|**status_description**|**nvarchar(120)**|Description de l'état actuel du catalogue de texte intégral actif.|  
|**previous_status**|**int**|État précédent du catalogue de texte intégral. Celui-ci peut avoir l'une des valeurs suivantes :<br /><br /> 0 = En cours d'initialisation<br /><br /> 1 = Prêt<br /><br /> 2 = En pause <br /><br /> 3 = Erreur temporaire<br /><br /> 4 = Doit être remonté<br /><br /> 5 = Arrêt<br /><br /> 6 = Suspendu pour sauvegarde<br /><br /> 7 = Sauvegarde en cours via le catalogue<br /><br /> 8 = Catalogue endommagé|  
|**previous_status_description**|**nvarchar(120)**|Description de l'état précédent du catalogue de texte intégral actif.|  
|**worker_count**|**int**|Nombre de threads opérant actuellement sur ce catalogue de texte intégral.|  
|**active_fts_index_count**|**int**|Nombre d'index de recherche en texte intégral en cours d'alimentation.|  
|**auto_population_count**|**int**|Nombre de tables en cours d'alimentation automatique pour ce catalogue de texte intégral.|  
|**manual_population_count**|**int**|Nombre de tables en cours d'alimentation manuelle pour ce catalogue de texte intégral.|  
|**full_incremental_population_count**|**int**|Nombre de tables en cours de remplissage complet ou incrémentiel pour ce catalogue de texte intégral.|  
|**row_count_in_thousands**|**int**|Nombre estimé de lignes (en milliers) dans tous les index de texte intégral de ce catalogue de texte intégral.|  
|**is_importing**|**bit**|Indique si le catalogue de texte intégral est en cours d'importation :<br /><br /> 1 = le catalogue est en cours d'importation.<br /><br /> 2 = le catalogue n'est pas en cours d'importation.|  
  
## <a name="remarks"></a>Notes  
 La colonne is_importing est nouvelle dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
   
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures significatives de cette vue de gestion dynamique](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-active-catalogs-1.gif "Jointures significatives de cette vue de gestion dynamique")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relation|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|Un-à-un|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|Un-à-un|  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne des informations sur les catalogues de texte intégral actifs de la base de données active.  
  
```  
SELECT catalog.name, catalog.is_importing, catalog.auto_population_count,  
  OBJECT_NAME(population.table_id) AS table_name,  
  population.population_type_description, population.is_clustered_index_scan,  
  population.status_description, population.completion_type_description,  
  population.queued_population_type_description, population.start_time,  
  population.range_count   
FROM sys.dm_fts_active_catalogs catalog   
CROSS JOIN sys.dm_fts_index_population population   
WHERE catalog.database_id = population.database_id   
AND catalog.catalog_id = population.catalog_id   
AND catalog.database_id = (SELECT dbid FROM sys.sysdatabases WHERE name = DB_NAME());  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  
