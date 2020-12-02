---
title: 'Restauration hors ligne : groupe de fichiers primaire et 1 groupe de fichiers'
description: Cet exemple montre la restauration hors ligne du groupe de fichiers principal d’une base de données et d’un autre groupe de fichiers dans SQL Server, à l’aide du mode de récupération complète avec plusieurs groupes de fichiers.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- offline restores [SQL Server]
- restore sequences [SQL Server], offline
ms.assetid: 7d6c50eb-dc84-4d66-855a-0b5f1bd89737
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3a09b3fc709cff218548b35318505372acca3479
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130420"
---
# <a name="example-offline-restore-of-primary-and-1-other-filegroup-full-recovery-model"></a>Exemple : Restauration hors ligne du groupe de fichiers primaire et d’un autre groupe de fichiers (mode de récupération complète)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique concerne uniquement les bases de données contenant plusieurs groupes de fichiers et obéissant au mode de récupération complète.  
  
 Dans cet exemple, une base de données appelée `adb` contient trois groupes de fichiers. Les groupes de fichiers `A` et `C` sont en lecture-écriture, et le groupe de fichiers `B` est en lecture seule. Le groupe de fichiers primaire et le groupe de fichiers `B` sont endommagés, tandis que les groupes de fichiers `A` et `C` sont intacts. Avant le sinistre, tous les groupes de fichiers étaient en ligne.  
  
 L'administrateur de base de données décide de restaurer et de récupérer le groupe de fichiers primaire et le groupe de fichiers `B`. La base de données utilise le mode de restauration complète ; par conséquent, avant le début de la restauration, il convient d'effectuer une sauvegarde de la fin du journal sur la base de données. Lorsque la base de données se retrouve en ligne, les groupes de fichiers `A` et `C` sont automatiquement mis en ligne.  
  
> [!NOTE]  
>  La séquence de restauration hors connexion a moins d'étapes qu'une restauration en ligne d'un fichier en lecture seule. Pour obtenir un exemple, consultez [Exemple : Restauration en ligne d’un fichier en lecture seule &#40;Mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md). Toutefois, la totalité de la base de données se trouve hors connexion tout au long de la séquence.  
  
## <a name="tail-log-backup"></a>Sauvegarde de la fin du journal  
 Avant de restaurer la base de données, l'administrateur de la base de données doit sauvegarder la fin du journal. La base de données étant endommagée, la création de la sauvegarde de la fin du journal requiert l'utilisation de l'option NO_TRUNCATE :  
  
```  
BACKUP LOG adb TO tailLogBackup   
   WITH NORECOVERY, NO_TRUNCATE  
```  
  
 La sauvegarde de la fin du journal est la dernière sauvegarde appliquée dans les séquences de restauration qui suivent.  
  
## <a name="restore-sequence"></a>Séquence de restauration  
 Pour restaurer le groupe de fichiers primaire et le groupe de fichiers `B`, l'administrateur de base de données utilise une séquence de restauration sans l'option PARTIAL, comme suit :  
  
```  
RESTORE DATABASE adb FILEGROUP='Primary' FROM backup1   
WITH NORECOVERY  
RESTORE DATABASE adb FILEGROUP='B' FROM backup2   
WITH NORECOVERY  
RESTORE LOG adb FROM backup3 WITH NORECOVERY  
RESTORE LOG adb FROM backup4 WITH NORECOVERY  
RESTORE LOG adb FROM backup5 WITH NORECOVERY  
RESTORE LOG adb FROM tailLogBackup WITH RECOVERY  
```  
  
 Les fichiers non restaurés sont automatiquement mis en ligne. Tous les groupes de fichiers sont désormais en ligne.  
  
## <a name="see-also"></a> Voir aussi  
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
