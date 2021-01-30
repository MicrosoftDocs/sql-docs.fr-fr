---
description: sp_tables_ex (Transact-SQL)
title: sp_tables_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables_ex
- sp_tables_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables_ex
ms.assetid: 33755c33-7e1e-4ef7-af14-a9cebb1e2ed4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0126cfd14adde25d88d6990a5d7e78c2141ea21e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182760"
---
# <a name="sp_tables_ex-transact-sql"></a>sp_tables_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations de table sur les tables provenant du serveur lié spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_tables_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]  
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @table_type = ] 'table_type' ]   
     [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_server = ] 'table_server'` Nom du serveur lié pour lequel les informations de table doivent être retournées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
``[ , [ @table_name = ] 'table_name']`` Nom de la table pour laquelle les informations sur le type de données doivent être retournées. *table_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_schema = ] 'table_schema']` Est le schéma de la table. *TABLE_SCHEMA* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_catalog = ] 'table_catalog'` Nom de la base de données dans laquelle réside le *table_name* spécifié. *TABLE_CATALOG* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_type = ] 'table_type'` Type de la table à retourner. *TABLE_TYPE* est de **type sysname**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**AFFECTÉ**|Nom d'un alias.|  
|**GLOBAL TEMPORARY**|Nom d'une table temporaire disponible au niveau du système.|  
|**LOCAL TEMPORARY**|Nom d'une table temporaire disponible uniquement au niveau du travail en cours.|  
|**SYNONYM**|Nom d'un synonyme.|  
|**TABLE SYSTÈME**|Nom d'une table système.|  
|**VUE SYSTÈME**|Nom d'une vue système.|  
|**TABLE**|Nom d'une table utilisateur.|  
|**VIEW**|Nom d'une vue.|  
  
`[ @fUsePattern = ] 'fUsePattern'` Détermine si les caractères **_**, **%** , **[** et **]** sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de la table. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans certains autres produits, elle représente le nom du serveur de l’environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**TABLE_TYPE**|**varchar(32)**|Table, table système ou vue.|  
|**NOTES**|**varchar (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
  
## <a name="remarks"></a>Notes  
 **sp_tables_ex** est exécuté en interrogeant l’ensemble de lignes tables de l’interface **IDBSchemaRowset** du fournisseur OLE DB correspondant à *table_server*. Les paramètres *table_name*, *TABLE_SCHEMA*, *TABLE_CATALOG* et *Column* sont passés à cette interface pour limiter les lignes retournées.  
  
 **sp_tables_ex** retourne un jeu de résultats vide si le fournisseur OLE DB du serveur lié spécifié ne prend pas en charge l’ensemble de lignes tables de l’interface **IDBSchemaRowset** .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les tables contenues dans le schéma `HumanResources` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], sur le serveur lié `LONDON2`.  
  
```  
EXEC sp_tables_ex @table_server = 'LONDON2',   
@table_catalog = 'AdventureWorks2012',   
@table_schema = 'HumanResources',   
@table_type = 'TABLE';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de requêtes distribuées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_columns_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-columns-ex-transact-sql.md)   
 [sp_column_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_table_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
