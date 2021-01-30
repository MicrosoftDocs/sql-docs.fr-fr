---
description: sp_depends (Transact-SQL)
title: sp_depends (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_depends
- sp_depends_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_depends
ms.assetid: d9934590-c6ae-4936-91c3-146055ef2c57
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6251c556475165504075ae60a84b6d4f28766aef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206030"
---
# <a name="sp_depends-transact-sql"></a>sp_depends (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations sur les dépendances des objets de base de données, par exemple les vues et procédures qui dépendent d'une table ou vue, et les tables et vues dont dépend la vue ou procédure. Les références à des objets qui se situent en dehors de la base de données active ne sont pas signalées.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md) et [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_depends [ @objname = ] '<object>'   
  
<object> ::=  
{  
    [ database_name. [ schema_name ] . | schema_name.  
    object_name  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel appartient l'objet.  
  
 *object_name*  
 Objet de base de données dont les dépendances doivent être analysées. L'objet peut être une table, une vue, une procédure stockée, une fonction définie par l'utilisateur ou un déclencheur. o *bject_name* est de type **nvarchar (776)**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_depends** affiche deux jeux de résultats.  
  
 Le jeu de résultats suivant affiche les objets dont *\<object>* dépend.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar (257** **)**|Nom de l'élément pour lequel il existe une dépendance.|  
|**type**|**nvarchar (16)**|Type de l’élément.|  
|**mettre**|**nvarchar (7)**|Indique si l'élément est mis à jour.|  
|**sélectionné**|**nvarchar(8)**|Indique si l'objet est utilisé dans une instruction SELECT.|  
|**column**|**sysname**|Colonne ou paramètre sur lequel repose la dépendance.|  
  
 Le jeu de résultats suivant affiche les objets qui dépendent de *\<object>* .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar (257** **)**|Nom de l'élément pour lequel il existe une dépendance.|  
|**type**|**nvarchar (16)**|Type de l’élément.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-dependencies-on-a-table"></a>R. Établissement de la liste des dépendances d'une table  
 L'exemple suivant établit la liste des objets de base de données qui dépendent de la table `Sales.Customer` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Le nom de schéma et le nom de la table sont tous deux spécifiés.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_depends @objname = N'Sales.Customer' ;  
```  
  
### <a name="b-listing-dependencies-on-a-trigger"></a>B. Établissement des dépendances d'un déclencheur.  
 L'exemple suivant établit la liste des objets de base de données dont dépend le déclencheur `iWorkOrder`.  
  
```  
EXEC sp_depends @objname = N'AdventureWorks2012.Production.iWorkOrder' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)  
  
  
