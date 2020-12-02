---
title: 'Restauration en ligne : fichier en lecture seule (mode de récupération simple)'
description: Cet exemple montre la restauration en ligne d’un fichier en lecture seule d’une base de données dans SQL Server, à l’aide du mode de récupération simple avec plusieurs groupes de fichiers.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restore sequences [SQL Server], online
- online restores [SQL Server], simple recovery model
- simple recovery model [SQL Server], RESTORE examples
ms.assetid: 0c691fc6-9865-46a7-b055-8097424492d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58c4448ecde5bfed478ad19aa961dc74c1db59a5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130436"
---
# <a name="example-online-restore-of-a-read-only-file-simple-recovery-model"></a>Exemple : Restauration en ligne d’un fichier en lecture seule (Mode de récupération simple)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique concerne les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] obéissant au mode de récupération simple et contenant un groupe de fichiers en lecture seule. Avec le mode de récupération simple, un fichier en lecture seule peut être restauré en ligne s'il en existe une sauvegarde créée depuis que le fichier est passé en lecture seule pour la dernière fois.  
  
 Dans cet exemple, une base de données appelée `adb` contient trois groupes de fichiers. Le groupe de fichiers `A` est en lecture/écriture, et les groupes de fichiers `B` et `C` sont en lecture seule. Au départ, tous les groupes de fichiers sont en ligne. Un fichier en lecture seule du groupe de fichiers `B`, `b1`doit être restauré. L'administrateur de la base de données peut le restaurer à l'aide d'une sauvegarde créée après que le fichier est passé en lecture seule. Pendant la durée de la restauration, le groupe de fichiers `B` est hors connexion. Les autres parties de la base de données restent en ligne.  
  
## <a name="restore-sequence"></a>Séquence de restauration  
  
> [!NOTE]  
>  La syntaxe pour une séquence de restauration en ligne est la même que pour une séquence de restauration hors connexion.  
  
 Pour restaurer le fichier, l'administrateur de la base de données doit utiliser la séquence de restauration suivante :  
  
```  
RESTORE DATABASE adb FILE='b1' FROM filegroup_B_backup   
WITH RECOVERY  
```  
  
 Le fichier est en ligne.  
  
## <a name="additional-examples"></a>Autres exemples  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers uniquement &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire d’une base de données &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Exemple : restauration fragmentaire de quelques groupes de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Exemple : restauration en ligne d’un fichier en lecture seule &#40;mode de restauration complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
