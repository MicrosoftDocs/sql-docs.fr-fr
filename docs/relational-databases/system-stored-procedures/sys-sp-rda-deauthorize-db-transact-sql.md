---
title: sys.sp_rda_deauthorize_db (Transact-SQL) | Microsoft Docs
description: Découvrez comment utiliser sys.sp_rda_deauthorize_db pour supprimer les connexions authentifiées entre les bases de données Stretch locales et les bases de données Azure distantes.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_deauthorize_db
- sys.sp_rda_deauthorize_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_deauthorize_db stored procedure
ms.assetid: 2e362e15-2cd5-4856-9f0b-54df56b0866b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9abdf527c434674ccecd655c93c27606b29b940d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186636"
---
# <a name="syssp_rda_deauthorize_db-transact-sql"></a>sys.sp_rda_deauthorize_db (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Supprime la connexion authentifiée entre une base de données Stretch locale et la base de données Azure distante. Exécutez **sp_rda_deauthorize_db**  lorsque la base de données distante est inaccessible ou dans un état incohérent et que vous souhaitez modifier le comportement de la requête pour toutes les tables Stretch dans la base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_rda_deauthorize_db   
```  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou >0 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations db_owner.  
  
## <a name="remarks"></a>Notes  
 Après l’exécution de **sp_rda_deauthorize_db** , toutes les requêtes sur les tables et les bases de données Stretch échouent. Autrement dit, le mode de requête est défini sur désactivé. Pour quitter ce mode, effectuez l’une des opérations suivantes.  
  
-   Exécutez [sys.sp_rda_reauthorize_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) pour vous reconnecter à la base de données Azure distante. Cette opération rétablit automatiquement le mode de requête LOCAL_AND_REMOTE, qui est le comportement par défaut pour Stretch Database. Autrement dit, les requêtes retournent des résultats à partir de données locales et distantes.  
  
-   Exécutez [sys.sp_rda_set_query_mode &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) avec l’argument LOCAL_ONLY pour permettre aux requêtes de continuer à s’exécuter sur des données locales uniquement.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_rda_set_query_mode &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md)   
 [sys.sp_rda_reauthorize_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
