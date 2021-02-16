---
title: Configurer la copie des journaux de transaction (SQL Server) | Microsoft Docs
description: Découvrez comment configurer la copie des journaux de transaction à l’aide de SQL Server Management Studio ou de Transact-SQL dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], enabling
- log shipping [SQL Server], configuring
ms.assetid: c42aa04a-4945-4417-b4c7-50589d727e9c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 519377db5732e17d3301f76a189405b2af11aaa7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348319"
---
# <a name="configure-log-shipping-sql-server"></a>Configurer la copie des journaux de transaction (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment configurer une copie des journaux de transaction dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et ses versions ultérieures prennent en charge la compression de la sauvegarde. Lorsque vous créez une configuration de copie des journaux de transaction, vous pouvez contrôler le comportement de compression des sauvegardes de fichiers journaux. Pour plus d’informations, consultez [Compression de sauvegardes &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Composants requis](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer la copie des journaux de transaction, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   La base de données primaire doit utiliser le mode de récupération complète ou de récupération utilisant les journaux de transactions ; le basculement de la base de données vers une récupération simple empêcherait toute copie des journaux de transaction.  
  
-   Avant de configurer la copie des journaux de transaction, vous devez créer un partage afin de rendre les sauvegardes de journaux de transactions disponibles pour le serveur secondaire. Il s'agit d'un partage du répertoire vers lequel les sauvegardes des journaux de transactions seront générées. Par exemple, si vous sauvegardez vos journaux de transactions dans le répertoire c:\data\tlogs\\, vous pouvez créer le partage \\\\*primaryserver*\tlogs de ce répertoire.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les procédures stockées de copie des journaux de transaction nécessitent l’appartenance au rôle serveur fixe **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-log-shipping"></a>Pour configurer la copie des journaux de transaction  
  
1.  Cliquez avec le bouton droit sur la base de données que vous voulez utiliser en tant que base de données primaire dans une configuration de copie des journaux de transaction, puis cliquez sur **Propriétés**.  
  
2.  Sous **Sélectionner une page**, cliquez sur **Envoi de journaux de transaction**.  
  
3.  Activez la case à cocher **Activer en tant que base de données primaire dans une configuration de la copie des journaux de transactions** .  
  
4.  Sous **Sauvegardes des journaux de transactions**, cliquez sur **Paramètres de sauvegarde**.  
  
5.  Dans la zone **Chemin d'accès réseau au dossier de sauvegarde** , tapez le chemin d'accès réseau vers le partage que vous avez créé pour le dossier de sauvegarde des journaux de transactions.  
  
6.  **Si le dossier de sauvegarde se trouve sur le serveur principal, tapez un chemin d’accès local au dossier dans la boîte de dialogue**  du dossier de sauvegarde. Si le dossier de sauvegarde n'est pas situé sur le serveur principal, vous pouvez laisser cette zone vide.  
  
    > [!IMPORTANT]  
    >  Si le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur votre serveur principal est exécuté sous le compte système local, vous devez créer votre dossier de sauvegarde sur le serveur principal et spécifier un chemin d'accès local vers ce dossier.  
  
7.  Configurez les paramètres **Supprimer les fichiers antérieurs à** et **Envoyer une alerte si aucune sauvegarde ne se produit dans l'espace de** .  
  
8.  Notez la planification de la sauvegarde figurant dans la zone **Planification** sous **Travail de sauvegarde**. Si vous souhaitez personnaliser la planification pour votre installation, cliquez ensuite sur **Planification** et ajustez la planification de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction de vos besoins.  
  
9. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prend en charge la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Lorsque vous créez une configuration de copie des journaux de transaction, vous pouvez contrôler le comportement de compression des sauvegardes de fichiers journaux grâce à l’une des options suivantes : **Utiliser le paramètre de serveur par défaut**, **Compresser la sauvegarde**, ou **Ne pas compresser la sauvegarde**. Pour plus d’informations, consultez [Log Shipping Transaction Log Backup Settings](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md).  
  
10. Cliquez sur **OK**.  
  
11. Sous **Instances de serveurs et bases de données secondaires**, cliquez sur **Ajouter**.  
  
12. Cliquez sur **Se connecter** et connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser comme serveur secondaire.  
  
13. Dans la zone **Base de données secondaire** , choisissez une base de données dans la liste ou tapez le nom de la base de données que vous voulez créer.  
  
14. Dans l'onglet **Initialiser la base de données secondaire** , choisissez l'option à utiliser pour initialiser la base de données secondaire.  
  
    > [!NOTE]  
    >  Si vous choisissez l’option suivant laquelle [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] initialise la base de données secondaire à partir d’une sauvegarde de la base de données, les fichiers de données et les fichiers journaux de la base de données secondaire sont placés au même emplacement que les fichiers de données et les fichiers journaux de la base de données **MASTER** . Il est probable que cet emplacement soit différent de celui des fichiers de données et des fichiers journaux de la base de données primaire.  
  
