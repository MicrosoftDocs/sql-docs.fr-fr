---
description: sys.fn_listextendedproperty (Transact-SQL)
title: sys.fn_listextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_listextendedproperty
- fn_listextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_listextendedproperty function
- displaying extended properties
- database extended properties [SQL Server]
- viewing extended properties
- column extended properties [SQL Server]
- sys.fn_listextendedproperties function
- database objects [SQL Server], extended properties
- extended properties [SQL Server], columns
- table extended properties [SQL Server]
ms.assetid: 59bbb91f-a277-4a35-803e-dcb91e847a49
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8b60e2fabcccd2040c3dceb1307b7f7e7907bda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201941"
---
# <a name="sysfn_listextendedproperty-transact-sql"></a>sys.fn_listextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne les valeurs de propriétés étendues d'objets de base de données.  
 
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_listextendedproperty (   
    { default | 'property_name' | NULL }   
  , { default | 'level0_object_type' | NULL }   
  , { default | 'level0_object_name' | NULL }   
  , { default | 'level1_object_type' | NULL }   
  , { default | 'level1_object_name' | NULL }   
  , { default | 'level2_object_type' | NULL }   
  , { default | 'level2_object_name' | NULL }   
  )   
```  
  
## <a name="arguments"></a>Arguments  
 {default | '*property_name*' | NUL  
 Nom de la propriété. *property_name* est de **type sysname**. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom de propriété.  
  
 {default | '*level0_object_type*' | NUL  
 Type défini par l'utilisateur ou utilisateur. *level0_object_type* est de type **varchar (128)**, avec NULL comme valeur par défaut. Les entrées valides sont ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, et NULL.  
  
> [!IMPORTANT]  
>  Les types de niveau 0 USER et TYPE seront éliminés dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement. À la place de USER, utilisez SCHEMA en tant que type de niveau 0. Pour TYPE, utilisez SCHEMA comme type de niveau 0 et TYPE comme type de niveau 1.  
  
 {default | '*level0_object_name*' | NUL  
 Nom du type d'objet de niveau 0 spécifié. *level0_object_name* est de **type sysname** , avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
 {default | '*level1_object_type*' | NUL  
 Type d'objet de niveau 1. *level1_object_type* est de type **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TYPE, VIEW, XML SCHEMA COLLECTION et NULL.  
  
> [!NOTE]  
>  La valeur par défaut correspond à la valeur NULL, et la valeur 'default' est mappée sur le type d'objet DEFAULT.  
  
 {default | '*level1_object_name*' | NUL  
 Nom du type d'objet de niveau 1 spécifié. *level1_object_name* est de **type sysname** , avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
 {default | '*level2_object_type*' | NUL  
 Type d'objet de niveau 2. *level2_object_type* est de type **varchar (128)** avec NULL comme valeur par défaut. Les entrées valides sont DEFAULT, la valeur par défaut (qui correspond à la valeur NULL) et NULL. Les entrées valides pour *level2_object_type* sont Column, CONSTRAINT, Event notification, index, Parameter, Trigger et null.  
  
 {default | '*level2_object_name*' | NUL  
 Nom du type d'objet de niveau 2 spécifié. *level2_object_name* est de **type sysname** , avec NULL comme valeur par défaut. Les entrées autorisées sont les valeurs par défaut, NULL, ou un nom d'objet.  
  
## <a name="tables-returned"></a>Tables retournées  
 Le format des tables renvoyées par l'instruction fn_listextendedproperty est le suivant :  
  
|Nom de la colonne|Type de données|  
|-----------------|---------------|  
|objtype|**sysname**|  
|objname|**sysname**|  
|name|**sysname**|  
|value|**sql_variant**|  
  
 Si la table renvoyée est vide, l'objet ne dispose pas de propriétés étendues, ou l'utilisateur n'est pas habilité à afficher la liste des propriétés étendues associées à cet objet. Lorsque les propriétés étendues de la base de données proprement dite sont retournées, les colonnes objtype et objname ont la valeur NULL.  
  
## <a name="remarks"></a>Notes  
 Si la valeur de *property_name* est null ou par défaut, fn_listextendedproperty retourne toutes les propriétés de l’objet spécifié.  
  
 Lorsque le type d'objet est spécifié et que la valeur du nom d'objet correspondant est NULL ou la valeur par défaut, fn_listextendedproperty retourne toutes les propriétés étendues de tous les objets du type spécifié.  
  
 Les objets se distinguent selon des niveaux : le niveau 0 étant le plus élevé et le niveau 2 le moins élevé. Si un type et un nom d'objet de niveau inférieur (niveau 1 ou 2) sont spécifiés, le type et le nom de l'objet parent ne doivent pas comporter la valeur NULL ou la valeur par défaut. Autrement, la fonction renvoie un jeu de résultats vide.  
  
 **nomobj** est fixe en tant que Latin1_General_CI_AI. Toutefois, vous pouvez contourner ce problème en remplaçant le classement en comparaison.  
  
```  
SELECT o.[object_id] AS 'table_id', o.[name] 'table_name',  
0 AS 'column_order', NULL AS 'column_name', NULL AS 'column_datatype',  
NULL AS 'column_length', Cast(e.value AS varchar(500)) AS 'column_description'  
FROM AdventureWorks.sys.objects AS o  
LEFT JOIN sys.fn_listextendedproperty(N'MS_Description', N'user',N'HumanResources',N'table', N'Employee', null, default) AS e  
    ON o.name = e.objname COLLATE SQL_Latin1_General_CP1_CI_AS  
WHERE o.name = 'Employee';  
```  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'affichage des propriétés étendues des objets peuvent varier selon le type d'objet.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>R. Affichage des propriétés étendues d'une base de données  
 L'exemple suivant affiche toutes les propriétés étendues de l'objet de base de données lui-même.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty(default, default, default, default, default, default, default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype    objname     name            value`  
  
 `---------  ---------   -----------     ----------------------------`  
  
 `NULL       NULL        MS_Description  AdventureWorks2008 Sample OLTP Database`  
  
 `(1 row(s) affected)`  
  
### <a name="b-displaying-extended-properties-on-all-columns-in-a-table"></a>B. Affichage des propriétés étendues de toutes les colonnes d'une table  
 L’exemple suivant répertorie les propriétés étendues des colonnes de la `ScrapReason` table. Celle-ci est contenue dans le schéma `Production`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Production', 'table', 'ScrapReason', 'column', default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype objname      name            value`  
  
 `------- -----------  -------------   ------------------------`  
  
 `COLUMN ScrapReasonID MS_Description  Primary key for ScrapReason records.`  
  
 `COLUMN Name          MS_Description  Failure description.`  
  
 `COLUMN ModifiedDate  MS_Description  Date the record was last updated.`  
  
 `(3 row(s) affected)`  
  
### <a name="c-displaying-extended-properties-on-all-tables-in-a-schema"></a>C. Affichage des propriétés étendues de toutes les tables d'un schéma  
 L’exemple suivant répertorie des propriétés étendues pour toutes les tables contenues dans le `Sales` schéma.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Sales', 'table', default, NULL, NULL);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
