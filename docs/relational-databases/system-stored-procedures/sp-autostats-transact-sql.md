---
description: sp_autostats (Transact-SQL)
title: sp_autostats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_autostats_TSQL
- sp_autostats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_autostats
ms.assetid: d1df8c15-ee73-49eb-9d13-6e98943c3e38
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4a1746acf81c59b5b83a45d1be0403d4d58ad6c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199228"
---
# <a name="sp_autostats-transact-sql"></a>sp_autostats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Affiche ou modifie l'option de mise à jour automatique des statistiques, AUTO_UPDATE_STATISTICS, pour un index, un objet de statistiques, une table ou une vue indexée.  
  
 Pour plus d’informations sur l’option AUTO_UPDATE_STATISTICS, consultez [options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) et [statistiques](../../relational-databases/statistics/statistics.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_autostats [ @tblname = ] 'table_or_indexed_view_name'   
    [ , [ @flagc = ] 'stats_flag' ]   
    [ , [ @indname = ] 'statistics_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @tblname = ] 'table_or_indexed_view_name'` Nom de la table ou de la vue indexée sur laquelle afficher l’option AUTO_UPDATE_STATISTICS. *table_or_indexed_view_name* est de type **nvarchar (776)**, sans valeur par défaut.  
  
`[ @flagc = ] 'stats_flag'` Met à jour l’option AUTO_UPDATE_STATISTICS avec l’une des valeurs suivantes :  
  
 **on** = on  
  
 **off** = désactivé  
  
 Lorsque *stats_flag* n’est pas spécifié, affichez le paramètre de AUTO_UPDATE_STATISTICS actuel. *stats_flag* est de type **varchar (10)**, avec NULL comme valeur par défaut.  
  
`[ @indname = ] 'statistics_name'` Nom des statistiques à afficher ou à mettre à jour l’option AUTO_UPDATE_STATISTICS. Pour afficher les statistiques d'un index, vous pouvez utiliser le nom de l'index ; un index et son objet de statistiques correspondant portent le même nom.  
  
 *statistics_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *stats_flag* est spécifié, **sp_autostats** signale l’action qui a été effectuée, mais ne retourne aucun jeu de résultats.  
  
 Si *stats_flag* n’est pas spécifié, **sp_autostats** retourne le jeu de résultats suivant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Nom de l'index**|**sysname**|Nom de l'index ou des statistiques.|  
|**AUTOSTATS**|**varchar (3)**|Valeur actuelle de l'option AUTO_UPDATE_STATISTICS.|  
|**Dernière mise à jour**|**datetime**|Date de la mise à jour des statistiques la plus récente.|  
  
 Le jeu de résultats d’une table ou d’une vue indexée comprend les statistiques créées pour les index, les statistiques à une seule colonne générées avec l’option AUTO_CREATE_STATISTICS et les statistiques créées à l’aide de l’instruction [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) .  
  
## <a name="remarks"></a>Notes  
 Si l'index spécifié est désactivé ou si la table spécifiée a un index cluster désactivé, un message d'erreur s'affiche.  
  
 AUTO_UPDATE_STATISTICS est toujours désactivé (OFF) pour les tables optimisées en mémoire.  
  
## <a name="permissions"></a>Autorisations  
 Pour modifier l’option AUTO_UPDATE_STATISTICS, vous devez appartenir au rôle de base de données fixe **db_owner** ou à l’autorisation ALTER sur *table_name*. Pour afficher l’option AUTO_UPDATE_STATISTICS requiert l’appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-display-the-status-of-all-statistics-on-a-table"></a>R. Afficher l'état de toutes les statistiques d'une table  
 L'exemple suivant affiche l'état de toutes les statistiques de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product';  
GO  
```  
  
### <a name="b-enable-auto_update_statistics-for-all-statistics-on-a-table"></a>B. Activer AUTO_UPDATE_STATISTICS pour toutes les statistiques d'une table  
 L'exemple suivant active l'option AUTO_UPDATE_STATISTICS pour toutes les statistiques de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'ON';  
GO  
```  
  
### <a name="c-disable-auto_update_statistics-for-a-specific-index"></a>C. Désactiver AUTO_UPDATE_STATISTICS pour un index spécifique  
 L'exemple suivant désactive l'option AUTO_UPDATE_STATISTICS pour l'index `AK_Product_Name` de la table `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'OFF', AK_Product_Name;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
