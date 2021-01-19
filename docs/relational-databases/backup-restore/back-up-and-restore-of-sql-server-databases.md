---
title: Sauvegarder et restaurer des bases de données SQL Server | Microsoft Docs
description: Cet article décrit les avantages que présente la sauvegarde des bases de données SQL Server, puis il décrit les stratégies de sauvegarde et de restauration, et aborde certains points relatifs à la sécurité.
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], see restoring [SQL Server]
- backups [SQL Server]
- restoring databases [SQL Server]
- backup [SQL Server], see backing up [SQL Server]
- databases [SQL Server], restoring
- backing up databases [SQL Server]
- restore [SQL Server], see restoring [SQL Server]
- disaster recovery [SQL Server], see backing up [SQL Server]
- backing up [SQL Server]
- Database Engine [SQL Server], backups
- databases [SQL Server], backups
ms.assetid: 570a21b3-ad29-44a9-aa70-deb2fbd34f27
author: cawrites
ms.author: chadam
ms.openlocfilehash: 76aa3e890169e73d30d9a15aa57aa4fd832a7ef9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171311"
---
# <a name="back-up-and-restore-of-sql-server-databases"></a>Sauvegarde et restauration des bases de données SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cet article décrit les avantages de la sauvegarde des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les conditions de sauvegarde et de restauration de base, et présente les stratégies de sauvegarde et de restauration pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ainsi que les questions de sécurité pour la sauvegarde et la restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> Cet article présente les sauvegardes SQL Server. Pour découvrir la procédure spécifique de sauvegarde des bases de données SQL Server, consultez [Création de sauvegardes](#creating-backups).
  
 Le composant de sauvegarde et restauration de SQL Server apporte une sécurité essentielle pour la protection des données cruciales stockées dans vos bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour réduire le risque de perte catastrophique de données, vous devez sauvegarder vos bases de données pour conserver les modifications apportées à vos données de façon régulière. Une stratégie de sauvegarde et de restauration correctement planifiée permet de protéger les bases de données contre la perte de données provoquée par différentes défaillances. Testez votre stratégie en restaurant un ensemble de sauvegardes, puis en récupérant votre base de données pour vous préparer à réagir efficacement en cas de sinistre.
  
 En plus du stockage local pour stocker les sauvegardes, SQL Server prend également en charge la sauvegarde et la restauration à partir du service Stockage Blob Azure. Pour plus d’informations, voir [SQL Server Backup and Restore with Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)(en anglais). Pour les fichiers de base de données stockés à l’aide du service Microsoft Azure Blob Storage, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] offre la possibilité d’utiliser des instantanés Azure pour obtenir des sauvegardes quasi instantanées et des restaurations plus rapides. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md). Azure offre également une solution de sauvegarde de classe entreprise pour les instances SQL Server s’exécutant sur des machines virtuelles Azure. Solution de sauvegarde entièrement managée, elle prend en charge les groupes de disponibilité Always On, la rétention à long terme, la récupération jusqu`à une date et heure, ainsi que des fonctions de gestion et de surveillance centralisées. Pour plus d’informations, voir [Sauvegarde Azure pour SQL Server dans une machine virtuelle Azure](/azure/backup/backup-azure-sql-database).
  
##  <a name="why-back-up"></a>Pourquoi sauvegarder ?  
-   La sauvegarde de vos bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'exécution de procédures de restauration de test sur vos sauvegardes et le stockage des copies des sauvegardes à un emplacement sécurisé et hors site permettent de vous protéger contre une éventuelle perte catastrophique de données. **La sauvegarde est le seul moyen de protéger vos données.**

     Avec des sauvegardes valides d'une base de données, vous pouvez récupérer vos données suite à de nombreuses défaillances, par exemple :  
  
    -   défaillance du support ;    
    -   erreurs utilisateur, telles que la suppression d'une table par inadvertance ;    
    -   défaillances matérielles, telles qu'un lecteur de disque endommagé ou la perte permanente d'un serveur ;    
    -   catastrophes naturelles. Utilisez Sauvegarde SQL Server dans le service Stockage Blob Azure pour créer une sauvegarde hors site dans une autre région que celle de votre emplacement local, qui sera disponible en cas de catastrophe naturelle affectant votre emplacement local.  
  
-   Par ailleurs, il peut être utile d'effectuer des sauvegardes d'une base de données afin d'effectuer des tâches administratives de routine, telles que la copie d'une base de données d'un serveur vers un autre, la configuration de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ou la mise en miroir de bases de données et l'archivage.  
  
##  <a name="glossary-of-backup-terms"></a>Glossaire des termes de sauvegarde
 **sauvegarder** [verbe]  
 Processus de création d’une **sauvegarde [nom]** en copiant les enregistrements de données à partir d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou les enregistrements de journaux à partir du journal des transactions.  
  
 **sauvegarde** [nom]  
 Copie de données qui peut être utilisée pour restaurer et récupérer les données après une défaillance. Les sauvegardes d'une base de données peuvent également être utilisées pour restaurer une copie de la base de données à un nouvel emplacement.  
  
unité de **sauvegarde**  
 Unité de disque ou de bande sur laquelle les sauvegardes de SQL Server sont écrites et à partir de laquelle elles peuvent être restaurées. Les sauvegardes SQL Server peuvent également être écrites dans un service de stockage Blob Azure, et le format d’ **URL** est utilisé pour spécifier la destination et le nom du fichier de sauvegarde. Pour plus d’informations, voir [SQL Server Backup and Restore with Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)(en anglais).  
  
**support de sauvegarde**  
 Une ou plusieurs bandes ou un ou plusieurs fichiers sur disque sur lesquels une ou plusieurs sauvegardes ont été écrites.  
  
**sauvegarde de données**  
 Sauvegarde de données dans une base de données complète (sauvegarde de base de données), une base de données partielle (sauvegarde partielle) ou un ensemble de fichiers ou groupes de fichiers (sauvegarde de fichiers).  
  
**sauvegarde de base de données**  
 Sauvegarde d'une base de données. Les sauvegardes complètes de base de données représentent l'intégralité de la base de données à l'issue de l'opération de sauvegarde. Les sauvegardes différentielles contiennent uniquement les modifications apportées à la base de données depuis sa plus récente sauvegarde complète de base de données.  
  
**sauvegarde différentielle**  
 Sauvegarde de données basée sur la dernière sauvegarde complète d'une base de données complète ou partielle ou d'un ensemble de fichiers de données ou de groupes de fichiers (base différentielle) et qui contient uniquement les données ayant changé depuis cette base.  
  
**sauvegarde complète**  
 Sauvegarde de données qui contient toutes les données d'une base de données particulière ou d'un jeu de groupes de fichiers ou de fichiers, ainsi qu'une partie suffisante du journal pour permettre la récupération de ces données.  
  
**sauvegarde de fichier journal**  
 Sauvegarde des journaux des transactions qui inclut tous les enregistrements des journaux qui n'ont pas été sauvegardés lors d'une sauvegarde de fichier journal précédente. (mode de récupération complète)  
  
**recover**  
 Pour rétablir une base de données à un état stable et cohérent.  
  
**recovery**  
 Phase de démarrage de base de données ou de restauration avec récupération qui place la base de données dans un état cohérent au niveau des transactions.  
  
**mode de récupération**  
 Propriété de base de données qui contrôle la maintenance du journal des transactions sur une base de données. Il existe trois modes de récupération : simple, complète et utilisant les journaux de transactions. Le mode de récupération de base de données détermine les spécifications de sauvegarde et de restauration.  
  
**restore**  
 Processus à plusieurs phases qui copie toutes les données et les pages des journaux à partir d'une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiée dans une base de données spécifiée, puis restaure toutes les transactions journalisées dans la sauvegarde en appliquant les modifications journalisées pour rétablir un état ultérieur des données.  
  
 ##  <a name="backup-and-restore-strategies"></a>Stratégies de sauvegarde et de restauration  
 Les sauvegardes et restaurations de données doivent être adaptées à un environnement particulier et doivent pouvoir utiliser les ressources disponibles. Pour qu’elles soient efficaces, les sauvegardes et restaurations aux fins de récupération doivent par conséquent faire l’objet d’une stratégie. Une stratégie de sauvegarde et de restauration bien conçue doit équilibrer les besoins spécifiques de l’entreprise en matière de disponibilité maximale et de perte minimale des données, tout en prenant en compte le coût de maintenance et de stockage des sauvegardes.  

 Une stratégie de sauvegarde et de restauration s'articule autour de deux pôles : la sauvegarde et la restauration. Le pôle sauvegarde de la stratégie définit le type et la fréquence des sauvegardes, la nature et la vitesse du matériel employé, la manière dont les sauvegardes sont testées, ainsi que les modalités et l’emplacement de stockage des supports de sauvegarde (sans oublier les questions de sécurité). Le pôle restauration détermine le responsable des restaurations et leurs modalités d’exécution pour atteindre les objectifs de l’entreprise en termes de disponibilité de la base de données, de limitation des pertes de données et de test des restaurations. 
  
 La conception d'une stratégie de sauvegarde et de restauration efficace nécessite une planification, une mise en œuvre et des tests rigoureux. Des tests sont nécessaires : vous ne disposez d’aucune stratégie de sauvegarde tant que vous n’avez pas restauré comme il se doit les sauvegardes dans toutes les combinaisons incluses dans votre stratégie de restauration et testé la cohérence physique de la base de données restaurée. Vous devez prendre en compte différents facteurs. notamment :  
  
- les objectifs de votre entreprise en ce qui concerne les bases de données de production, en particulier les besoins de disponibilité et de protection des données contre les pertes ou les sinistres ;  
  
-  la nature de chaque base de données : sa taille, ses modèles d’utilisation, la nature de son contenu, les exigences au niveau des données, etc. ;  
  
-   les contraintes imposées aux ressources telles que : matériel, personnel, espace de stockage des supports de sauvegarde et leur sécurité physique, etc.  

## <a name="best-practice-recommendations"></a>Recommandations relatives aux bonnes pratiques

### <a name="use-separate-storage"></a>Utiliser un stockage distinct 
> [!IMPORTANT] 
> Veillez à placer vos sauvegardes de bases de données sur un appareil ou à un emplacement physique distinct des fichiers de base de données. Quand votre lecteur physique qui stocke les bases de données ne fonctionne pas ou plante, la récupération dépend de la possibilité d’accéder à un lecteur distinct ou à un appareil distant qui stocke les sauvegardes afin de procéder à une restauration. Gardez à l’esprit que vous pouvez créer plusieurs partitions ou volumes logiques à partir d’un même lecteur de disque physique. Étudiez attentivement la disposition des partitions de disque et des volumes logiques avant de choisir un emplacement de stockage pour les sauvegardes.

### <a name="choose-appropriate-recovery-model"></a>Choisir le mode de récupération approprié
 Les opérations de sauvegarde et de restauration interviennent dans le cadre d’un [mode de récupération](../backup-restore/recovery-models-sql-server.md). Un mode de récupération désigne une propriété de base de données qui contrôle le mode de gestion du journal des transactions. Ainsi, le mode de récupération d’une base de données détermine les types de sauvegardes et les scénarios de restauration pris en charge pour la base de données ainsi qu’une estimation de la taille des sauvegardes des journaux des transactions. En règle générale, une base de données utilise le mode de récupération complète ou le mode de récupération simple. Le mode de récupération complète peut être augmenté en basculant vers le mode de récupération utilisant les journaux de transactions avant d’effectuer des opérations en bloc. Pour obtenir une présentation de ces modes de récupération et leur impact sur la gestion du journal des transactions, consultez [Journal des transactions (SQL Server)](../logs/the-transaction-log-sql-server.md)  
  
 Le meilleur choix du mode de récupération de la base de données dépend de vos exigences d'entreprise. Pour éviter la gestion du journal des transactions et simplifier la sauvegarde et la restauration, optez pour le mode de récupération simple. Pour minimiser les risques de perte de travail, mais avec un coût en termes de charges d'administration, choisissez le mode de récupération complète. Pour réduire l’impact sur la taille du journal pendant les opérations journalisées en bloc tout en autorisant la récupération de ces opérations, utilisez le mode de récupération utilisant les journaux de transactions. Pour obtenir des informations sur l’impact des modes de récupération sur la sauvegarde et la restauration, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
### <a name="design-your-backup-strategy"></a>Concevoir votre stratégie de sauvegarde  
 Après avoir choisi un mode de récupération satisfaisant vos exigences d'entreprise pour une base de données précise, vous devez planifier et implémenter une stratégie de sauvegarde correspondante. Le choix de la meilleure stratégie de sauvegarde possible dépend d'un éventail de facteurs, notamment des facteurs primordiaux suivants :  
  
-   Combien d'heures par jour les applications ont-elles besoin d'accéder à la base de données ?  
  
     S'il existe une période creuse évidente, nous vous recommandons de planifier les sauvegardes complètes de la base de données pendant cette période.  
  
-   Quelle est la fréquence de modification et de mise à jour possible ?  
  
     Si les modifications sont fréquentes, tenez compte des éléments suivants :  
  
    -   Dans le cadre du mode de récupération simple, planifiez, si possible, des sauvegardes différentielles entre les sauvegardes complètes de la base de données. Une sauvegarde différentielle enregistre uniquement les modifications effectuées depuis la toute dernière sauvegarde complète de la base de données.  
  
    -   Si vous travaillez en mode de récupération complète, prévoyez des sauvegardes fréquentes du journal. La planification de sauvegardes différentielles entre des sauvegardes complètes peut réduire le temps de restauration en diminuant le nombre de sauvegardes de fichier journal à restaurer après la restauration des données.  
  
-   Ces modifications risquent-elles de porter sur une petite ou sur une grande partie de la base de données ?  
  
     Dans le cas d'une base de données volumineuse dont les modifications sont concentrées dans une partie des fichiers ou des groupes de fichiers, des sauvegardes partielles et/ou des sauvegardes de fichiers peuvent s'avérer utiles. Pour plus d’informations, consultez [Sauvegardes partielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md) et [Sauvegardes complètes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md).  
  
-   Quelle est la quantité d'espace disque nécessaire pour une sauvegarde complète de base de données ?  
-   Pendant combien de temps votre entreprise doit-elle conserver les sauvegardes ? 

    Vérifiez que vous disposez d’une planification de sauvegarde appropriée établie en fonction des besoins de l’application et des exigences de l’entreprise. À mesure que les sauvegardes vieillissent, le risque de perte de données est plus élevé, sauf si vous avez la possibilité de régénérer toutes les données jusqu’au point de défaillance. Avant de choisir de supprimer les anciennes sauvegardes en raison des limitations de ressources de stockage, voyez si la récupération est nécessaire aussi loin dans le passé

  
 ### <a name="estimate-the-size-of-a-full-database-backup"></a>Estimer la taille d’une sauvegarde complète de base de données  
 Avant de mettre en place une stratégie de sauvegarde et de restauration, vous devez estimer la quantité d'espace disque qu'utilisera une sauvegarde complète de base de données. L'opération de sauvegarde copie les données de la base de données dans un fichier de sauvegarde. La sauvegarde contient uniquement les données réelles de la base de données et aucun espace inutilisé. Ainsi, la sauvegarde est généralement moins volumineuse que la base de données elle-même. Vous pouvez estimer la taille d’une sauvegarde complète de base de données en utilisant la procédure stockée système **sp_spaceused** . Pour plus d’informations, consultez [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md).  
  
### <a name="schedule-backups"></a>Planifier des sauvegardes  
 Les opérations de sauvegarde ayant peu d'impact sur l'exécution des transactions, elles peuvent donc avoir lieu en même temps que les opérations normales. Vous pouvez effectuer une sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec un effet minimal sur les charges de production.  
   
>  Pour plus d’informations sur les restrictions d’accès concurrentiel lors d’une sauvegarde, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
 Après avoir choisi les types de sauvegardes dont vous avez besoin et défini la fréquence à laquelle vous devez effectuer chaque type, nous vous recommandons de prévoir des sauvegardes régulières dans le cadre d'un plan de maintenance de la base de données. Pour plus d'informations sur les plans de maintenance et leur création pour les sauvegardes de base de données et de journaux, consultez [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).
  
### <a name="test-your-backups"></a>Évaluer vos sauvegardes  
 Vous ne disposez d'aucune stratégie de restauration tant que vous n'avez pas testé vos sauvegardes. Il est essentiel de procéder à une évaluation minutieuse de votre stratégie de sauvegarde pour chacune de vos bases de données en restaurant une copie de la base de données sur un système de test. Vous devez tester la restauration de chaque type de sauvegarde que vous envisagez d'utiliser. Une fois que vous avez restauré la sauvegarde, il est également recommandé d’effectuer des vérifications de cohérence de la base de données via DBCC CHECKDB de la base de données pour valider que le support de sauvegarde n’a pas été endommagé. 

### <a name="verify-media-stability-and-consistency"></a>Vérifier la stabilité et la cohérence des supports
Utilisez les options de vérification fournies par les utilitaires de sauvegarde (commande T-SQL BACKUP, plans de maintenance SQL Server, votre solution ou logiciel de sauvegarde, etc.). Pour obtenir un exemple, consultez [RESTORE VERIFYONLY] (../t-sql/statements/restore-statements-verifyonly-transact-sql.md). Utilisez des fonctionnalités avancées telles que BACKUP CHECKSUM pour détecter les problèmes liés au support de sauvegarde lui-même. Pour plus d’informations, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration (SQL Server)](../backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

### <a name="document-backuprestore-strategy"></a>Stratégie de sauvegarde/restauration de documents 
Nous vous recommandons de documenter vos procédures de sauvegarde et de restauration, sans oublier de conserver une copie de la documentation dans votre dossier d'exploitation.
Nous vous recommandons également la tenue d’un manuel des opérations pour chaque base de données. Ce manuel doit consigner l'emplacement des sauvegardes, les noms des unités de sauvegarde (le cas échéant) et le temps requis pour la restauration des sauvegardes de test.



## <a name="monitor-progress-with-xevent"></a>Surveiller la progression avec un xEvent
Les opérations de sauvegarde et de restauration peuvent prendre beaucoup de temps en raison de la taille de la base de données et de la complexité des opérations impliquées. En cas de problème avec l’une ou l’autre des opérations, vous pouvez utiliser l’événement étendu **backup_restore_progress_trace** pour surveiller la progression en direct. Pour plus d'informations sur les événements étendus, consultez [Événements étendus](../extended-events/extended-events.md).

  >[!WARNING]
  > L’utilisation de l’événement étendu backup_restore_progress_trace peut impacter les performances et consommer une grande quantité d’espace disque. Utilisez-le sur de courtes périodes, avec précaution, et effectuez des tests avant l’implémentation en production.


```sql
-- Create the backup_restore_progress_trace extended event esssion
CREATE EVENT SESSION [BackupRestoreTrace] ON SERVER 
ADD EVENT sqlserver.backup_restore_progress_trace
ADD TARGET package0.event_file(SET filename=N'BackupRestoreTrace')
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=5 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
GO

