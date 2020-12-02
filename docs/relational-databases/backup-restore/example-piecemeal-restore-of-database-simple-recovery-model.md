---
title: 'Restauration fragmentaire : mode de récupération simple'
description: Cet exemple montre la restauration fragmentaire d’une base de données dans SQL Server sur un nouvel ordinateur, à l’aide du mode de récupération simple.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], simple recovery model
- restore sequences [SQL Server], piecemeal
- simple recovery model [SQL Server], RESTORE examples
ms.assetid: 9834b14a-4e56-4654-b190-c2a38624b6b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9114f8c2318b7b17692a6c522b6c87f019886a3
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126944"
---
# <a name="example-piecemeal-restore-of-database-simple-recovery-model"></a>Exemple : Restauration fragmentaire d’une base de données (mode de récupération simple)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Une séquence de restauration fragmentaire restaure et récupère une base de données par étapes au niveau des groupes de fichiers, en commençant par le groupe de fichiers primaire et tous les groupes de fichiers secondaires en lecture-écriture.  
  
 Dans cet exemple, la base de données `adb` est restaurée sur un nouvel ordinateur après un problème grave. La base de données utilise le mode de récupération simple. Avant le sinistre, tous les groupes de fichiers sont en ligne. Les groupes de fichiers `A` et `C` sont en lecture-écriture, et le groupe de fichiers `B` est en lecture seule. Le groupe de fichiers `B` est passé en lecture seule avant la sauvegarde partielle la plus récente qui contient le groupe de fichiers principal et les groupes de fichiers secondaires en lecture-écriture, `A` et `C`. Après le passage du groupe de fichiers `B` en lecture seule, une sauvegarde de fichiers séparée du groupe de fichiers `B` a été effectuée.  
  
## <a name="restore-sequences"></a>Séquences de restauration  
  
1.  Restauration partielle des groupes de fichiers primaires `A` et `C`.  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='A',FILEGROUP='C'   
       FROM partial_backup   
       WITH PARTIAL, RECOVERY;  
  
    ```  
  
     À ce stade, le groupe de fichiers primaire et les groupes de fichiers `A` et `C` sont en ligne. Tous les fichiers du groupe de fichiers `B` sont en attente de récupération et le groupe de fichiers est hors connexion.  
  
2.  Restauration en ligne du groupe de fichiers `B`.  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='B' FROM backup WITH RECOVERY;  
  
    ```  
  
     Tous les groupes de fichiers sont maintenant en ligne.  
  
## <a name="additional-examples"></a>Autres exemples  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
