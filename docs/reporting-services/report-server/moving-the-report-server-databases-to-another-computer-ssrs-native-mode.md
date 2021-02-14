---
title: Déplacer des bases de données du serveur de rapports vers un autre ordinateur (en mode natif) | Microsoft Docs
description: Vous pouvez déplacer les bases de données du serveur de rapports qui sont utilisées dans une installation du moteur de base de données SQL Server vers une instance située sur un autre ordinateur.
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 44a9854d-e333-44f6-bdc7-8837b9f34416
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 58d7a46df8eb6580961d54e02f3ee6b0c57ee32f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059324"
---
# <a name="moving-report-server-databases-to-another-computer-ssrs-native-mode"></a>Déplacement des bases de données du serveur de rapports vers un autre ordinateur (en mode natif SSRS)

  Vous pouvez déplacer les bases de données du serveur de rapports qui sont utilisées dans une installation du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers une instance située sur un autre ordinateur. Les bases de données reportserver et reportservertempdb doivent être déplacées ou copiées ensemble. Ces deux bases de données sont requises dans une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ; la base de données reportservertempdb doit être liée par nom à la base de données reportserver primaire que vous déplacez.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif.  
  
 Le déplacement d'une base de données n'a aucune incidence sur les opérations planifiées qui sont actuellement définies pour les éléments du serveur de rapports.  
  
-   Les planifications sont recréées la première fois que vous redémarrerez le service Report Server.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les travaux de l’Agent, qui sont utilisés pour déclencher une planification, sont recréés dans la nouvelle instance de base de données. Vous n'avez pas à déplacer les travaux vers le nouvel ordinateur ; toutefois, vous pouvez supprimer des travaux sur l'ordinateur qui ne sera plus utilisé.  
  
-   Les abonnements, les instantanés et les rapports mis en cache sont préservés dans la base de données déplacée. Si un instantané ne collecte pas des données actualisées après le déplacement de la base de données, décochez les options d’instantané et sélectionnez **Appliquer** pour enregistrer vos changements, recréez la planification et resélectionnez **Appliquer** pour enregistrer vos changements.  
  
-   Les données temporaires de session utilisateur et de rapport qui sont stockées dans reportservertempdb sont conservées lorsque vous déplacez la base de données.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] propose plusieurs approches pour déplacer les bases de données, notamment la sauvegarde et la restauration, l’attachement et le détachement, ainsi que la copie. Les approches ne sont pas toutes appropriées lorsqu'il s'agit de déplacer une base de données vers une nouvelle instance de serveur. L'approche à utiliser pour déplacer la base de données du serveur de rapports varie selon les impératifs de disponibilité de votre système. Pour déplacer les bases de données du serveur de rapports, la méthode la plus simple consiste à les attacher et à les détacher. Cependant, cette approche exige une mise hors connexion du serveur de rapports durant le détachement de la base de données. La méthode de sauvegarde et de restauration est un choix plus judicieux si vous voulez minimiser les perturbations de service, mais vous devez exécuter des commandes [!INCLUDE[tsql](../../includes/tsql-md.md)] pour effectuer les opérations. La copie de la base de données n'est pas recommandée (surtout à l'aide de l'Assistant Copie de base de données), car elle ne conserve pas les paramètres d'autorisation dans la base de données.  
  
> [!IMPORTANT]  
>  Les procédures décrites dans cet article sont recommandées quand le déplacement de la base de données du serveur de rapports est le seul changement à faire dans l’installation existante. Procéder à la migration de toute une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (c’est-à-dire déplacer la base de données et modifier l’identité du service Windows Report Server utilisant la base de données) nécessite la reconfiguration de la connexion et la réinitialisation de la clé de chiffrement.  
  
## <a name="detaching-and-attaching-the-report-server-databases"></a>Détachement et attachement des bases de données du serveur de rapports  
 Si vous pouvez procéder à une mise hors connexion du serveur de rapports, vous pouvez détacher les bases de données pour les déplacer vers l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous souhaitez utiliser. Cette approche permet de conserver les autorisations définies dans les bases de données. Si vous utilisez une base de données SQL Server, vous devez la déplacer vers une autre instance SQL Server. Une fois que vous avez déplacé les bases de données, vous devez reconfigurer la connexion du serveur de rapports à la base de données du serveur de rapports. Si vous exécutez un déploiement avec montée en puissance parallèle, vous devez reconfigurer la connexion à la base de données du serveur de rapports pour chaque serveur de rapports appartenant au déploiement.  
  
 Suivez la procédure ci-dessous pour déplacer les bases de données :  
  
