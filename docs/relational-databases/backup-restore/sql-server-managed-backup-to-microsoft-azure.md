---
title: Gestion de sauvegarde de SQL Server sur Microsoft Azure| Microsoft Docs
description: La sauvegarde managée SQL Server vers Microsoft Azure permet de gérer et d’automatiser les sauvegardes SQL Server dans le stockage Blob Microsoft Azure.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: afa01165-39e0-4efe-ac0e-664edb8599fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: c925c4a8361b54ff563b6a1443e392f0ca392df9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237265"
---
# <a name="sql-server-managed-backup-to-microsoft-azure"></a>Gestion de sauvegarde de SQL Server sur Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] gère et automatise les sauvegardes SQL Server dans le stockage d’objets blob Microsoft Azure. Vous pouvez choisir d’autoriser SQL Server à déterminer la planification de sauvegarde en fonction de la charge de travail des transactions de votre base de données. Ou vous pouvez utiliser les options avancées pour définir une planification. Les paramètres de rétention déterminent la durée pendant laquelle les sauvegardes sont stockées dans le stockage d’objets blob Azure. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] prend en charge la restauration limitée dans le temps pour la période de rétention spécifiée.  
  
 À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], les procédures et le comportement sous-jacent de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] ont changé. Pour plus d’informations, consultez [Migrer les paramètres de gestion de sauvegarde de SQL Server 2014 vers SQL Server 2016](../../relational-databases/backup-restore/migrate-sql-server-2014-managed-backup-settings-to-sql-server-2016.md).  
  
> [!TIP]  
>  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est recommandée pour les instances de SQL Server exécutées sur des machines virtuelles Microsoft Azure.  
  
## <a name="benefits"></a>Avantages  
 Actuellement, l'automatisation des sauvegardes pour plusieurs bases de données requiert le développement d'une stratégie de sauvegarde, l'écriture d'un code personnalisé et la planification des sauvegardes. À l’aide de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], vous pouvez créer un plan de sauvegarde en spécifiant uniquement la durée de rétention et l’emplacement de stockage. Il existe des paramètres avancés mais ils ne sont pas obligatoires. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] planifie, exécute et administre les sauvegardes.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] peut être configurée au niveau de la base de données ou au niveau de l’instance de SQL Server. Lors de la configuration au niveau de l’instance, les nouvelles bases de données sont également sauvegardées automatiquement. Vous pouvez utiliser les paramètres au niveau de la base de données pour remplacer les valeurs par défaut au niveau de l’instance pour un cas particulier.  
  
 Vous pouvez également chiffrer les sauvegardes pour renforcer la sécurité, et vous pouvez configurer une planification personnalisée pour contrôler le moment où les sauvegardes sont effectuées. Pour plus d’informations sur les avantages de Microsoft Azure Blob Storage pour les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
##  <a name="prerequisites"></a><a name="Prereqs"></a> Conditions préalables  
 Le stockage Microsoft Azure est utilisé par la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour stocker les fichiers de sauvegarde. Les prérequis suivants sont obligatoires :  
  
