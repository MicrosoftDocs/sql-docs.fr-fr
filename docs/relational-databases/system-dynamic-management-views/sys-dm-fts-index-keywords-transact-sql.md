---
description: sys.dm_fts_index_keywords (Transact-SQL)
title: sys.dm_fts_index_keywords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords
- sys.dm_fts_index_keywords
- sys.dm_fts_index_keywords_TSQL
- dm_fts_index_keywords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords dynamic management function
- full-text search [SQL Server], viewing keywords
- troubleshooting [SQL Server], full-text search
ms.assetid: fce7b2a1-7e74-4769-86a8-c77c7628decd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 55bd43c5d4704d145618401230be62e59f3cd3c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211209"
---
# <a name="sysdm_fts_index_keywords-transact-sql"></a>sys.dm_fts_index_keywords (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur le contenu d'un index de recherche en texte intégral pour la table spécifiée.  
  
 **sys.dm_fts_index_keywords** est une fonction de gestion dynamique.  
  
> [!NOTE]  
>  Pour afficher les informations d’index de recherche en texte intégral de niveau inférieur, utilisez la fonction de gestion dynamique [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md) au niveau du document.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_fts_index_keywords( DB_ID('database_name'), OBJECT_ID('table_name') )  
```  
  
## <a name="arguments"></a>Arguments  
 db_id («*database_name*»)  
 Appel à la fonction [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Cette fonction accepte un nom de base de données et retourne l’ID de base de données, que **sys.dm_fts_index_keywords** utilise pour rechercher la base de données spécifiée. Si *database_name* est omis, la fonction retourne l’ID de la base de données active.  
  
 object_id («*table_name*»)  
 Appel à la fonction [OBJECT_ID ()](../../t-sql/functions/object-id-transact-sql.md) . Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**keyword**|**nvarchar(4000)**|Représentation hexadécimale du mot clé stocké dans l’index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou d’un jeu de données.|  
|**display_term**|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format hexadécimal.<br /><br /> Remarque : la valeur de **display_term** pour OxFF est « fin de fichier ».|  
|**column_id**|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|**document_count**|**int**|Nombre de documents ou de lignes contenant le terme actuel.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_fts_index_keywords** sont utiles pour la recherche des éléments suivants :  
  
-   Si un mot clé fait partie de l'index de recherche en texte intégral.  
  
-   Le nombre de documents ou de lignes qui contiennent un mot clé donné.  
  
-   Le mot clé le plus courant dans l'index de recherche en texte intégral :  
  
    -   **document_count** de chaque *keyword_value* comparé au total **document_count**, le nombre de documents de 0xFF.  
  
    -   En règle générale, les mots clés courants peuvent être déclarés en tant que mots vides.  
  
> [!NOTE]  
>  Le **document_count** retourné par **sys.dm_fts_index_keywords** peut être moins précis pour un document spécifique que le nombre retourné par **sys.dm_fts_index_keywords_by_document** ou une requête **Contains** . Cette imprécision éventuelle est estimée inférieure à 1 %. Ce problème peut se produire si un **document_id** peut être compté deux fois lorsqu’il continue sur plusieurs lignes dans le fragment d’index, ou lorsqu’il apparaît plusieurs fois dans la même ligne. Pour obtenir un nombre plus précis pour un document spécifique, utilisez **sys.dm_fts_index_keywords_by_document** ou une requête **Contains** .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-high-level-full-text-index-content"></a>R. Affichage du contenu de l'index de recherche en texte intégral au niveau supérieur  
 L'exemple suivant affiche des informations sur le contenu du niveau supérieur de l'index de recherche en texte intégral de la table `HumanResources.JobCandidate`.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords(db_id('AdventureWorks2012'), object_id('HumanResources.JobCandidate'))  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
