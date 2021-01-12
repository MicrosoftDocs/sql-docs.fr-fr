---
description: sys.dm_db_missing_index_details (Transact-SQL)
title: sys.dm_db_missing_index_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fdce3b3269d0b60c20d82064955c83416c4254eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095165"
---
# <a name="sysdm_db_missing_index_details-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations détaillées sur les index manquants, à l'exclusion des index spatiaux.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|Identifie un index manquant. L'identificateur est unique sur le serveur. **index_handle** est la clé de cette table.|  
|**database_id**|**smallint**|Identifie la base de données dans laquelle réside la table comportant les index manquants.|  
|**object_id**|**int**|Identifie la table dans laquelle est situé l'index manquant.|  
|**equality_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, qui contribuent aux prédicats d'égalité au format :<br /><br /> *table. colonne*  = *constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, qui contribuent aux prédicats d'inégalité, par exemple, les prédicats au format :<br /><br /> *table. colonne*  >  *constant_value*<br /><br /> Tout opérateur de comparaison autre que "=" exprime l'inégalité.|  
|**included_columns**|**nvarchar(4000)**|Liste de colonnes, séparées par des virgules, requises comme colonnes de couverture pour la requête. Pour plus d’informations sur les colonnes de couverture ou les colonnes incluses, consultez [créer des index avec des colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).<br /><br /> Pour les index optimisés en mémoire (à la fois les hachages et les non-cluster optimisés en mémoire), ignorez **included_columns**. Toutes les commandes de la table sont incluses dans chaque index optimisé en mémoire.|  
|**instruction**|**nvarchar(4000)**|Nom de la table dans laquelle est situé l'index manquant.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_db_missing_index_details** sont mises à jour lorsqu'une requête est optimisée par l'optimiseur de requête, et elles ne sont pas conservées de manière permanente. Les informations sur les index manquants sont simplement conservées jusqu'au redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs de base de données doivent effectuer régulièrement des copies de sauvegarde des informations sur les index manquants s'ils souhaitent les conserver après le recyclage du serveur.  
  
 Pour savoir à quels groupes d'index manquants appartient un index manquant, vous pouvez interroger la vue de gestion dynamique **sys.dm_db_missing_index_groups** en établissant une équijointure avec **sys.dm_db_missing_index_details** d'après la colonne **index_handle**.  

  >[!NOTE]
  >Le jeu de résultats pour cette DMV est limité à 600 lignes. Chaque ligne contient un index manquant. Si vous avez plus de 600 index manquants, vous devez traiter les index manquants existants afin de pouvoir en afficher les plus récents. 
  
## <a name="using-missing-index-information-in-create-index-statements"></a>Utilisation des informations sur les index manquants dans les instructions CREATE INDEX  
 Pour convertir les informations retournées par **sys.dm_db_missing_index_details** dans une instruction CREATE index pour les index à mémoire optimisée et sur disque, les colonnes d’égalité doivent être placées avant les colonnes d’inégalité, et ensemble, elles doivent créer la clé de l’index. Les colonnes incluses doivent être ajoutées à l'instruction CREATE INDEX à l'aide de la clause INCLUDE. Pour déterminer un ordre efficace pour les colonnes d'égalité, vous devez les organiser en fonction de leur sélectivité : répertoriez les colonnes les plus sélectives (les colonnes de gauche dans la liste des colonnes).  
  
 Pour plus d’informations sur les index optimisés en mémoire, consultez [index pour les Tables Memory-Optimized](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  
  
## <a name="transaction-consistency"></a>Cohérence transactionnelle  
 Si une transaction crée ou supprime une table, les lignes qui contiennent les informations d'index manquants concernant les objets supprimés sont retirées de cet objet de gestion dynamique, ce qui permet de préserver la cohérence des transactions.  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