|Configuration requise|Description|  
|------------------|-----------------|  
|**Compte Microsoft Azure**|Vous pouvez commencer à utiliser Azure avec une [version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/) avant d’explorer les [options d’achat](https://azure.microsoft.com/pricing/purchase-options/).|  
|**Compte Stockage Azure**|Les sauvegardes sont stockées dans le stockage d’objets blob Azure associé à un compte de stockage Azure. Pour obtenir des instructions détaillées sur la création d’un compte de stockage, consultez [À propos des comptes de stockage Azure](/azure/storage/common/storage-account-create).|  
|**Conteneur d’objets blob**|Les objets blob sont organisés dans des conteneurs. Vous spécifiez le conteneur cible pour les fichiers de sauvegarde. Vous pouvez créer un conteneur dans le [portail de gestion Azure](https://manage.windowsazure.com/)ou vous pouvez utiliser la commande **New-AzureStorageContainer**[Azure PowerShell](/powershell/azure/) .|  
|**Signature d’accès partagé (SAP)**|L’accès au conteneur cible est contrôlé par une signature d’accès partagé (SAS). Pour une vue d’ensemble de SAS, consultez [Signatures d’accès partagé, partie 1 : Présentation du modèle SAP](/azure/storage/common/storage-sas-overview). Vous pouvez créer un jeton SAS dans le code ou avec la commande PowerShell **New-AzureStorageContainerSASToken** . Pour obtenir un script PowerShell qui simplifie ce processus, consultez [Simplification de la création d’informations d’identification SQL avec des jetons de signature d’accès partagé (SAS) sur le stockage Azure avec Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell). Le jeton SAS peut être stocké dans des **informations d’identification SQL** pour une utilisation avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|**SQL Server Agent**|L’Agent SQL Server doit être en cours d’exécution pour que [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] fonctionne. Envisagez de définir l’option de démarrage sur automatique.|  
  
## <a name="components"></a>Components  
 Transact-SQL est l'interface principale utilisée pour interagir avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Les procédures stockées système sont utilisées pour activer, configurer et surveiller la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Les fonctions système sont utilisées pour récupérer des paramètres de configuration existants, des valeurs de paramètre et des informations sur le fichier de configuration. Les événements étendus sont utilisés pour exposer des erreurs et des avertissements. Les mécanismes d'alerte sont activés via les travaux SQL Agent et la gestion basées sur des stratégies SQL Server. Voici la liste des objets et la description de leurs fonctionnalités en relation à la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Des applets de commande PowerShell sont également disponibles pour configurer la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. SQL Server Management Studio prend en charge la restauration de sauvegardes créées par la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] à l'aide de la tâche **Restaurer la base de données** .  
  
|Objet système|Description|  
|-|-|  
|**MSDB**|Stocke les métadonnées et l'historique de toutes les sauvegardes créées par la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)|Active la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_backup_config_advanced &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)|Configure les paramètres avancés pour la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)], telles que le chiffrement.|  
|[managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|Crée une planification personnalisée pour la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_ backup_master_switch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-master-switch-transact-sql.md)|Interrompt et reprend la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|[managed_backup.sp_set_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql.md)|Active et configure la surveillance pour la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Exemples : activer les événements étendus, les paramètres de courrier électronique pour les notifications.|  
|[managed_backup.sp_backup_on_demand &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-on-demand-transact-sql.md)|Effectue une sauvegarde ad hoc d’une base de données qui est activée pour utiliser la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sans rompre la séquence de journaux de transactions consécutifs.|  
|[managed_backup.fn_backup_db_config &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-backup-db-config-transact-sql.md)|Retourne l’état actuel de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] et les valeurs de configuration d’une base de données, ou de toutes les bases de données sur l’instance.|  
|[managed_backup.fn_is_master_switch_on &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-is-master-switch-on-transact-sql.md)|Retourne l’état du commutateur principal.|  
|[managed_backup.sp_get_backup_diagnostics &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql.md)|Retourne les événements enregistrés par Événements étendus.|  
|[managed_backup.fn_get_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-parameter-transact-sql.md)|Retourne les valeurs actuelles des paramètres système de la sauvegarde comme les paramètres de surveillance et de courrier électronique pour les alertes.|  
|[managed_backup.fn_available_backups &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-available-backups-transact-sql.md)|Récupère les sauvegardes disponibles pour une base de données spécifiée ou pour toutes les bases de données dans une instance.|  
|[managed_backup.fn_get_current_xevent_settings &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-current-xevent-settings-transact-sql.md)|Retourne les paramètres actuels des événements étendus.|  
|[managed_backup.fn_get_health_status &#40;Transact-SQL&#41;](../../relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql.md)|Retourne le décompte agrégé des erreurs enregistrées par les événements étendus pour une période spécifiée.|  
  
## <a name="backup-strategy"></a>Stratégie de sauvegarde  
  
### <a name="backup-scheduling"></a>Planification de la sauvegarde  
 Vous pouvez spécifier une planification de sauvegarde personnalisée à l’aide de la procédure stockée système [managed_backup.sp_backup_config_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md). Si vous ne spécifiez pas de planification personnalisée, le type des sauvegardes planifiées et leur fréquence sont déterminés en fonction de la charge de travail de la base de données. Les paramètres de période de rétention sont utilisés pour déterminer la période pendant laquelle un fichier de sauvegarde doit être retenu dans le stockage et la capacité à restaurer une base de données à un point précis dans le temps au cours de la période de rétention.  
  
### <a name="backup-file-naming-conventions"></a>Conventions d’affectation de noms aux fichiers de sauvegarde  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] utilise le conteneur que vous spécifiez, donc vous pouvez contrôler le nom du conteneur. Pour les fichiers de sauvegarde, les bases de données autres que des bases de données de disponibilité sont nommées selon la convention suivante : le nom est créé à l’aide des 40 premiers caractères du nom de la base de données, le GUID de la base de données sans ‘-‘, et l’horodatage. Le caractère de soulignement est inséré entre les segments comme délimiteurs. L'extension **.bak** est utilisée pour le fichier en cas de sauvegarde complète et l'extension **.log** est utilisée pour les sauvegardes de journal. Pour les bases de données d’un groupe de disponibilité, en plus de la convention de nommage décrite ci-dessus, le GUID de la base de données du groupe de disponibilité est ajouté à la suite des 40 caractères du nom de la base de données. La valeur du GUID de la base de données du groupe de disponibilité est la valeur de group_database_id dans sys.databases.  
  
### <a name="full-database-backup"></a>Sauvegarde de base de données complète  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] planifie une sauvegarde de base de données complète si l'une des conditions suivantes est vraie.  
  
-   La [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée sur une base de données pour la première fois, ou la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée avec les paramètres par défaut au niveau de l'instance.  
  
-   La taille du journal depuis la dernière sauvegarde de base de données complète est égale ou supérieure à 1 Go.  
  
-   L'intervalle maximum d'une semaine est dépassé depuis la dernière sauvegarde de base de données complète.  
  
-   La séquence de journaux de transactions consécutifs est rompue. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] vérifie périodiquement si la séquence de journaux de transactions consécutifs est intacte en comparant le premier et le dernier LSN des fichiers de sauvegarde. Si la séquence de journaux de transactions consécutifs est rompue pour un motif quelconque, la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] planifie une sauvegarde de base de données complète. Le motif le plus fréquent d'une rupture de la séquence de journaux de transactions consécutifs est le plus souvent une commande de sauvegarde émise à l'aide de Transact-SQL ou via la tâche de sauvegarde dans SQL Server Management Studio.  D'autres scénarios communs sont la suppression accidentelle des fichiers journaux de sauvegarde ou le remplacement accidentel des sauvegardes.  
  
