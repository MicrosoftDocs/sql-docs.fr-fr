---
description: sp_statistics (Transact-SQL)
title: sp_statistics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_statistics_TSQL
- sp_statistics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_statistics
ms.assetid: 0bb6495f-258a-47ec-9f74-fd16671d23b8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1132a44770b70cae1906bc4ed9a45366616c08f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206474"
---
# <a name="sp_statistics-transact-sql"></a>sp_statistics (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la liste de tous les index et statistiques d'une table ou d'une vue indexée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_statistics [ @table_name = ] 'table_name'    
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
     [ , [ @accuracy = ] 'accuracy' ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments  
`[ @table_name = ] 'table_name'` Spécifie la table utilisée pour retourner les informations de catalogue. *table_name* est de **type sysname**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge.  
  
`[ @table_owner = ] 'owner'` Nom du propriétaire de la table utilisée pour retourner les informations de catalogue. *TABLE_OWNER* est de **type sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *owner* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Si, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les index de la table sont retournés. Si *owner* n’est pas spécifié et que l’utilisateur actuel ne possède pas de table portant le *nom* spécifié, cette procédure recherche une table portant le *nom* spécifié, détenue par le propriétaire de la base de données. Si cette table existe, ses index sont retournés.  
  
`[ @table_qualifier = ] 'qualifier'` Nom du qualificateur de la table. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce paramètre représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
`[ @index_name = ] 'index_name'` Nom de l’index. *index_name* est de **type sysname**, avec% comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
`[ @is_unique = ] 'is_unique'` Indique si seuls les index uniques (si **Y**) doivent être retournés. *is_unique* est de **type char (1)**, avec **N** comme valeur par défaut.  
  
`[ @accuracy = ] 'accuracy'` Niveau de cardinalité et de précision de la page pour les statistiques. la *précision* est **char (1)**, avec **Q** comme valeur par défaut. Spécifiez **E** pour vous assurer que les statistiques sont mises à jour afin que la cardinalité et les pages soient exactes.  
  
 La valeur **E** (SQL_ENSURE) demande au pilote de récupérer de manière inconditionnelle les statistiques.  
  
 La valeur **Q** (SQL_QUICK) demande au pilote de récupérer la cardinalité et les pages uniquement si elles sont immédiatement disponibles sur le serveur. Dans ce cas, le pilote ne s'assure pas que les valeurs sont actuelles. Pour les applications écrites conformément au standard de groupe ouvert, seule l'option SQL_QUICK est disponible de la part des pilotes compatibles ODBC 3.x.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur de la table. Cette colonne peut être NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de la table. Cette colonne renvoie toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de la table. Cette colonne renvoie toujours une valeur.|  
|**NON_UNIQUE**|**smallint**|NON NULL.<br /><br /> 0 = Unique<br /><br /> 1 = Non unique|  
|**INDEX_QUALIFIER**|**sysname**|Nom du propriétaire de l’index. Certains produits SGBD acceptent que des utilisateurs autres que le propriétaire de la table créent des index. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne est toujours identique à **table_name**.|  
|**INDEX_NAME**|**sysname**|Nom de l'index. Cette colonne renvoie toujours une valeur.|  
|**TYPE**|**smallint**|Cette colonne renvoie toujours une valeur :<br /><br /> 0 = Statistiques pour une table<br /><br /> 1 = Clustered<br /><br /> 2 = Haché<br /><br /> 3 = non cluster|  
|**SEQ_IN_INDEX**|**smallint**|Position de la colonne dans l'index|  
|**COLUMN_NAME**|**sysname**|Nom de colonne de chaque colonne de la **table_name** retournée. Cette colonne renvoie toujours une valeur.|  
|**COLLATION**|**char(1)**|Ordre utilisé dans les classements. Valeurs possibles :<br /><br /> A = Croissant<br /><br /> D = Décroissant<br /><br /> NULL = Non applicable|  
|**CARDINALITÉ**|**int**|Nombre de lignes dans la table ou valeurs uniques dans l’index.|  
|**PAGES**|**int**|Nombre de pages pour stocker l'index ou la table.|  
|**FILTER_CONDITION**|**varchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur.|  
  
## <a name="return-code-values"></a>Codet de retour  
 None  
  
## <a name="remarks"></a>Notes  
 Les index du jeu de résultats s’affichent par ordre croissant selon les colonnes **NON_UNIQUE**, **type**, **index_name** et **SEQ_IN_INDEX**.  
  
 Le type d'index cluster fait référence à un index dans lequel les données de la table sont stockées dans l'ordre de l'index. Cela correspond aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] index cluster.  
  
 Le type d'index haché accepte les recherches de concordance exacte ou d'intervalle, mais les recherches par critères spéciaux n'utilisent pas l'index.  
  
 **sp_statistics** est équivalent à **SQLStatistics** dans ODBC. Les résultats retournés sont triés par **NON_UNIQUE**, **type**, **INDEX_QUALIFIER**, **index_name** et **SEQ_IN_INDEX**. Pour plus d’informations, consultez [référence de l’API ODBC](../../odbc/reference/syntax/odbc-reference.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="example-sssdwfull-and-sspdw"></a>Exemple : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant retourne des informations sur la `DimEmployee` table.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_statistics DimEmployee;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
