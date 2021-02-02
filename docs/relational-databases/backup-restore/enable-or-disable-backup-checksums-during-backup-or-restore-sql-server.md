---
title: Activer | désactiver les sommes de contrôle de sauvegarde
description: Cet article explique comment activer ou désactiver les sommes de contrôle de sauvegarde pour une base de données dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup checksums [SQL Server]
- disabling checksums
- checksums [SQL Server]
ms.assetid: 6786bd1e-ad97-430a-8dfb-d4ba952d6c4d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 47716f50091f695a43d751c57e9dbfbbe6b7001e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075911"
---
# <a name="enable-or-disable-backup-checksums-during-backup-or-restore-sql-server"></a>Activer ou désactiver les sommes de contrôle de sauvegarde au cours d’opérations de sauvegarde ou de restauration (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit comment activer ou désactiver les sommes de contrôle de sauvegarde lorsque vous sauvegardez ou restaurez une base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour activer ou désactiver les sommes de contrôle de sauvegarde, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 BACKUP  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
 RESTORE  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-enable-or-disable-checksums-during-a-backup-operation"></a>Pour activer ou désactiver les sommes de contrôle pendant une opération de sauvegarde  
  
1.  Suivez les étapes pour [créer une sauvegarde de base de données](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  Sur la page **Options** , dans la section **Fiabilité** , cliquez sur **Effectuer une somme de contrôle avant d'écrire sur le support**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-enable-or-disable-backup-checksum-for-a-backup-operation"></a>Pour activer ou désactiver la somme de contrôle de sauvegarde pour une opération de sauvegarde  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Pour activer les sommes de contrôle de sauvegarde dans une instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) , spécifiez l'option WITH CHECKSUM. Pour désactiver les sommes de contrôle de sauvegarde, spécifiez l'option WITH NO_CHECKSUM. Ceci est le comportement par défaut, sauf pour une sauvegarde compressée. L'exemple suivant spécifie que les sommes de contrôle doivent être effectuées.  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH CHECKSUM;  
GO  
```  
  
#### <a name="to-enable-or-disable-backup-checksum-for-a-restore-operation"></a>Pour activer ou désactiver la somme de contrôle de sauvegarde pour une opération de restauration  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Pour activer les sommes de contrôle de sauvegarde dans une instruction [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) , spécifiez l'option WITH CHECKSUM. Ceci est le comportement par défaut pour une sauvegarde compressée. Pour désactiver les sommes de contrôle de sauvegarde, spécifiez l'option WITH NO_CHECKSUM. Ceci est le comportement par défaut, sauf pour une sauvegarde compressée. L'exemple suivant spécifie que les sommes de contrôle de sauvegarde doivent être effectuées.  
  
```sql  
RESTORE DATABASE AdventureWorks2012   
 FROM DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH CHECKSUM;  
GO  
```  
  
> [!WARNING]  
>  Si vous spécifiez explicitement CHECKSUM pour une opération de restauration et que la sauvegarde contient des sommes de contrôle de sauvegarde, ces sommes de contrôle de sauvegarde ainsi que les sommes de contrôle de page sont vérifiées, comme dans le cas par défaut. Toutefois, si le jeu de sauvegarde manque de sommes de contrôle de sauvegarde, l'opération de restauration se solde par un échec et l'affichage d'un message indiquant l'absence des sommes de contrôle.  
  
## <a name="see-also"></a> Voir aussi  
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Spécifier si une opération de sauvegarde ou de restauration continue ou s’arrête après la survenue d’une erreur &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
  
