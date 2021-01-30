---
description: sp_help_fulltext_columns (Transact-SQL)
title: sp_help_fulltext_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_columns
- sp_help_fulltext_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_columns
ms.assetid: 92c8656b-f7fd-4904-9796-acc9ffed4106
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9918652699b3d7bcf6d4ec92dadb749604634641
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198306"
---
# <a name="sp_help_fulltext_columns-transact-sql"></a>sp_help_fulltext_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie les colonnes désignées pour l'indexation de texte intégral.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt l’affichage catalogue [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_columns [ [ @table_name = ] 'table_name' ] ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_name = ] 'table\_name'` Nom de la table en une ou deux parties pour laquelle les informations d’index de recherche en texte intégral sont demandées. *table_name* est de type **nvarchar (517)**, avec NULL comme valeur par défaut. Si *table_name* est omis, les informations sur les colonnes d’index de recherche en texte intégral sont récupérées pour chaque table indexée de texte intégral.  
  
`[ @column_name = ] 'column\_name'` Nom de la colonne pour laquelle les métadonnées d’index de recherche en texte intégral sont demandées. *column_name* est de **type sysname**, avec NULL comme valeur par défaut. Si *column_name* est omis ou si a la valeur null, des informations sur les colonnes de texte intégral sont retournées pour chaque colonne indexée de texte intégral pour *table_name*. Si *table_name* est également omis ou a la valeur null, des informations sur les colonnes d’index de recherche en texte intégral sont retournées pour chaque colonne indexée de texte intégral pour toutes les tables de la base de données.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propriétaire de la table. Nom de l'utilisateur de la base de données qui a créé la table.|  
|**TABLE_ID**|**int**|ID de la table.|  
|**TABLE_NAME**|**sysname**|Nom de la table.|  
|**FULLTEXT_COLUMN_NAME**|**sysname**|Colonne d'une table indexée de texte intégral désignée pour l'indexation.|  
|**FULLTEXT_COLID**|**int**|ID de la colonne indexées sur le texte intégral.|  
|**FULLTEXT_BLOBTP_COLNAME**|**sysname**|Colonne d'une table indexée sur le texte intégral qui désigne le type de document de la colonne indexée sur le texte intégral. Cette valeur s’applique uniquement lorsque la colonne indexée de texte intégral est une colonne **varbinary (max)** ou **image** .|  
|**FULLTEXT_BLOBTP_COLID**|**int**|Identification de la colonne du type de document. Cette valeur s’applique uniquement lorsque la colonne indexée de texte intégral est une colonne **varbinary (max)** ou **image** .|  
|**FULLTEXT_LANGUAGE**|**sysname**|Langue utilisée pour la recherche en texte intégral sur la colonne.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution reviennent par défaut aux membres du rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne des informations sur les colonnes qui ont été désignées pour l'indexation de texte intégral dans la table `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_columns 'Production.Document';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [sp_fulltext_column &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)   
 [sp_help_fulltext_columns_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
