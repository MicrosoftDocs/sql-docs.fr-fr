---
title: Mettre à niveau la copie des journaux de transaction vers SQL Server 2016 ou des versions ultérieures
description: Découvrez la séquence à suivre pour préserver votre solution de reprise d’activité de copie des journaux de transaction lors de la mise à niveau vers SQL Server 2016 ou des versions ultérieures à partir d’une version précédente.
ms.custom: seo-lt-2019
ms.date: 02/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], upgrading
ms.assetid: b1289cc3-f5be-40bb-8801-0e3eed40336e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b3e98cc2f5ce76093f3710f6f49e1b186219c04
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643842"
---
# <a name="upgrading-log-shipping-to-sql-server-2016-transact-sql"></a>Mise à niveau de la copie des journaux de transaction vers SQL Server 2016 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lors de la mise à niveau d’une configuration de la copie des journaux de transaction de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une nouvelle version de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], d’un nouveau Service Pack [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d’une mise à jour cumulative de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la mise à niveau de vos serveurs de copie des journaux de transaction dans le bon ordre permettra de préserver votre solution de reprise d’activité de la copie des journaux de transaction.  
  
> [!NOTE]  
>  [La compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) a été introduite dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]. Une configuration mise à niveau de copie des journaux de transaction utilise l’option de configuration du niveau serveur par défaut pour la **compression de sauvegarde** pour contrôler si la compression de la sauvegarde est utilisée pour les fichiers de sauvegarde du journal des transactions. Le comportement de la compression de la sauvegarde des sauvegardes de fichiers journaux peut être spécifié pour chaque configuration de la copie des journaux de transaction. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
 **Dans cette rubrique :**  
  