1.  Sauvegardez les clés de chiffrement de la base de données du serveur de rapports que vous souhaitez déplacer. Vous pouvez utiliser l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour sauvegarder les clés.  
  
2.  Arrêtez le service Report Server. Vous pouvez utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour arrêter le service.  
  
3.  Démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et établissez une connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge les bases de données du serveur de rapports.  
  
4.  Cliquez avec le bouton droit sur la base de données du serveur de rapports, puis cliquez sur **Détacher**. Répétez cette étape pour chaque base de données temporaire du serveur de rapports.  
  
5.  Copiez ou déplacez les fichiers .mdf et .ldf vers le dossier Data de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous souhaitez utiliser. Étant donné que vous déplacez deux bases de données, vérifiez que vous déplacez ou copiez les quatre fichiers.  
  
6.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], établissez une connexion à la nouvelle instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui hébergera les bases de données du serveur de rapports.  
  
7.  Cliquez avec le bouton droit sur le nœud Bases de données, puis cliquez sur **Détacher**.  
  
8.  Cliquez sur **Ajouter** pour sélectionner les fichiers .mdf et .ldf de base de données du serveur de rapports que vous voulez attacher. Répétez cette étape pour chaque base de données temporaire du serveur de rapports.  
  
9. Une fois les bases de données attachées, vérifiez que **RSExecRole** est un rôle de base de données dans la base de données du serveur de rapports et la base de données temporaire. **RSExecRole** doit disposer des autorisations de sélection, d’insertion, de mise à jour, de suppression et de référence sur les tables de la base de données du serveur de rapports, et de l’autorisation d’exécution sur les procédures stockées. Pour plus d’informations, consultez [Créer le rôle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
10. Démarrez l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis établissez une connexion au serveur de rapports.  
  
11. Dans la page Installation de la base de données, sélectionnez la nouvelle instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis cliquez sur **Se connecter**.  
  
12. Sélectionnez la base de données de serveur de rapports que vous venez de déplacer, puis cliquez sur **Appliquer**.  
  
13. Dans la page Clés de chiffrement, cliquez sur Restaurer. Spécifiez le fichier qui contient la copie de sauvegarde des clés, ainsi que le mot de passe qui déverrouille le fichier.  
  
14. Redémarrez le service Report Server.  
  
## <a name="backing-up-and-restoring-the-report-server-databases"></a>Sauvegarde et restauration des bases de données du serveur de rapports  
 Si vous ne pouvez pas procéder à la mise hors connexion du serveur de rapports, vous pouvez utiliser la sauvegarde et la restauration pour déplacer les bases de données du serveur de rapports. Vous devez utiliser des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour les opérations de sauvegarde et de restauration. Une fois les bases de données restaurées, vous devez configurer le serveur de rapports pour qu'il utilise la base de données sur la nouvelle instance de serveur. Pour plus d'informations, consultez les instructions figurant à la fin de cette rubrique.  
  
### <a name="using-backup-and-copy_only-to-backup-the-report-server-databases"></a>Utilisation de BACKUP et COPY_ONLY pour sauvegarder les bases de données du serveur de rapports  
 Lorsque vous sauvegardez les bases de données, définissez l'argument COPY_ONLY. Veillez à sauvegarder les fichiers de base de données et les fichiers journaux.  
  
```  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServer  
   SET RECOVERY FULL  
  
-- If the ReportServerData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerData.bak'  
  
-- Create a logical backup device, ReportServerLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerLog.bak'  
  
-- Back up the full ReportServer database.  
BACKUP DATABASE ReportServer  
   TO ReportServerData  
   WITH COPY_ONLY  
  
-- Back up the ReportServer log.  
BACKUP LOG ReportServer  
   TO ReportServerLog  
   WITH COPY_ONLY  
  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServerTempdb  
   SET RECOVERY FULL  
  
-- If the ReportServerTempDBData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBData',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBData.bak'  
  
-- Create a logical backup device, ReportServerTempDBLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBLog.bak'  
  
-- Back up the full ReportServerTempDB database.  
BACKUP DATABASE ReportServerTempDB  
   TO ReportServerTempDBData  
   WITH COPY_ONLY  
  
-- Back up the ReportServerTempDB log.  
BACKUP LOG ReportServerTempDB  
   TO ReportServerTempDBLog  
   WITH COPY_ONLY  
```  
  
### <a name="using-restore-and-move-to-relocate-the-report-server-databases"></a>Utilisation de RESTORE et de MOVE pour déplacer les bases de données du serveur de rapports  
 Lorsque vous restaurez les bases de données, veillez à inclure l'argument MOVE pour pouvoir spécifier un chemin d'accès. Utilisez l'argument NORECOVERY pour effectuer la restauration initiale ; la base de données reste ainsi dans l'état de restauration, ce qui vous laisse le temps de vérifier les sauvegardes des journaux pour déterminer ceux qui doivent être restaurés. L'étape finale répète l'opération RESTORE avec l'argument RECOVERY.  
  
 L'argument MOVE utilise le nom logique du fichier de données. Pour trouver le nom logique, exécutez l'instruction suivante : `RESTORE FILELISTONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Les exemples suivants incluent l'argument FILE pour que vous puissiez spécifier la position de fichier du fichier journal à restaurer. Pour trouver la position de fichier, exécutez l'instruction suivante : `RESTORE HEADERONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Lorsque vous restaurez les fichiers de base de données et les fichiers journaux, vous devez exécuter chaque opération RESTORE séparément.  
  
```  
-- Restore the report server database and move to new instance folder   
RESTORE DATABASE ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore the report server log file to new instance folder   
RESTORE LOG ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore and move the report server temporary database  
RESTORE DATABASE ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Restore the temporary database log file to new instance folder   
RESTORE LOG ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServer  
   WITH RECOVERY  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServerTempDB  
   WITH RECOVERY  
GO  
```  
  
### <a name="how-to-configure-the-report-server-database-connection"></a>Procédure à suivre pour configurer la connexion à la base de données du serveur de rapports  
  
1.  Démarrez le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis établissez une connexion au serveur de rapports.  
  
2.  Dans la page Base de données, cliquez sur **Modifier la base de données**. Cliquez sur **Suivant**.  
  
3.  Cliquez sur **Choisir une base de données de serveur de rapports existante**. Cliquez sur **Suivant**.  
  
4.  Sélectionnez le serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge désormais la base de données du serveur de rapports, puis cliquez sur **Tester la connexion**. Cliquez sur **Suivant**.  
  
5.  Dans Nom de la base de données, sélectionnez la base de données du serveur de rapports que vous voulez utiliser. Cliquez sur **Suivant**.  
  
6.  Dans Informations d'identification, spécifiez les informations d'identification que le serveur de rapports doit utiliser pour se connecter à la base de données du serveur de rapports. Cliquez sur **Suivant**.  
  
7.  Cliquez sur **Suivant** , puis sur **Terminer**.  
  
> [!NOTE]  
>  Une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] nécessite que l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] comporte le rôle **RSExecRole** . La création de rôles, l’inscription d’une connexion et les attributions de rôles ont lieu quand vous définissez la connexion à la base de données du serveur de rapports par le biais de l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si vous utilisez d'autres approches (surtout si vous recourez à l'utilitaire d'invite de commandes rsconfig.exe) pour configurer la connexion, le serveur de rapports ne sera pas en état de fonctionner. Vous devrez peut-être écrire du code WMI pour rendre le serveur de rapports disponible. Pour plus d’informations, consultez [Accéder au fournisseur WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  

## <a name="next-steps"></a>Étapes suivantes

[Créer le rôle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md)   
[Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
[Configurer une connexion à la base de données du serveur de rapports](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Configurer le compte d’exécution sans assistance](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
[Gestionnaire de configuration du serveur de rapports](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[Utilitaire rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md)   
[Configurer et gérer des clés de chiffrement](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
[Base de données du serveur de rapports](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