-- Start the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = start;  
GO  

-- Stop the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = stop;  
GO  
```

### <a name="sample-output-from-extended-event"></a>Exemple de sortie d’un événement étendu 

![Exemple de sortie d’un xevent de sauvegarde](media/back-up-and-restore-of-sql-server-databases/backup-xevent-example.png)
![Exemple de sortie d’un xevent de restauration](media/back-up-and-restore-of-sql-server-databases/restore-xevent-example.png)
 
  
## <a name="more-about-backup-tasks"></a>En savoir plus sur les tâches de sauvegarde  
-   [Créer un plan de maintenance](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
-   [Créer un travail](../../ssms/agent/create-a-job.md)  
  
-   [Planifier un travail](../../ssms/agent/schedule-a-job.md)  
  
## <a name="working-with-backup-devices-and-backup-media"></a>Utilisation des unités et supports de sauvegarde  
-   [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [Spécifier un disque ou une bande comme destination de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [Supprimer une unité de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [Définir la date d’expiration d’une sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [Afficher le contenu d’un fichier ou d’une bande de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [Afficher les fichiers de données et les fichiers journaux dans un jeu de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [Afficher les propriétés et le contenu d’une unité de sauvegarde logique &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [Restaurer une sauvegarde à partir d’une unité &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
## <a name="creating-backups"></a>Création de sauvegardes  
**Remarque** Pour réaliser des sauvegardes partielles ou de copie uniquement, vous devez utiliser l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) avec l’option PARTIAL ou COPY_ONLY, respectivement.  
  
 ### <a name="using-ssms"></a>Utilisation de SSMS   
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 ### <a name="using-t-sql"></a>Utilisation de T-SQL  
-   [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [Activer ou désactiver des sommes de contrôle de sauvegarde au cours d’opérations de sauvegarde ou de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [Spécifier si une opération de sauvegarde ou de restauration continue ou s’arrête après la survenue d’une erreur &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="restore-data-backups"></a>Restaurer des sauvegardes de données 
### <a name="using-ssms"></a>Utilisation de SSMS 
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
-   [Restaurer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Restaurer des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
### <a name="using-t-sql"></a>Utilisation de T-SQL
-   [Restaurer une sauvegarde de base de données en mode de récupération simple &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)  
  
-   [Restaurer une base de données jusqu’au point d’échec en mode de récupération complète &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurer des fichiers et groupes de fichiers en remplaçant des fichiers existants &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Restaurer des fichiers à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Restaurer la base de données MASTER &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  

## <a name="restore-transaction-logs-full-recovery-model"></a>Restaurer des journaux de transactions (mode de récupération complète)
### <a name="using-ssms"></a>Utilisation de SSMS  
-   [Restaurer une base de données jusqu’à une transaction marquée &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
 ### <a name="using-t-sql"></a>Utilisation de T-SQL 
-   [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
 
-   [Redémarrer une opération de restauration interrompue &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
-   [Récupérer une base de données sans restaurer les données &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
 
## <a name="more-information-and-resources"></a>Informations et ressources supplémentaires
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sauvegarde et restauration de bases de données Analysis Services](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)   
 [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
