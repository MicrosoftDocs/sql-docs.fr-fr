---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c44ef469dfa60e7c4ac96386ab87812fab447555
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210290"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque objet qui est un module défini en langage SQL dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , y compris la fonction scalaire définie par l’utilisateur compilée en mode natif. Les objets de type P, RF, V, TR, FN, IF, TF et R possèdent un module SQL associé. Les valeurs par défaut autonomes, les objets de type D, possèdent également une définition de module SQL dans cette vue. Pour obtenir une description de ces types, consultez la colonne **type** de l’affichage catalogue [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
 Pour plus d’informations, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID d'objet de l'objet conteneur. Unique dans une base de données.|  
|**définition**|**nvarchar(max)**|Texte SQL qui définit ce module. Cette valeur peut également être obtenue à l’aide de la fonction intégrée [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .<br /><br /> NULL = chiffré|  
|**uses_ansi_nulls**|**bit**|Le module a été créé avec SET ANSI_NULLS ON.<br /><br /> Sera toujours = 0 pour les règles et les valeurs par défaut.|  
|**uses_quoted_identifier**|**bit**|Le module a été créé avec SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|Module créé avec l'option SCHEMABINDING.<br /><br /> Contient toujours la valeur 1 pour les procédures stockées compilées en mode natif.|  
|**uses_database_collation**|**bit**|1 = La définition d'un module lié au schéma dépend du classement par défaut de la base de données pour une évaluation correcte ; dans tous les autres cas, 0. Une telle dépendance empêche la modification du classement par défaut de la base de données.|  
|**is_recompiled**|**bit**|Procédure créée avec l'option WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Le module a été déclaré pour produire une sortie NULL sur n'importe quelle entrée NULL.|  
|**execute_as_principal_id**|**Int**|ID du principal de base de données EXECUTE AS.<br /><br /> Valeur NULL par défaut ou dans le cas de l'instruction EXECUTE AS CALLER.<br /><br /> ID du principal spécifié si EXECUTe AS SELF ou EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|**bit**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].<br /><br /> 0 = Non compilé en mode natif<br /><br /> 1 = Compilé en mode natif<br /><br /> La valeur par défaut est 0.|  
|**is_inlineable**|**bit**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] et versions ultérieures.<br/><br />Indique si le module est inlineable ou non. L’inlineabilité est basée sur les conditions spécifiées [ici](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements).<br /><br /> 0 = non inlineable<br /><br /> 1 = est inlineable. <br /><br /> Pour les fonctions définies par l’utilisateur scalaire, la valeur sera égale à 1 si la fonction définie par l’utilisateur est Inline, et 0 dans le cas contraire. Elle contient toujours la valeur 1 pour Inline TVF, et 0 pour tous les autres types de modules.<br />|  
|**inline_type**|**bit**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] et versions ultérieures.<br /><br />Indique si l’incorporation est activée pour le module actuellement. <br /><br />0 = l’incorporation est désactivée<br /><br /> 1 = l’incorporation est activée.<br /><br /> Pour les fonctions définies par l’utilisateur scalaire, la valeur est 1 si l’incorporation est activée (explicitement ou implicitement). La valeur sera toujours 1 pour Inline TVF, et 0 pour les autres types de modules.<br />|  

  
## <a name="remarks"></a>Notes  
 L’expression SQL d’une contrainte par défaut, objet de type D, se trouve dans l’affichage catalogue [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) . L’expression SQL pour une contrainte CHECK, un objet de type C, se trouve dans l’affichage catalogue [sys.CHECK_CONSTRAINTS](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) .  
  
 Ces informations sont également décrites dans [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne le nom, le type et la définition de chaque module dans la base de données actuelle.  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
