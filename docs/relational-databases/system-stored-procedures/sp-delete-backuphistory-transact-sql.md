---
description: sp_delete_backuphistory (Transact-SQL)
title: sp_delete_backuphistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_backuphistory
- sp_delete_backuphistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_backuphistory
ms.assetid: bdb56834-616e-47e4-b942-e895d2325e97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac5426940ed3fd2a94c055968a22ec1000e4d564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195552"
---
# <a name="sp_delete_backuphistory-transact-sql"></a>sp_delete_backuphistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Réduit la taille des tables d'historique de sauvegarde et de restauration en supprimant les entrées correspondant aux jeux de sauvegarde antérieurs à la date spécifiée. Des lignes supplémentaires sont ajoutées aux tables d’historique de sauvegarde et de restauration après chaque opération de sauvegarde ou de restauration. par conséquent, nous vous recommandons d’exécuter régulièrement **sp_delete_backuphistory**.  
  
> [!NOTE]  
>  Les tables d’historique de sauvegarde et de restauration résident dans la base de données **msdb** .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_backuphistory [ @oldest_date = ] 'oldest_date'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @oldest_date = ] 'oldest\_date'` Date la plus ancienne conservée dans les tables d’historique de sauvegarde et de restauration. *oldest_date* est de **type DateTime**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_delete_backuphistory** doit être exécutée à partir de la base de données **msdb** et affecte les tables suivantes :  
  
-   [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
  
-   [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
  
-   [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
  
-   [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
  
-   [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
-   [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
  
-   [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
  
-   [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
  
 Les fichiers de sauvegarde physiques sont conservés, même si tout l'historique est supprimé.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** , mais les autorisations peuvent être accordées à d’autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime toutes les entrées créées avant le 14 janvier 2010, 12h00, dans les tables d'historique de sauvegarde et de restauration.  
  
```  
USE msdb;  
GO  
EXEC sp_delete_backuphistory @oldest_date = '01/14/2010';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_delete_database_backuphistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
