---
description: sys.dm_db_missing_index_groups (Transact-SQL)
title: sys. dm_db_missing_index_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_groups
- sys.dm_db_missing_index_groups_TSQL
- dm_db_missing_index_groups
- dm_db_missing_index_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_groups dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_groups dynamic management view
ms.assetid: 9cc00acd-d83d-49f8-be72-5b2aebed246b
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de673925756a51f10f39a1b280f245f484012a81
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88460431"
---
# <a name="sysdm_db_missing_index_groups-transact-sql"></a>sys.dm_db_missing_index_groups (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette DMV retourne des informations sur les index manquants dans un groupe d’index spécifique, à l’exception des index spatiaux. 
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**index_group_handle**|**int**|Identifie un groupe d'index manquants.|  
|**index_handle**|**int**|Identifie un index manquant qui appartient au groupe spécifié par **index_group_handle**.<br /><br /> Un groupe d'index ne contient qu'un seul index.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_db_missing_index_groups** sont mises à jour lorsqu'une requête est optimisée par l'optimiseur de requête, et elles ne sont pas conservées de manière permanente. Les informations sur les index manquants sont simplement conservées jusqu'au redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs de base de données doivent effectuer régulièrement des copies de sauvegarde des informations sur les index manquants s'ils souhaitent les conserver après le recyclage du serveur.  
  
 Aucune des deux colonnes de l'ensemble de résultats de sortie n'est une clé, mais ensemble, les colonnes constituent une clé d'index.  

  >[!NOTE]
  >Le jeu de résultats pour cette DMV est limité à 600 lignes. Chaque ligne contient un index manquant. Si vous avez plus de 600 index manquants, vous devez traiter les index manquants existants afin de pouvoir en afficher les plus récents.
  
## <a name="permissions"></a>Autorisations  
 Pour interroger cette vue de gestion dynamique, les utilisateurs doivent bénéficier de l'autorisation VIEW SERVER STATE ou de tout privilège qui implique l'autorisation VIEW SERVER STATE.  
  
## <a name="see-also"></a>Voir aussi  
 [sys. dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys. dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys. dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
