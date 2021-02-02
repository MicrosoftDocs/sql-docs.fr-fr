---
title: Ajouter une base de données secondaire à une configuration de copie des journaux de transaction
description: Décrit comment ajouter une base de données secondaire à une configuration existante de la copie des journaux de transaction à l’aide de SQL Server Management Studio ou Transact-SQL dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- adding secondary databases
- secondary databases [SQL Server], in log shipping
- secondary data files [SQL Server], adding
- log shipping [SQL Server], secondary databases
ms.assetid: b02eba13-f8e6-4684-b7e4-75ea038ea473
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b5a1a3bde0e2dbccb7e74670826fb29905016e1
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813201"
---
# <a name="add-a-secondary-database-to-a-log-shipping-configuration-sql-server"></a>Ajouter une base de données secondaire dans une configuration de copie des journaux de transaction (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment ajouter une base de données secondaire à une configuration de copie des journaux de transaction dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les procédures stockées de copie des journaux de transaction nécessitent l’appartenance au rôle serveur fixe **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Pour ajouter une base de données secondaire pour la copie des journaux de transaction  
  
1.  Cliquez avec le bouton droit sur la base de données à utiliser en tant que base de données primaire dans la configuration de copie des journaux de transaction, puis sélectionnez **Propriétés**.  
  
2.  Sous **Sélectionner une page**, cliquez sur **Envoi de journaux de transaction**.  
  
3.  Sous **Instances de serveurs et bases de données secondaires**, cliquez sur **Ajouter**.  
  
4.  Cliquez sur **Se connecter** et connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser comme serveur secondaire.  
  
5.  Dans la zone **Base de données secondaire** , choisissez une base de données dans la liste ou tapez le nom de la base de données que vous voulez créer.  
  
6.  Dans l'onglet **Initialiser la base de données secondaire** , choisissez l'option à utiliser pour initialiser la base de données secondaire.  
  
7.  Dans l' **onglet Copier les fichiers**, dans la zone **Dossier de destination des fichiers copiés** , entrez le chemin du dossier vers lequel vous voulez copier les sauvegardes des journaux des transactions. Ce dossier se situe généralement sur le serveur secondaire.  
  
8.  Notez la planification de la copie figurant dans la zone **Planification** sous **Copier le travail**. Si vous souhaitez personnaliser la planification de votre installation, cliquez sur **Planification** et ajustez ensuite la planification de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction de vos besoins. Cette planification doit être proche de la planification de la sauvegarde.  
  
9. Dans l’onglet **Restaurer** , sous **État de la base de données lors de la restauration des sauvegardes**, choisissez l’option **Aucun mode de récupération** ou **Mode veille** .  
  
10. Si vous choisissez l'option **Mode veille** , déterminez si vous voulez déconnecter des utilisateurs de la base de données secondaire pendant que l'opération de restauration est en cours.  
  
11. Si vous voulez retarder le processus de restauration sur le serveur secondaire, choisissez un délai sous **Retarder la restauration des sauvegardes d'au moins**.  
  
12. Choisissez un seuil d'alerte sous **Envoyer une alerte si aucune restauration ne se produit dans l'espace de**.  
  
13. Notez la planification de la restauration figurant dans la zone **Planification** sous **Restaurer le travail**. Si vous souhaitez personnaliser la planification de votre installation, cliquez sur **Planification** et ajustez ensuite la planification de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en fonction de vos besoins. Cette planification doit être proche de la planification de la sauvegarde.  
  
14. Cliquez sur **OK**.  
  
15. Cliquez sur **OK** dans la boîte de dialogue Propriétés de la base de données pour démarrer la procédure de configuration.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Pour ajouter une base de données secondaire pour la copie des journaux de transaction  
  
1.  Sur le serveur secondaire, exécutez [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) en fournissant les informations sur le serveur et la base de données principaux. Cette procédure stockée retourne l'ID secondaire et les ID des travaux de copie et de restauration.  
  
2.  Sur le serveur secondaire, exécutez [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) pour définir la planification des travaux de copie et de restauration.  
  
3.  Sur le serveur secondaire, exécutez [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) pour ajouter une base de données secondaire.  
  
4.  Sur le serveur principal, exécutez [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) pour ajouter les informations requises sur la nouvelle base de données secondaire au serveur principal.  
  
5.  Sur le serveur secondaire, activez les travaux de copie et de restauration. Pour plus d’informations, consultez [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Mise à niveau de la copie des journaux de transaction vers SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Supprimer une base de données secondaire dans une configuration de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Supprimer la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Afficher le rapport de la copie des journaux de transaction &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Surveiller la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