-   [Prérequis](#Prerequisites)  
  
-   [Protéger vos données avant la mise à niveau](#ProtectData)  
  
-   [Mise à niveau d'une instance du serveur moniteur](#UpgradeMonitor)  
  
-   [Mise à niveau des instances de serveur secondaires](#UpgradeSecondaries)  
  
-   [Mise à niveau de l'instance principale](#UpgradePrimary)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Avant de commencer, passez en revue les informations importantes suivantes :  
  
-   [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md): vérifiez que vous pouvez procéder à une mise à niveau vers SQL Server 2016 à partir de votre version du système d’exploitation Windows et de la version de SQL Server. Par exemple, vous ne pouvez pas mettre à niveau directement une instance SQL Server 2005 vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
-   [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): sélectionnez la méthode et les étapes de mise à niveau appropriées en fonction des versions et mises à niveau prises en charge ainsi que des autres composants installés dans votre environnement pour mettre à niveau les composants dans le bon ordre.  
  
-   [Planifier et tester le plan de mise à niveau du moteur de base de données](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): consultez les notes de version et les problèmes de mise à niveau connus, ainsi que la liste de contrôle préalable à la mise à niveau, puis développez et testez votre plan de mise à niveau.  
  
-   [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md): prenez connaissance de la configuration logicielle requise pour installer [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Si des logiciels supplémentaires sont nécessaires, installez-les sur chaque nœud avant de commencer le processus de mise à niveau pour réduire les éventuels temps d’arrêt.  
  
##  <a name="protect-your-data-before-the-upgrade"></a><a name="ProtectData"></a> Protéger vos données avant la mise à niveau  
 Comme méthode conseillée, nous vous recommandons de protéger vos données avant une mise à niveau de la copie des journaux de transaction.  
  
 **Pour protéger vos données**  
  
1.  Effectuez une sauvegarde complète de chaque base de données primaire.  
  
     Pour plus d’informations, consultez [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  Exécutez la commande [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur chaque base de données primaire.  
  
> [!IMPORTANT]  
>  Veillez à disposer de suffisamment d’espace sur votre serveur principal pour conserver les copies de sauvegarde du fichier journal pour la durée prévue de la durée de mise à niveau des serveurs secondaires.  Si vous basculez sur un serveur secondaire, ce même problème s'applique à ce serveur (le nouveau serveur principal).  
  
##  <a name="upgrading-the-optional-monitor-server-instance"></a><a name="UpgradeMonitor"></a> Mise à niveau (facultative) d'une instance du serveur moniteur  
 L'instance du serveur moniteur, si elle existe, peut être mise à niveau à tout moment. Cependant, il n’est pas nécessaire de mettre à niveau le serveur moniteur facultatif lorsque vous mettez à niveau le serveur principal et les serveurs secondaires.  
  
 Pendant que le serveur moniteur est mis à niveau, la configuration de la copie des journaux de transaction continue à fonctionner, mais son état n'est pas enregistré dans les tables sur le moniteur. Les alertes qui ont été configurées ne sont pas déclenchées tant que le serveur moniteur est en cours de mise à niveau. Après la mise à niveau, vous pouvez mettre à jour les informations dans les tables du moniteur en exécutant la procédure stockée système [sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md).   Pour plus d’informations sur les serveurs moniteurs, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
##  <a name="upgrading-the-secondary-server-instances"></a><a name="UpgradeSecondaries"></a> Mise à niveau des instances de serveur secondaires  
 Le processus de mise à niveau implique la mise à niveau des instances de serveur secondaire d'une configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] avant la mise à niveau de l'instance du serveur principal. Mettez toujours à niveau les instances secondaires de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en premier. La copie des journaux de transaction se poursuit durant le processus de mise à niveau parce que les instances de serveur secondaires mises à niveau continuent à restaurer les sauvegardes des journaux à partir de l’instance du serveur principal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si l’instance du serveur principal a été mise à niveau avant une instance de serveur secondaire, la copie des journaux de transaction échoue parce qu'une sauvegarde créée sur une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être restaurée sur une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez mettre à niveau les instances secondaires simultanément ou en série, mais toutes les instances secondaires doivent être mises à niveau avant l’instance principale, pour éviter un échec de copie des journaux de transaction.  
  
 Pendant qu’une instance de serveur secondaire est mise à niveau, les travaux de copie et de restauration de la copie des journaux de transaction ne s'exécutent pas. Cela signifie que les sauvegardes de journal des transactions s’accumulent sur le serveur principal et que vous devez disposer de suffisamment d’espace pour conserver ces sauvegardes non restaurées. Le niveau d’accumulation dépend de la fréquence des sauvegardes planifiées sur l’instance du serveur principal et de la séquence de mise à niveau des instances secondaires. De même, si un serveur moniteur séparé a été configuré, il se peut que des alertes soient déclenchées et indiquent que les restaurations n'ont pas été effectuées sur une durée supérieure à l'intervalle configuré.  
  
 Une fois que les instances serveur secondaires ont été mises à niveau, les travaux des agents de copie des journaux de transaction reprennent et poursuivent la copie et la restauration des sauvegardes de journal à partir de l'instance du serveur principal vers les instances de serveur secondaires. La durée nécessaire pour que les instances de serveur secondaires remettent la base de données secondaire à jour varie, selon la durée requise pour mettre à niveau l’instance de serveur secondaire et la fréquence des sauvegardes sur le serveur principal.  
  
> [!NOTE]  
>  Pendant la mise à niveau du serveur, la base de données secondaire elle-même n'est pas mise à niveau vers une base de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Il ne sera mis à niveau que s’il est mis en ligne en lançant un basculement de la base de données de copie des journaux de transaction. En théorie, cette condition peut perdurer indéfiniment. La durée nécessaire pour mettre à niveau les métadonnées de la base de données lors du lancement d’un basculement est faible.  
  
> [!IMPORTANT]  
>  L'option RESTORE WITH STANDBY n'est pas prise en charge pour une base de données qui requiert une mise à niveau. Si une base de données secondaire mise à niveau a été configurée à l'aide de RESTORE WITH STANDBY, les journaux des transactions ne peuvent plus être restaurés après la mise à niveau. Pour reprendre la copie des journaux de transaction sur cette base de données secondaire, vous devrez reconfigurer la copie des journaux de transaction sur ce serveur de secours. Pour plus d’informations sur l’option STANDBY, consultez [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="upgrading-the-primary-server-instance"></a><a name="UpgradePrimary"></a> Mise à niveau de l'instance du serveur principal  
 Étant donné que la copie des journaux de transaction est essentiellement une solution de récupération d’urgence, le scénario le plus simple et le plus courant est de mettre à niveau l’instance principale à la place, et de simplement laisser la base de données indisponible pendant cette mise à niveau. Une fois que le serveur est mis à niveau, la base de données est automatiquement remise en ligne, ce qui entraîne sa mise à niveau. Après que la base de données a été mise à niveau, les travaux de la copie des journaux de transaction reprennent.  
  
> [!NOTE]  
>  La copie des journaux de transaction de journaux permet aussi de [Basculer vers une base de données secondaire de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) et éventuellement de [Changer des rôles entre les serveurs primaire et secondaire de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md). Cependant, étant donné que la copie des journaux de transaction est rarement configurée en tant que solution à haute disponibilité de nos jours (les nouvelles options sont bien plus robustes), le basculement ne réduira généralement pas le temps d’arrêt, car les objets de base de données système ne sont pas synchronisés. De plus, permettre aux clients de localiser et se connecter facilement à un serveur secondaire promu peut être un calvaire.  
  
## <a name="see-also"></a>Voir aussi  
 [Effectuer une mise à niveau vers SQL Server 2016 à l’aide de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installer SQL Server 2016 à partir de l’invite de commandes](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)   
 [Surveiller la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)   
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