### <a name="transaction-log-backup"></a>Sauvegarde du journal des transactions  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] planifie une sauvegarde du journal si l'une des conditions suivantes est vraie :  
  
-   L'historique des sauvegardes de journaux est introuvable. Cela se produit habituellement lorsque la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée la première fois.  
  
-   L'espace du journal des transactions utilisé dépasse 5 Mo.  
  
-   L'intervalle maximum de 2 heures est dépassé depuis la dernière sauvegarde de journal.  
  
-   Lorsque la sauvegarde du journal des transactions traîne derrière une sauvegarde complète de la base de données. Le but est de conserver la séquence de journaux de transactions consécutifs avant la sauvegarde complète.  
  
## <a name="retention-period-settings"></a>Paramètres de période de rétention  
 Lorsque vous configurez la sauvegarde, vous devez définir la période de rétention en jours : 1 jour minimum et 30 jours maximum.  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , en fonction des paramètres de la période de rétention, évalue la capacité à restaurer une base de données à un point précis dans le temps au cours de la période de rétention pour déterminer quels sont les fichiers de sauvegarde à conserver et quels sont ceux à supprimer. Le paramètre backup_finish_date de la sauvegarde est utilisé pour déterminer et vérifier la durée spécifiée dans les paramètres de la période de rétention.  
  
## <a name="important-considerations"></a>Éléments importants à prendre en considération  
 Pour une base de données, si un travail de sauvegarde complète de base de données est en cours, la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] attend la fin du travail avant d'effectuer une autre sauvegarde complète de la même base de données. De même, une seule sauvegarde de journal de transactions peut être exécutée à la fois. Toutefois, une sauvegarde de base de données complète et une sauvegarde de journal peuvent s'exécuter simultanément. Les échecs sont enregistrés en tant qu'événements étendus.  
  
 Si plus de 10 sauvegardes de base de données complètes sont planifiées simultanément, un avertissement est généré au moyen du canal de débogage des événements étendus. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] met alors en file d'attente les bases de données qui restent à sauvegarder, jusqu'à ce que toutes les sauvegardes soient planifiées et terminées.  

> [!NOTE]
> La gestion de sauvegarde de SQL Server n’est pas prise en charge avec les serveurs proxy.
>
  
##  <a name="supportability"></a><a name="support_limits"></a> Prise en charge  
 Les considérations et les limitations suivantes relatives à la prise en charge sont spécifiques à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]:  
  
-   La sauvegarde de bases de données système **master**, **modèle** et **msdb** est prise en charge. La sauvegarde de **tempdb** n’est pas prise en charge. 
  
-   Pour [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], tous les modes de récupération sont pris en charge (complet, journalisé en bloc et simple).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] prend uniquement en charge les sauvegardes complètes de base de données et les sauvegardes de fichier journal. La sauvegarde automatique de fichier n'est pas prise en charge.  
  
-   Le service Stockage Blob Microsoft Azure est la seule option de stockage de sauvegarde prise en charge. Les sauvegardes sur disque ou sur bande ne sont pas prises en charge.  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] utilise la fonctionnalité Sauvegarde vers l’objet blob de blocs. La taille maximale d’un objet blob de blocs est de 200 Go. Mais en utilisant l’agrégation, la taille maximale d’une sauvegarde individuelle peut atteindre 12 To. Si vos besoins en matière de sauvegarde sont supérieurs, envisagez d’utiliser la compression et testez la taille du fichier de sauvegarde avant de configurer la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Pour le test, effectuez une sauvegarde sur un disque local ou une sauvegarde manuelle dans Microsoft Azure Storage à l’aide de l’instruction Transact-SQL **BACKUP TO URL** . Pour plus d’informations, consultez [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
-   [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] peut avoir d'autres limitations lorsqu'elle est configurée avec d'autres technologies prenant en charge la sauvegarde, la haute disponibilité ou la récupération d'urgence.  
  
## <a name="see-also"></a> Voir aussi  
- [Activer la gestion de sauvegarde de SQL Server sur Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)   
- [Configurer les options avancées pour la gestion de sauvegarde de SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)   
- [Désactiver la gestion de sauvegarde de SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/disable-sql-server-managed-backup-to-microsoft-azure.md)
- [Sauvegarde et restauration des bases de données système](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
  
