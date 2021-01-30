---
description: sp_sproc_columns (Transact-SQL)
title: sp_sproc_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c6d7113657e036b0116e212c17df8ed4479cc1cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189316"
---
# <a name="sp_sproc_columns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Fournit des informations de colonne pour une fonction définie par l'utilisateur ou une procédure stockée unique dans l'environnement courant.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @procedure_name = ] 'name'` Nom de la procédure utilisée pour retourner les informations de catalogue. *Name* est de type **nvarchar (** 390 **)**, avec% comme valeur par défaut, qui correspond à toutes les tables de la base de données active. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
`[ @procedure_owner = ] 'owner'` Nom du propriétaire de la procédure. *owner* est de type **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *owner* n’est pas spécifié, les règles de visibilité des procédures par défaut du SGBD sous-jacent s’appliquent.  
  
 Si l'utilisateur actuel possède une procédure du nom spécifié, les informations relatives à cette procédure sont retournées. Si *owner* n’est pas spécifié et que l’utilisateur actuel ne possède pas de procédure portant le nom spécifié, **sp_sproc_columns** recherche une procédure portant le nom spécifié qui appartient au propriétaire de la base de données. Si la procédure existe, les informations relatives à ses colonnes sont renvoyées.  
  
`[ @procedure_qualifier = ] 'qualifier'` Nom du qualificateur de la procédure. *qualifier* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de table en trois parties (*qualifier.Owner.Name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce paramètre représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
`[ @column_name = ] 'column_name'` Est une colonne unique et est utilisée lorsqu’une seule colonne d’informations de catalogue est souhaitée. *column_name* est de type **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. Si *column_name* est omis, toutes les colonnes sont retournées. La recherche de correspondance avec des caractères génériques est prise en charge. Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les modèles de comparaison standard ISO (caractères génériques % et _).  
  
`[ @ODBCVer = ] 'ODBCVer'` Version d’ODBC utilisée. *ODBCVer* est de **type int**, avec 2 comme valeur par défaut, qui indique ODBC version 2,0. Pour plus d’informations sur la différence entre ODBC version 2,0 et ODBC version 3,0, reportez-vous à la spécification ODBC **SQLProcedureColumns** pour odbc version 3,0  
  
`[ @fUsePattern = ] 'fUsePattern'` Détermine si les caractères de soulignement (_), de pourcentage (%) et de crochets ([]) sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nom du qualificateur de procédure. Cette colonne peut être NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nom du propriétaire de la procédure. Cette colonne renvoie toujours une valeur.|  
|**PROCEDURE_NAME**|**nvarchar (** 134 **)**|Nom de la procédure. Cette colonne renvoie toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne de chaque colonne de la **table_name** retournée. Cette colonne renvoie toujours une valeur.|  
|**COLUMN_TYPE**|**smallint**|Ce champ retourne toujours une valeur :<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|Code entier d'un type de données ODBC. Si ce type de données ne peut pas être mappé à un type ISO, la valeur est NULL. Le nom du type de données natif est retourné dans la colonne **type_name** .|  
|**TYPE_NAME**|**sysname**|Représentation sous forme de chaîne du type de données. Nom du type de données, tel qu'il existe dans le SGBD sous-jacent.|  
|**PRECISION**|**int**|Nombre de chiffres significatifs. La valeur de retour de la colonne **PRECISION** est en base 10.|  
|**LENGTH**|**int**|Taille de transfert des données.|  
|**ÉCHELLE**|**smallint**|Nombre de chiffres situés à droite du séparateur décimal.|  
|**RADIX**|**smallint**|Base des types numériques.|  
|**NULLABLE**|**smallint**|Précise la possibilité de valeur nulle.<br /><br /> 1 = Le type de données peut être créé en autorisant des valeurs NULL.<br /><br /> 0 = les valeurs NULL ne sont pas autorisées.|  
|**NOTES**|**varchar (** 254 **)**|Description de la colonne de procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|Valeur par défaut de la colonne.|  
|**SQL_DATA_TYPE**|**smallint**|Valeur du type de données SQL tel qu’il apparaît dans le champ **type** du descripteur. Cette colonne est la même que la colonne **DATA_TYPE**, excepté pour les types de données **datetime** et **interval** ISO. Cette colonne renvoie toujours une valeur.|  
|**SQL_DATETIME_SUB**|**smallint**|Le sous-code **interval** ISO de **datetime** si la valeur de **SQL_DATA_TYPE** est **SQL_DATETIME** ou **SQL_INTERVAL**. Pour les types de données autres que **DateTime** et **Interval** ISO, ce champ a la valeur null.|  
|**CHAR_OCTET_LENGTH**|**int**|Longueur maximale en octets d’une colonne de type de données **binaire** ou **caractère** . Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|**ORDINAL_POSITION**|**int**|Numéro d'ordre de la colonne dans la table. La première colonne de la table est la colonne 1. Cette colonne renvoie toujours une valeur.|  
|**IS_NULLABLE**|**varchar (254)**|Possibilité de valeurs NULL dans la colonne de la table. Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO ne peut pas renvoyer de chaîne vide.<br /><br /> Affiche YES si la colonne peut comprendre des valeurs NULL et NO dans le cas contraire.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> La valeur renvoyée pour cette colonne est différente de celle renvoyée pour la colonne NULLABLE.|  
|**SS_DATA_TYPE**|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par les procédures stockées étendues. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
## <a name="remarks"></a>Notes  
 **sp_sproc_columns** est équivalent à **SQLProcedureColumns** dans ODBC. Les résultats retournés sont triés par **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, **PROCEDURE_NAME** et l’ordre dans lequel les paramètres apparaissent dans la définition de la procédure.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
