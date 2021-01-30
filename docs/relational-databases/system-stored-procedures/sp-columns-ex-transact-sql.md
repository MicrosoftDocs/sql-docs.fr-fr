---
description: sp_columns_ex (Transact-SQL)
title: sp_columns_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_ex
- sp_columns_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns_ex
ms.assetid: c12ef6df-58c6-4391-bbbf-683ea874bd81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6696187b8f2b85b8a7ed179bfe406490e2c650aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174489"
---
# <a name="sp_columns_ex-transact-sql"></a>sp_columns_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie les informations de colonne, à raison d'une ligne par colonne, pour les tables du serveur lié spécifiées. **sp_columns_ex** retourne les informations de colonne uniquement pour la colonne spécifique si la *colonne* est spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_columns_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_server = ] 'table_server'` Nom du serveur lié pour lequel les informations de colonne doivent être retournées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @table_name = ] 'table_name'` Nom de la table pour laquelle les informations de colonne doivent être retournées. *table_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_schema = ] 'table_schema'` Nom de schéma de la table pour laquelle les informations de colonne doivent être retournées. *TABLE_SCHEMA* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_catalog = ] 'table_catalog'` Nom du catalogue de la table pour laquelle les informations de colonne doivent être retournées. *TABLE_CATALOG* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @column_name = ] 'column'` Nom de la colonne de base de données pour laquelle des informations doivent être fournies. *Column* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @ODBCVer = ] 'ODBCVer'` Est la version de ODBC en cours d’utilisation. *ODBCVer* est de **type int**, avec 2 comme valeur par défaut. Cela indique ODBC version 2. Les valeurs valides sont 2 ou 3. Consultez la spécification ODBC SQLColumns pour connaître les différences de comportement entre les versions 2 et 3.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de la table ou de la vue. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table ou de la vue. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de table ou de vue. Ce champ retourne toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne, pour chaque colonne de la **table_name** retournée. Ce champ retourne toujours une valeur.|  
|**DATA_TYPE**|**smallint**|Valeur entière correspondant à des indicateurs de type ODBC. S'il s'agit d'un type de données qui ne peut pas être mappé avec un type ODBC, cette valeur est NULL. Le nom du type de données natif est retourné dans la colonne **type_name** .|  
|**TYPE_NAME**|**varchar (** 13 **)**|Chaîne représentant un type de données. Le SGBD sous-jacent dispose de ce nom de type de données.|  
|**COLUMN_SIZE**|**int**|Nombre de chiffres significatifs. La valeur de retour de la colonne **PRECISION** est en base 10.|  
|**BUFFER_LENGTH**|**int**|Taille de transfert des données.1|  
|**DECIMAL_DIGITS**|**smallint**|Nombre de chiffres situés à droite du séparateur décimal.|  
|**NUM_PREC_RADIX**|**smallint**|Est la base des types de données numériques.|  
|**NULLABLE**|**smallint**|Spécifie la possibilité de contenir une valeur NULL.<br /><br /> 1 = les valeurs NULL sont autorisées.<br /><br /> 0 = pas de valeur NULL.|  
|**NOTES**|**varchar (** 254 **)**|Ce champ renvoie toujours la valeur NULL.|  
|**COLUMN_DEF**|**varchar (** 254 **)**|Valeur par défaut de la colonne.|  
|**SQL_DATA_TYPE**|**smallint**|Valeur du type de données SQL tel qu'il apparaît dans le champ TYPE du descripteur. Cette colonne est la même que la colonne **data_type** , à l’exception des types de données **DateTime** et SQL-92 **Interval** . Cette colonne renvoie toujours une valeur.|  
|**SQL_DATETIME_SUB**|**smallint**|Code de sous-type pour les types de données **DateTime** et SQL-92 **Interval** . Pour les autres types de données, cette colonne renvoie la valeur NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longueur maximale, en octets, d'une colonne de type de données caractère ou entier. Pour tous les autres types de données, cette colonne renvoie une valeur NULL.|  
|**ORDINAL_POSITION**|**int**|Numéro d'ordre de la colonne dans la table. La première colonne de la table est la colonne 1. Cette colonne renvoie toujours une valeur.|  
|**IS_NULLABLE**|**varchar (** 254 **)**|Possibilité de valeurs NULL dans la colonne de la table. Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO SQL ne peut pas renvoyer de chaîne vide.<br /><br /> YES = la colonne peut inclure des valeurs NULL.<br /><br /> NO = la colonne ne peut pas inclure de valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> La valeur retournée pour cette colonne est différente de la valeur renvoyée pour la colonne **Nullable** .|  
|**SS_DATA_TYPE**|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisé par les procédures stockées étendues.|  
  
 Pour plus d'informations, consultez la documentation de Microsoft ODBC.  
  
## <a name="remarks"></a>Notes  
- **sp_columns_ex** est exécuté en interrogeant l’ensemble de lignes COLUMNS de l’interface **IDBSchemaRowset** du fournisseur OLE DB correspondant à *table_server*. Les paramètres *table_name*, *TABLE_SCHEMA*, *TABLE_CATALOG* et *Column* sont passés à cette interface pour limiter les lignes retournées.  
  
- **sp_columns_ex** retourne un jeu de résultats vide si le fournisseur de OLE DB du serveur lié spécifié ne prend pas en charge l’ensemble de lignes COLUMNS de l’interface **IDBSchemaRowset** .  
  
- **sp_columns_ex** suit la configuration requise pour les identificateurs délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple qui suit retourne le type de données de la colonne `JobTitle` de la table `HumanResources.Employee` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur le serveur lié `Seattle1`.  
  
```  
EXEC sp_columns_ex 'Seattle1',   
   'Employee',   
   'HumanResources',   
   'AdventureWorks2012',   
   'JobTitle';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_foreignkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
