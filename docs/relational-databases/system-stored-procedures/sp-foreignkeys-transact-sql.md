---
description: sp_foreignkeys (Transact-SQL)
title: sp_foreignkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_foreignkeys_TSQL
- sp_foreignkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_foreignkeys
ms.assetid: 935fe385-19ff-41a4-8d0b-30618966991d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 377ef64c76238e2e840107dbbe6b32dc9ba4c00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189572"
---
# <a name="sp_foreignkeys-transact-sql"></a>sp_foreignkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les clés étrangères faisant référence aux clés primaires de la table du serveur lié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_foreignkeys [ @table_server = ] 'table_server'   
     [ , [ @pktab_name = ] 'pktab_name' ]   
     [ , [ @pktab_schema = ] 'pktab_schema' ]   
     [ , [ @pktab_catalog = ] 'pktab_catalog' ]   
     [ , [ @fktab_name = ] 'fktab_name' ]   
     [ , [ @fktab_schema = ] 'fktab_schema' ]   
     [ , [ @fktab_catalog = ] 'fktab_catalog' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_server = ] 'table_server'` Nom du serveur lié pour lequel les informations de table doivent être retournées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @pktab_name = ] 'pktab_name'` Nom de la table avec une clé primaire. *pktab_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @pktab_schema = ] 'pktab_schema'` Nom du schéma avec une clé primaire. *pktab_schema* est de **type sysname**, avec NULL comme valeur par défaut. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il contient le nom du propriétaire.  
  
`[ @pktab_catalog = ] 'pktab_catalog'` Nom du catalogue contenant une clé primaire. *pktab_catalog* est de **type sysname**, avec NULL comme valeur par défaut. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il contient le nom de la base de données.  
  
`[ @fktab_name = ] 'fktab_name'` Nom de la table avec une clé étrangère. *fktab_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @fktab_schema = ] 'fktab_schema'` Nom du schéma avec une clé étrangère. *fktab_schema* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @fktab_catalog = ] 'fktab_catalog'` Nom du catalogue avec une clé étrangère. *fktab_catalog* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Divers produits SGBD prennent en charge les noms de table en trois parties (_catalogue_)**.** _schéma_**.** _table_), qui est représenté dans le jeu de résultats.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**PKTABLE_CAT**|**sysname**|Catalogue de la table contenant la clé primaire.|  
|**PKTABLE_SCHEM**|**sysname**|Schéma de la table contenant la clé primaire.|  
|**PKTABLE_NAME**|**sysname**|Nom de la table (contenant la clé primaire). Ce champ retourne toujours une valeur.|  
|**PKCOLUMN_NAME**|**sysname**|Nom de la ou des colonnes de clé primaire, pour chaque colonne de la **table_name** retournée. Ce champ retourne toujours une valeur.|  
|**FKTABLE_CAT**|**sysname**|Catalogue de la table contenant la clé étrangère.|  
|**FKTABLE_SCHEM**|**sysname**|Schéma de la table contenant la clé étrangère.|  
|**FKTABLE_NAME**|**sysname**|Nom de la table (contenant une clé étrangère). Ce champ retourne toujours une valeur.|  
|**FKCOLUMN_NAME**|**sysname**|Nom des colonnes de clé étrangère, pour chacune des colonnes de la table TABLE_NAME retournées. Ce champ retourne toujours une valeur.|  
|**KEY_SEQ**|**smallint**|Numéro de séquence de la colonne dans une clé primaire multicolonne. Ce champ retourne toujours une valeur.|  
|**UPDATE_RULE**|**smallint**|Action appliquée à la clé étrangère lorsque l'opération SQL est une mise à jour. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne 0, 1 ou 2 pour ces colonnes :<br /><br /> 0=Modifications de type CASCADE apportées à la clé étrangère.<br /><br /> 1=Modifications de type NO ACTION en présence de clé étrangère.<br /><br /> 2=SET_NULL ; affectation de NULL à la clé étrangère.|  
|**DELETE_RULE**|**smallint**|Action appliquée à la clé étrangère lorsque l'opération SQL est une suppression. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne 0, 1 ou 2 pour ces colonnes :<br /><br /> 0=Modifications de type CASCADE apportées à la clé étrangère.<br /><br /> 1=Modifications de type NO ACTION en présence de clé étrangère.<br /><br /> 2=SET_NULL ; affectation de NULL à la clé étrangère.|  
|**FK_NAME**|**sysname**|Identificateur de clé étrangère. NULL si non applicable à la source de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le nom de la contrainte FOREIGN KEY.|  
|**PK_NAME**|**sysname**|Identificateur de clé primaire. NULL si non applicable à la source de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le nom de la contrainte PRIMARY KEY.|  
|**DEFERRABILITY**|**smallint**|Indique si la vérification des contraintes peut être différée.|  
  
 Dans le jeu de résultats, les colonnes FK_NAME et PK_NAME retournent toujours une valeur NULL.  
  
## <a name="remarks"></a>Notes  
 **sp_foreignkeys** interroge l’ensemble de lignes foreign_keys de l’interface **IDBSchemaRowset** du fournisseur OLE DB qui correspond à *table_server*. Les paramètres *table_name*, *TABLE_SCHEMA*, *TABLE_CATALOG* et *Column* sont passés à cette interface pour limiter les lignes retournées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations de clé étrangère relatives à la table `Department` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] située sur le serveur lié `Seattle1`.  
  
```  
EXEC sp_foreignkeys @table_server = N'Seattle1',   
   @pktab_name = N'Department',   
   @pktab_catalog = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
