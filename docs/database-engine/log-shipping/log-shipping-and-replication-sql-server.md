---
title: Copie des journaux de transaction et réplication (SQL Server) | Microsoft Docs
description: Découvrez comment la copie des journaux de transaction répercute dans la base de données secondaire chaque insertion, mise à jour ou suppression apportée à la base de données primaire.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], log shipping and
- log shipping [SQL Server], replication and
ms.assetid: 132bebfd-0206-4d23-829a-b38e5ed17bc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96f64ac1ade48962f5f8dd58f0af6f0a7f662b5c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346968"
---
# <a name="log-shipping-and-replication-sql-server"></a>Copie des journaux de transaction et réplication (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La copie des journaux de transaction consiste à avoir deux exemplaires d'une même base de données qui résident généralement sur des ordinateurs différents. À un moment donné précis, les clients ne peuvent accéder qu'à un seul exemplaire de la base de données. Cet exemplaire s'appelle la base de données primaire. Les mises à jour apportées par les clients à la base de données primaire sont répercutées, par copie des journaux de transaction, à l'autre exemplaire de la base de données appelé base de données secondaire. La copie des journaux de transaction consiste à répercuter dans la base de données secondaire chaque insertion, mise à jour ou suppression apportée à la base de données primaire.  
  
 La copie des journaux de transaction peut intervenir en parallèle à la réplication selon les principes suivants :  
  
-   La réplication s'arrête après le basculement de la copie des journaux de transaction. Si un basculement se produit, les agents de réplication ne se connectent pas à la base de données secondaire, ainsi les transactions ne sont pas répliquées sur les Abonnés. Si une restauration automatique de la base de données primaire a lieu, la réplication reprend. Toutes les transactions copiées à l'aide des journaux de transactions de la base de données secondaire vers la base de données primaire sont répliquées sur les Abonnés.  
  