15. Sous l'onglet **Copier les fichiers** , dans la zone **Dossier de destination des fichiers copiés** , tapez le chemin d'accès du dossier dans lequel les sauvegardes des journaux de transactions doivent être copiées. Ce dossier se situe généralement sur le serveur secondaire.  
  
16. Notez la planification de la copie figurant dans la zone **Planification** sous **Copier le travail**. Si vous souhaitez personnaliser la planification de votre installation, cliquez sur **Planification** et ajustez ensuite la planification de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction de vos besoins. Cette planification doit être proche de la planification de la sauvegarde.  
  
17. Dans l’onglet **Restaurer** , sous **État de la base de données lors de la restauration des sauvegardes**, choisissez l’option **Aucun mode de récupération** ou **Mode veille** .  
    > [!IMPORTANT]  
    > L’option **Mode veille** n’est disponible que lorsque la version du serveur principal et celle du serveur secondaire sont identiques. Lorsque la version principale du serveur secondaire est supérieure à celle du serveur principal, seule l’option **Aucun mode de récupération** est autorisée.
  
18. Si vous choisissez l'option **Mode veille** , déterminez si vous voulez déconnecter des utilisateurs de la base de données secondaire pendant que l'opération de restauration est en cours.  
  
19. Si vous voulez retarder le processus de restauration sur le serveur secondaire, choisissez un délai sous **Retarder la restauration des sauvegardes d'au moins**.  
  
20. Choisissez un seuil d'alerte sous **Envoyer une alerte si aucune restauration ne se produit dans l'espace de**.  
  
21. Notez la planification de la restauration figurant dans la zone **Planification** sous **Restaurer le travail**. Si vous souhaitez personnaliser la planification de votre installation, cliquez sur **Planification** et ajustez ensuite la planification de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction de vos besoins. Cette planification doit être proche de la planification de la sauvegarde.  
  
22. Cliquez sur **OK**.  
  
23. Sous **Instance du serveur moniteur**, activez la case à cocher **Utiliser une instance du serveur moniteur** , puis cliquez sur **Paramètres**.  
  
    > [!IMPORTANT]  
    >  Pour analyser cette configuration d'envoi de journaux, vous devez ajouter maintenant le serveur moniteur. Pour ajouter le serveur moniteur ultérieurement, vous devrez supprimer cette configuration d'envoi de journaux, puis la remplacer par une nouvelle configuration qui inclut un serveur moniteur.  
  
24. Cliquez sur **Connecter** et connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous souhaitez utiliser en tant que serveur moniteur.  
  
25. Sous **Connexions du moniteur**, choisissez la méthode de connexion que devront utiliser les travaux de sauvegarde, copie et restauration pour se connecter au serveur moniteur.  
  
26. Sous **Rétention de l'historique**, choisissez la durée pendant laquelle vous voulez conserver un enregistrement de l'historique de copie des journaux de transactions.  
  
27. Cliquez sur **OK**.  
  
28. Dans la boîte de dialogue **Propriétés de la base de données** , cliquez sur **OK** pour démarrer le processus de configuration.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-log-shipping"></a>Pour configurer la copie des journaux de transaction  
  
1.  Initialisez la base de données secondaire en restaurant une sauvegarde complète de la base de données primaire sur le serveur secondaire.  
  
2.  Sur le serveur principal, exécutez [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md) pour ajouter une base de données primaire. La procédure stockée renvoie l'ID du travail de sauvegarde et l'ID primaire.  
  
3.  Sur le serveur principal, exécutez [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) pour ajouter une planification pour le travail de sauvegarde.  
  
4.  Sur le serveur moniteur, exécutez [sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md) pour ajouter le travail d’alerte.  
  
5.  Sur le serveur principal, activez le travail de sauvegarde.  
  
6.  Sur le serveur secondaire, exécutez [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) en fournissant les informations sur le serveur et la base de données principaux. Cette procédure stockée retourne l'ID secondaire et les ID des travaux de copie et de restauration.  
  
7.  Sur le serveur secondaire, exécutez [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) pour définir la planification des travaux de copie et de restauration.  
  
8.  Sur le serveur secondaire, exécutez [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) pour ajouter une base de données secondaire.  
  
9. Sur le serveur principal, exécutez [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) pour ajouter les informations requises sur la nouvelle base de données secondaire au serveur principal.  
  
10. Sur le serveur secondaire, activez les travaux de copie et de restauration. Pour plus d’informations, consultez [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Mise à niveau de la copie des journaux de transaction vers SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Ajouter une base de données secondaire dans une configuration de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Supprimer une base de données secondaire dans une configuration de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Supprimer la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Afficher le rapport de la copie des journaux de transaction &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Surveiller la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
