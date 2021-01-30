---
description: sp_indexes (Transact-SQL)
title: sp_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_indexes_TSQL
- sp_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexes
ms.assetid: 25469e72-9d95-463f-912a-193471c8f5e2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f11b9a32c05450dbe97c9130d774e4d92226fcdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198503"
---
# <a name="sp_indexes-transact-sql"></a>sp_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie les informations d'index pour la table distante spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_indexes [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_db' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_server =] '*table_server*'  
 Nom du serveur lié exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour lequel les informations de table sont demandées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
 [ @table_name =] '*table_name*'  
 Nom de la table distante pour laquelle les informations d'index sont demandées. *table_name* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur de cet argument est NULL, toutes les tables de la base de données spécifiée sont retournées.  
  
 [ @table_schema =] '*TABLE_SCHEMA*'  
 Spécifie le schéma de la table. Dans l'environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ceci correspond au propriétaire de la table. *TABLE_SCHEMA* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @table_catalog =] '*table_db*'  
 Nom de la base de données dans laquelle *table_name* réside. *table_db* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, *table_db* est défini par défaut sur **Master**.  
  
 [ @index_name =] '*index_name*'  
 Nom de l'index pour lequel les informations sont demandées. *index* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @is_unique =] '*is_unique*'  
 Type d'index pour lequel les informations sont demandées. *is_unique* est de type **bit**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|1|Renvoie des informations sur les index uniques.|  
|0|Renvoie des informations sur les index qui ne sont pas uniques.|  
|NULL|Renvoie des informations sur tous les index.|  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|TABLE_CAT|**sysname**|Nom de la base de données qui contient la table spécifiée.|  
|TABLE_SCHEM|**sysname**|Schéma pour la table.|  
|TABLE_NAME|**sysname**|Nom de la table distante.|  
|NON_UNIQUE|**smallint**|Indique si l'index est unique ou non :<br /><br /> 0 = Unique<br /><br /> 1 = Non unique|  
|INDEX_QUALIFIER|**sysname**|Nom du propriétaire de l'index. Certains produits SGBD acceptent que des utilisateurs autres que le propriétaire de la table créent des index. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne est toujours identique à **table_name**.|  
|INDEX_NAME|**sysname**|Nom de l'index.|  
|TYPE|**smallint**|Type de l'index :<br /><br /> 0 = Statistiques pour une table<br /><br /> 1 = Clustered<br /><br /> 2 = Haché<br /><br /> 3 = autre|  
|ORDINAL_POSITION|**int**|Numéro d'ordre de la colonne dans l'index. La première colonne dans l'index est 1. Cette colonne renvoie toujours une valeur.|  
|COLUMN_NAME|**sysname**|Nom correspondant de chaque colonne de la table TABLE_NAME renvoyée.|  
|ASC_OR_DESC|**varchar**|Ordre utilisé dans les classements :<br /><br /> A = Croissant<br /><br /> D = Décroissant<br /><br /> NULL = Non applicable<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne toujours A.|  
|CARDINALITY|**int**|Nombre de lignes dans la table ou de valeurs uniques dans l’index.|  
|PAGES|**int**|Nombre de pages pour le stockage de l'index ou de la table.|  
|FILTER_CONDITION|**nvarchar (** 4000 **)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant renvoie toutes les informations d'index à partir de la table `Employees` de la base de données `AdventureWorks2012` sur le serveur lié `Seattle1`.  
  
```  
EXEC sp_indexes @table_server = 'Seattle1',   
   @table_name = 'Employee',   
   @table_schema = 'HumanResources',  
   @table_catalog = 'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de requêtes distribuées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_tables_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
