---
description: sp_help_fulltext_tables_cursor (Transact-SQL)
title: sp_help_fulltext_tables_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_tables_cursor
- sp_help_fulltext_tables_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables_cursor
ms.assetid: 155791eb-8832-4596-8487-7fc70dfba5b9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7f5ce76745803b1a10caf74f6870202aa508268
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159793"
---
# <a name="sp_help_fulltext_tables_cursor-transact-sql"></a>sp_help_fulltext_tables_cursor (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Utilise un curseur pour retourner une liste des tables inscrites pour l'indexation de texte intégral.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt le nouvel affichage catalogue **sys.fulltext_indexes** . Pour plus d’informations, consultez [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_tables_cursor [ @cursor_return = ] @cursor_variable OUTPUT   
     [ , [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @cursor_return = ] @cursor_variable OUTPUT` Variable de sortie de type **Cursor**. Le curseur est en lecture seule, dynamique et permet les défilements.  
  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Nom du catalogue de texte intégral. *fulltext_catalog_name* est de **type sysname**, avec NULL comme valeur par défaut. Si *fulltext_catalog_name* est omis ou si a la valeur null, toutes les tables indexées en texte intégral associées à la base de données sont retournées. Si *fulltext_catalog_name* est spécifié, mais que *table_name* est omis ou a la valeur null, les informations de l’index de recherche en texte intégral sont récupérées pour chaque table indexée de texte intégral associée à ce catalogue. Si *fulltext_catalog_name* et *table_name* sont spécifiés, une ligne est retournée si *table_name* est associé à *fulltext_catalog_name*; dans le cas contraire, une erreur est générée.  
  
`[ @table_name = ] 'table_name'` Nom de la table en une ou deux parties pour laquelle les métadonnées de texte intégral sont demandées. *table_name* est de type **nvarchar (517)**, avec NULL comme valeur par défaut. Si seul *table_name* est spécifié, seule la ligne pertinente pour *table_name* est retournée.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propriétaire de la table. Nom de l'utilisateur de la base de données qui a créé la table.|  
|**TABLE_NAME**|**sysname**|Nom de la table.|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|Index imposant la contrainte UNIQUE dans la colonne désignée comme colonne clé unique.|  
|**FULLTEXT_KEY_COLID**|**int**|ID de colonne de l'index unique identifié par FULLTEXT_KEY_NAME.|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|Indique si les colonnes de cette table marquées pour l'indexation de texte intégral peuvent faire l'objet de requêtes :<br /><br /> 0 = Inactif<br /><br /> 1 = Actif|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|Catalogue de texte intégral contenant les données d'index sur le texte intégral.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution reviennent par défaut aux membres du rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne le nom des tables indexées sur le texte intégral associées au catalogue de texte intégral `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_tables_cursor @mycursor OUTPUT, 'Cat_Desc';  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END;  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
