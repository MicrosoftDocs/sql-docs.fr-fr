---
title: Procédures stockées étendues (Transact-SQL)
description: En savoir plus sur les procédures stockées étendues que vous pouvez utiliser lorsque vous travaillez avec des bases de données Stretch. Consultez Comment rapprocher des colonnes et effectuer d’autres tâches.
titleSuffix: SQL Server Stretch Database
ms.custom: seo-dt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Stretch Database, stored procedures
ms.assetid: bda29952-4b8b-4295-ab78-f24dcb0b03c6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 556e74812ca328a42f1332f20b2576b4cc1b9359
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178069"
---
# <a name="stretch-database-extended-stored-procedures-transact-sql"></a>Stretch Database des procédures stockées étendues (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

 Cette section décrit les procédures stockées étendues qui sont liées à Stretch Database.  
  
## <a name="in-this-section"></a>Dans cette section  
[sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) Supprime la connexion authentifiée entre une base de données Stretch locale et la base de données Azure distante.

[sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md) Obtient le nombre d’heures de données migrées que SQL Server conserve dans une table de mise en lots pour garantir une restauration complète de la base de données Azure distante, si une restauration est nécessaire.
  
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) Restaure la connexion authentifiée entre une base de données locale activée pour Stretch et la base de données distante.
  
 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)  
 Rapproche l’ID de lot stocké dans la table de SQL Server avec Stretch pour les données migrées les plus récentes avec l’ID de lot stocké dans la table Azure distante. 
 
[sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md) Rapproche les colonnes de la table Azure distante des colonnes de la table de SQL Server avec Stretch.
 
 [sys.sp_rda_reconcile_indexes](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-indexes-transact-sql.md) Met en file d’attente une tâche de schéma pour rapprocher des index sur la table distante.
 
 [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) Spécifie si les requêtes sur la base de données Stretch active et ses tables retournent à la fois les données locales et distantes (par défaut), ou les données locales uniquement.
 
 [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) Définit le nombre d’heures de données migrées que SQL Server conserve dans une table de mise en lots pour garantir une restauration complète de la base de données Azure distante, si une restauration est nécessaire.
 
 [sys.sp_rda_test_connection](../../relational-databases/system-stored-procedures/sys-sp-rda-test-connection-transact-sql.md) Teste la connexion de SQL Server au serveur Azure distant et signale les problèmes susceptibles d’empêcher la migration des données.
 
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
