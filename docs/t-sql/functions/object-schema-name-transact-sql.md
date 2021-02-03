---
description: OBJECT_SCHEMA_NAME (Transact-SQL)
title: OBJECT_SCHEMA_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_SCHEMA_NAME
- OBJECT_SCHEMA_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], names
- schemas [SQL Server], names
- displaying schema names
- database objects [SQL Server], names
- OBJECT_SCHEMA_NAME function
ms.assetid: 5ba90bb9-d045-4164-963e-e9e96c0b1e8b
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 9a92347498da6d200b5ef179c8c507e2e77ba683
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189854"
---
# <a name="object_schema_name-transact-sql"></a>OBJECT_SCHEMA_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Retourne le nom de schéma de base de données des objets de portée de schéma. Pour obtenir la liste de tous les objets délimités aux schémas, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
OBJECT_SCHEMA_NAME ( object_id [, database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *object_id*  
 ID de l'objet à utiliser. *object_id* est de type **int** et est supposé être un objet étendu aux schémas dans le contexte de la base de données spécifiée ou de la base de données active.  
  
 *database_id*  
 ID de la base de données où l'objet est à rechercher. *database_id* est de type **int**.  
  
## <a name="return-types"></a>Types de retour  
 **sysname**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet. Si l'option AUTO_CLOSE de la base de données cible a pour valeur ON, la fonction ouvre la base de données.  
  
 Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, telles que OBJECT_SCHEMA_NAME, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ANY sur l'objet. Pour spécifier un ID de base de données, l'autorisation CONNECT à la base de données est également nécessaire ou le compte Invité doit être activé.  
  
## <a name="remarks"></a>Notes  
 Les fonctions système peuvent être utilisées dans la liste de sélection, dans une clause WHERE, et partout où une expression est autorisée. Pour plus d’informations, consultez [Expressions](../../t-sql/language-elements/expressions-transact-sql.md) et [WHERE](../../t-sql/queries/where-transact-sql.md).  
  
 Le jeu de résultats retourné par cette fonction système utilise le classement de la base de données active.  
  
 Si *database_id* n’est pas spécifié, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] considère que *object_id* se trouve dans le contexte de la base de données active. Une requête référençant un *object_id* dans une autre base de données renvoie la valeur NULL ou des résultats incorrects. Par exemple, dans la requête suivante, le contexte de la base de données active est [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] essaie de retourner un nom d'objet pour l'ID d'objet spécifié, à partir de cette base de données et non de la base de données indiquée dans la clause FROM de la requête. Par conséquent, des informations incorrectes sont retournées.  
  
```sql
SELECT DISTINCT OBJECT_SCHEMA_NAME(object_id)  
FROM master.sys.objects;  
```  
  
 L'exemple suivant spécifie l'ID de base de données pour la base de données `master` dans la fonction `OBJECT_SCHEMA_NAME` et retourne les résultats corrects.  
  
```sql
SELECT DISTINCT OBJECT_SCHEMA_NAME(object_id, 1) AS schema_name  
FROM master.sys.objects;   
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-object-schema-name-and-object-name"></a>R. Retour du nom de schéma d'objet et du nom d'objet  
 L'exemple suivant retourne le nom de schéma d'objet, le nom d'objet et le texte SQL pour tous les plans de requête mis en cache qui ne sont pas des instructions ad hoc ou préparées.  
  
```sql
SELECT DB_NAME(st.dbid) AS database_name,   
    OBJECT_SCHEMA_NAME(st.objectid, st.dbid) AS schema_name,  
    OBJECT_NAME(st.objectid, st.dbid) AS object_name,   
    st.text AS query_statement  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
WHERE st.objectid IS NOT NULL;  
GO  
```  
  
### <a name="b-returning-three-part-object-names"></a>B. Retour de noms d'objet en trois parties  
 L'exemple suivant retourne le nom d'objet, de schéma et de base de données ainsi que toutes les autres colonnes dans la vue de gestion dynamique `sys.dm_db_index_operational_stats` pour tous les objets de l'ensemble des bases de données.  
  
```sql
SELECT QUOTENAME(DB_NAME(database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_SCHEMA_NAME(object_id, database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_NAME(object_id, database_id))  
    , *   
FROM sys.dm_db_index_operational_stats(null, null, null, null);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [Éléments sécurisables](../../relational-databases/security/securables.md)
