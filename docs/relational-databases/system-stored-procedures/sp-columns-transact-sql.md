---
description: MSdbms (Transact-SQL)
title: sp_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32f865969933b07d2021a96b0af39f86d7f47893
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200818"
---
# <a name="sp_columns-transact-sql"></a>MSdbms (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations de colonne pour les objets spécifiés qui peuvent être interrogés dans l'environnement actuel.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ \@table_name = ] object` Nom de l’objet utilisé pour retourner les informations de catalogue. l' *objet* peut être une table, une vue ou un autre objet qui a des colonnes telles que des fonctions table. l' *objet* est de type **nvarchar (384)**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
`[ \@table_owner = ] owner` Propriétaire de l’objet utilisé pour retourner les informations de catalogue. *owner* est de type **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *owner* n’est pas spécifié, les règles de visibilité d’objet par défaut du SGBD sous-jacent s’appliquent.  
  
 Si l'utilisateur actuel est propriétaire d'un objet portant le nom spécifié, les colonnes de cet objet sont retournées. Si *owner* n’est pas spécifié et que l’utilisateur actuel ne possède pas d’objet avec l' *objet* spécifié, **sp_columns** recherche un objet avec l' *objet* spécifié détenu par le propriétaire de la base de données. S'il en existe un, les colonnes de cet objet sont retournées.  
  
`[ \@table_qualifier = ] qualifier` Nom du qualificateur de l’objet. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms d’objets en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de l'objet.  
  
`[ \@column_name = ] column` Est une colonne unique et est utilisée lorsqu’une seule colonne d’informations de catalogue est souhaitée. la *colonne* est de type **nvarchar (384)**, avec NULL comme valeur par défaut. Si la *colonne* n’est pas spécifiée, toutes les colonnes sont retournées. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , *colonne* représente le nom de colonne tel qu’il figure dans la table **syscolumns** . La recherche de correspondance avec des caractères génériques est prise en charge. Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les modèles de comparaison standard de SQL-92 (caractères génériques % et _).  
  
`[ \@ODBCVer = ] ODBCVer` Est la version de ODBC en cours d’utilisation. *ODBCVer* est de **type int**, avec 2 comme valeur par défaut. Cela indique ODBC version 2. Les valeurs valides sont 2 ou 3. Pour connaître les différences de comportement entre les versions 2 et 3, consultez la spécification ODBC **SQLColumns** .  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 La procédure stockée du catalogue **sp_columns** est équivalente à **SQLColumns** dans ODBC. Les résultats retournés sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER** et **table_name**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nom du qualificateur d'objet. Ce champ peut contenir la valeur NULL.|  
|**TABLE_OWNER**|**sysname**|Nom du propriétaire de l'objet. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom d’objet Ce champ retourne toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne, pour chaque colonne de la **table_name** retournée. Ce champ retourne toujours une valeur.|  
|**DATA_TYPE**|**smallint**|Code d’entier pour le type de données ODBC. S’il s’agit d’un type de données qui ne peut pas être mappé à un type ODBC, il a la valeur NULL. Le nom du type de données natif est retourné dans la colonne **type_name** .|  
|**TYPE_NAME**|**sysname**|Chaîne représentant un type de données. Le SGBD sous-jacent dispose de ce nom de type de données.|  
|**PRECISION**|**int**|Nombre de chiffres significatifs. La valeur de retour de la colonne **PRECISION** est en base 10.|  
|**LENGTH**|**int**|Taille de transfert des données. <sup>1</sup>|  
|**ÉCHELLE**|**smallint**|Nombre de chiffres situés à droite du séparateur décimal.|  
|**RADIX**|**smallint**|Base pour les types de données numériques.|  
|**NULLABLE**|**smallint**|Spécifie la possibilité de contenir une valeur NULL.<br /><br /> 1 = les valeurs NULL sont autorisées.<br /><br /> 0 = pas de valeur NULL.|  
|**NOTES**|**varchar (254)**|Ce champ renvoie toujours la valeur NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Valeur par défaut de la colonne.|  
|**SQL_DATA_TYPE**|**smallint**|Valeur du type de données SQL tel qu'il apparaît dans le champ TYPE du descripteur. Cette colonne est la même que la colonne **data_type** , à l’exception des types de données **DateTime** et SQL-92 **Interval** . Cette colonne renvoie toujours une valeur.|  
|**SQL_DATETIME_SUB**|**smallint**|Code de sous-type pour les types de données **DateTime** et SQL-92 **Interval** . Pour les autres types de données, cette colonne renvoie la valeur NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longueur maximale, en octets, d'une colonne de type de données caractère ou entier. Pour tous les autres types de données, cette colonne renvoie une valeur NULL.|  
|**ORDINAL_POSITION**|**int**|Numéro d'ordre de la colonne dans l'objet. La première colonne de l'objet est la colonne 1. Cette colonne renvoie toujours une valeur.|  
|**IS_NULLABLE**|**varchar (254)**|Possibilité de valeur Null dans la colonne de l'objet. Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO SQL ne peut pas renvoyer de chaîne vide.<br /><br /> YES = la colonne peut inclure des valeurs NULL.<br /><br /> NO = la colonne ne peut pas inclure de valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> La valeur retournée pour cette colonne est différente de la valeur renvoyée pour la colonne **Nullable** .|  
|**SS_DATA_TYPE**|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par les procédures stockées étendues. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> pour plus d’informations, consultez la documentation de Microsoft ODBC.  
  
## <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT et VIEW DEFINITION sur le schéma.  
  
## <a name="remarks"></a>Notes  
 **sp_columns** suit la configuration requise pour les identificateurs délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations de colonne pour une table spécifique.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'exemple suivant retourne les informations de colonne pour une table spécifique.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


