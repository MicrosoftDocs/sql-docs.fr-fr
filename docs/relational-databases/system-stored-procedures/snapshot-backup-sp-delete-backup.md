---
description: sp_delete_backup (Transact-SQL)
title: sp_delete_backup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 808e50ae-ff6e-4520-9ce2-530591d3d59b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 003b6422fe2485c1e7c7da43aa5f5168a75b797e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186014"
---
# <a name="sp_delete_backup-transact-sql"></a>sp_delete_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Supprime tous les instantanés et le fichier de sauvegarde qui composent un jeu de sauvegarde d’instantané de la base de données spécifiée. Cette procédure stockée système est la seule méthode recommandée pour gérer les jeux de sauvegardes d’instantanés. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_delete_backup   
    [ @backup_url = ] backup_metadata_file_url  
    ,[ [ @db_name = ] database_name | NULL ]  
```  
  
## <a name="arguments"></a>Arguments  
 *[ @backup_url =] backup_meta_file_url*  
 URL de la sauvegarde à supprimer, qui supprime toutes les captures instantanées contenant le jeu de sauvegarde spécifié, y compris le fichier de sauvegarde lui-même.  
  
 *[ @db_name =] database_name*  
 Nom de la base de données contenant l’instantané à supprimer. Lorsqu’un nom de base de données est fourni, le système vérifie que l’URL de sauvegarde fournie est une URL de sauvegarde pour la base de données spécifiée et utilise [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) pour supprimer chaque instantané. Si aucun nom de base de données n’est fourni, la vérification de la base de données n’est pas effectuée.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation ALTER ANY DATABASE ou l’autorisation ALTER sur la base de données spécifiée.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)   
 [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)  
  
  