-   Si la base de données primaire est définitivement perdue, la base de données secondaire peut être renommée afin que la réplication puisse se poursuivre. Le reste de cette rubrique décrit les conditions requises et les procédures à suivre pour gérer cette situation. L'exemple concerne la base de données de publication, qui est la base de données la plus fréquemment utilisée pour la copie des journaux de transaction, mais un processus similaire peut être appliqué aux bases de données d'abonnement et de distribution.  
  
 Pour plus d’informations sur la récupération des bases de données impliquées dans la réplication sans avoir à reconfigurer la réplication, consultez [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
> [!NOTE]  
>  Nous vous recommandons d'utiliser la mise en miroir des bases de données plutôt que la copie des journaux de transaction pour assurer la disponibilité de la base de données de publication. Pour plus d’informations, consultez [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
## <a name="requirements-and-procedures-for-replicating-from-the-secondary-if-the-primary-is-lost"></a>Conditions requises et procédures à suivre pour assurer une réplication à partir de la base de données secondaire en cas de perte de la base de données primaire  
 Tenez compte des exigences et des points suivants :  
  
-   Si une base de données primaire contient plusieurs bases de données de publication, copiez les journaux de toutes les bases de données de publication sur la même base de données secondaire.  
  
-   Le chemin d'installation de l'instance du serveur secondaire doit être identique à celui du serveur principal. Les emplacements des bases de données utilisateur sur le serveur secondaire doivent être identiques à ceux du serveur principal.  
  
-   Sauvegardez la clé principale du service sur le serveur principal. Cette clé sera restaurée sur le serveur secondaire. Pour plus d’informations, consultez [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md).  
  
-   La copie des journaux de transaction ne vous protège pas contre les pertes de données. Une défaillance de la base de données primaire peut entraîner la perte des données qui n'ont pas encore été sauvegardées ou des sauvegardes égarées suite à la défaillance.  
  
### <a name="log-shipping-with-transactional-replication"></a>Copie des journaux de transaction et réplication transactionnelle  
 En cas de réplication transactionnelle, le comportement de la copie des journaux de transaction dépend de l’option **sync with backup** . Cette option ne peut pas être définie dans les bases de données de publication et de distribution. En cas de copie des journaux de transaction pour le serveur de publication, seuls les paramètres de la base de données de publication s'appliquent.  
  
 L'activation de cette option garantit que les transactions ne sont pas remises à la base de données de distribution tant qu'elles ne sont pas sauvegardées dans la base de données de publication. La dernière sauvegarde de la base de données de publication peut alors être restaurée sur le serveur secondaire sans que la base de distribution possède des transactions que la base de données de publication n'aurait pas. Cette option permet aussi, en cas de basculement du serveur de publication sur un serveur secondaire, d'assurer la cohérence entre le serveur de publication, le serveur de distribution et les Abonnés. La latence et le débit sont affectés puisque les transactions ne peuvent pas être fournies à la base de données de distribution sans avoir été sauvegardées au préalable sur le serveur de publication. Si votre application tolère cette latence, nous vous recommandons d'activer cette option sur la base de données de publication. Si l’option **sync with backup** n’est pas activée, les Abonnés risquent de recevoir des modifications qui ne figurent plus dans la base de données restaurée au niveau du serveur secondaire. Pour plus d’informations, voir [Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d'instantané](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 **Pour configurer la réplication transactionnelle et la copie des journaux de transaction avec l'option sync with backup**  
  
1.  Si l'option sync with backup n'est pas activée sur la base de données de publication, exécutez `sp_replicationdboption '<publicationdatabasename>', 'sync with backup', 'true'`. Pour plus d’informations, consultez [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
2.  Configurez la copie des journaux de transaction pour la base de données de publication. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
3.  Lorsque le serveur de publication tombe en panne, restaurez le dernier journal de la base de données sur le serveur secondaire en utilisant l'option KEEP_REPLICATION de RESTORE LOG. De cette façon, vous conservez tous les paramètres de réplication pour la base de données. Pour plus d’informations, consultez [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) et [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Restaurez la base de données **msdb** et les bases de données **master** du serveur principal vers le serveur secondaire. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si le serveur principal était également un serveur de distribution, restaurez la base de données de distribution du serveur principal sur le serveur secondaire.  
  
     Ces bases de données doivent être cohérentes avec la base de données de publication au niveau du serveur principal en termes de configuration et de paramètres de la réplication.  
  
5.  Au niveau du serveur secondaire, renommez l'ordinateur, puis l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'après le nom du serveur principal. Pour plus d'informations sur la modification du nom de l'ordinateur, consultez la documentation Windows. Pour plus d’informations sur le renommage du serveur, consultez [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) et [Renommer une instance de cluster de basculement SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
6.  Au niveau du serveur secondaire, restaurez la clé principale du service qui a été sauvegardée depuis le serveur principal. Pour plus d’informations, consultez [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
 **Pour configurer la réplication transactionnelle et la copie des journaux de transaction sans l'option sync with backup**  
  
1.  Configurez la copie des journaux de transaction pour la base de données de publication. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  Lorsque le serveur de publication tombe en panne, restaurez le dernier journal de la base de données sur le serveur secondaire en utilisant l'option KEEP_REPLICATION de RESTORE LOG. De cette façon, vous conservez tous les paramètres de réplication pour la base de données. Pour plus d’informations, consultez [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) et [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
3.  Restaurez la base de données **msdb** et les bases de données **master** du serveur principal vers le serveur secondaire. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si le serveur principal était également un serveur de distribution, restaurez la base de données de distribution du serveur principal sur le serveur secondaire.  
  
     Ces bases de données doivent être cohérentes avec la base de données de publication au niveau du serveur principal en termes de configuration et de paramètres de la réplication.  
  
4.  Au niveau du serveur secondaire, renommez l'ordinateur, puis l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'après le nom du serveur principal. Pour plus d'informations sur la modification du nom de l'ordinateur, consultez la documentation Windows. Pour plus d’informations sur le renommage du serveur, consultez [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) et [Renommer une instance de cluster de basculement SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
     Vous pouvez recevoir un message d'erreur de l'Agent de lecture du journal indiquant que la base de données de publication et la base de données de distribution ne sont pas synchronisées.  
  
5.  Au niveau du serveur secondaire, restaurez la clé principale du service qui a été sauvegardée depuis le serveur principal. Pour plus d’informations, consultez [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Exécutez **sp_replrestart**. Cette procédure stockée permet d'obliger l'Agent de lecture du journal à ignorer toutes les transactions répliquées précédentes dans le journal de la base de données de publication. Les transactions appliquées après l'exécution de la procédure stockée sont traitées par l'Agent de lecture du journal. Pour plus d’informations, consultez [sp_replrestart &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md).  
  
7.  Redémarrez l'Agent de lecture du journal après exécution correcte de la procédure stockée. Pour plus d’informations, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
8.  Les transactions qui ont déjà été distribuées à l'Abonné peuvent être appliquées au serveur de publication. Pour être sûr que l’agent de distribution poursuivra le traitement en dépit d’une erreur rencontrée lors de la réapplication des transactions sur un Abonné, spécifiez le profil de l’agent intitulé **Continuer avec les erreurs de cohérence des données**.  
  
### <a name="log-shipping-with-merge-replication"></a>Copie des journaux de transaction et réplication de fusion  
 Suivez les étapes de la procédure ci-dessous pour configurer la réplication de fusion et la copie des journaux de transaction.  
  
 **Pour configurer la réplication de fusion et la copie des journaux de transaction**  
  
1.  Configurez la copie des journaux de transaction pour la base de données de publication. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
2.  Si le serveur de publication tombe en panne, au niveau du serveur secondaire, renommez l'ordinateur, puis l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'après le nom du serveur principal. Pour plus d'informations sur la modification du nom de l'ordinateur, consultez la documentation Windows. Pour plus d’informations sur le renommage du serveur, consultez [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) et [Renommer une instance de cluster de basculement SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).  
  
3.  Restaurez le dernier journal de la base de données sur le serveur secondaire en utilisant l'option KEEP_REPLICATION de RESTORE LOG. De cette façon, vous conservez tous les paramètres de réplication pour la base de données. Pour plus d’informations, consultez [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) et [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
4.  Restaurez la base de données **msdb** et les bases de données **master** du serveur principal vers le serveur secondaire. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). Si le serveur principal était également un serveur de distribution, restaurez la base de données de distribution du serveur principal sur le serveur secondaire.  
  
     Ces bases de données doivent être cohérentes avec la base de données de publication au niveau du serveur principal en termes de configuration et de paramètres de la réplication.  
  
5.  Au niveau du serveur secondaire, restaurez la clé principale du service qui a été sauvegardée depuis le serveur principal. Pour plus d’informations, consultez [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
6.  Synchronisez la base de données de publication avec une ou plusieurs bases de données d'abonnement. Cela vous permet de télécharger les modifications qui ont été apportées précédemment à la base de données de publication mais qui ne figurent pas dans la sauvegarde restaurée. Les données qui peuvent être chargées dépendent de la façon dont une publication est filtrée :  
  
    -   Si la publication n'est pas filtrée, vous devriez pouvoir mettre à jour la base de données de publication en la synchronisant avec l'Abonné le plus à jour.  
  
    -   Si la publication est filtrée, il est possible que vous ne puissiez pas mettre à jour la base de données de publication. Prenez une table qui est partitionnée de telle façon que chaque abonnement reçoit des données client seulement pour une région : Nord, Est, Sud et Ouest. S'il y a au moins un Abonné pour chaque partition de données, la synchronisation avec un Abonné pour chaque partition doit permettre la mise à jour de la base de données de publication. Cependant, si par exemple des données de la partition Ouest n'ont été répliquées vers aucun des Abonnés, ces données ne peuvent pas être mises à jour au niveau du serveur de publication. Dans ce cas, nous vous conseillons de réinitialiser tous les abonnements de façon à ce que les données du serveur de publication et des Abonnés convergent. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
     Si la synchronisation se fait avec un Abonné exécutant une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], l'abonnement ne peut pas être anonyme. Il doit s'agir d'un abonnement client ou d'un abonnement serveur (appelées abonnement local et abonnement global dans les versions précédentes). Pour plus d’informations, consultez [Synchroniser les données](../../relational-databases/replication/synchronize-data.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
  
