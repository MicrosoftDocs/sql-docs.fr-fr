---
title: DBCC DROPRESULTSETCACHE (Transact-SQL)
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 35adb2c120eae08cde529b09f8f15f5d45fc29d0
ms.sourcegitcommit: edba1c570d4d8832502135bef093aac07e156c95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86484590"
---
# <a name="dbcc-dropresultsetcache--transact-sql"></a>DBCC DROPRESULTSETCACHE  (Transact-SQL)

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]

Supprime toutes les entrées de cache de jeu de résultats à partir d’une base de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Azure.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DBCC DROPRESULTSETCACHE
[;]  
```  

## <a name="permissions"></a>Autorisations

Nécessite l'appartenance au rôle serveur fixe DB_OWNER.

## <a name="remarks"></a>Notes

- Cette commande vide le cache de du jeu de résultats pour toutes les requêtes.  

- La désactivation de la fonctionnalité du cache du jeu de résultats pour une base données entraîne également la suppression de tous les résultats mis en cache.  

- La suspension d’une base de données activée avec la mise en cache du jeu de résultats ne supprime pas les résultats mis en cache.  

## <a name="see-also"></a>Voir aussi

- [Optimisation des performances avec la mise en cache des jeux de résultats](/azure/sql-data-warehouse/performance-tuning-result-set-caching)</br>
- [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azure-sqldw-latest)</br>
- [ALTER DATABASE &#40;Transact-SQL&#41;](/sql/t-sql/statements/alter-database-transact-sql?view=azure-sqldw-latest)</br>
- [SET RESULT SET CACHING &#40;Transact-SQL&#41;](/sql/t-sql/statements/set-result-set-caching-transact-sql)</br>
- [DBCC SHOWRESULTCACHESPACEUSED &#40;Transact-SQL&#41;](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql)
