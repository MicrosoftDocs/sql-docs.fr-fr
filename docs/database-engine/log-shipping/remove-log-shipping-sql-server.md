---
title: Supprimer la copie des journaux de transaction (SQL Server) | Microsoft Docs
description: Découvrez comment supprimer la copie des journaux de transaction à l’aide de SQL Server Management Studio ou de Transact-SQL dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], removing
- removing log shipping
- deleting log shipping
ms.assetid: 859373db-c744-4a4b-8479-45163f61e8cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8994d1661c46d20bf1b1d82ab35af3ad4b1b581
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346961"
---
# <a name="remove-log-shipping-sql-server"></a>Supprimer la copie des journaux de transaction (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment supprimer la copie des journaux de transaction dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer la copie des journaux de transaction, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les procédures stockées de copie des journaux de transaction nécessitent l’appartenance au rôle serveur fixe **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-remove-log-shipping"></a>Pour supprimer l'envoi de journaux  
  
1.  Connectez-vous à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est actuellement le serveur principal de copie des journaux de transaction et développez-la.  
  
2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données primaire de copie des journaux de transaction, puis cliquez sur **Propriétés**.  
  
3.  Sous **Sélectionner une page**, cliquez sur **Envoi de journaux de transaction**.  
  
4.  Désactivez la case à cocher **Activer en tant que base de données primaire dans une configuration de la copie des journaux de transactions** .  
  
5.  Cliquez sur **OK** pour supprimer l'envoi de journaux depuis cette base de données primaire.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-remove-log-shipping"></a>Pour supprimer l'envoi de journaux  
  
1.  Sur le serveur principal de copie des journaux de transaction, exécutez [sp_delete_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md) pour supprimer du serveur principal les informations relatives à la base de données secondaire.  
  
2.  Sur le serveur secondaire de copie des journaux de transaction, exécutez [sp_delete_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md) pour supprimer la base de données secondaire.  
  
    > [!NOTE]  
    >  Si aucune autre base de données secondaire portant le même ID secondaire n’existe, **sp_delete_log_shipping_secondary_primary** est appelé par **sp_delete_log_shipping_secondary_database** et supprime l’entrée de l’ID secondaire, ainsi que les travaux de copie et de restauration.  
  
3.  Sur le serveur principal de copie des journaux de transaction, exécutez **sp_delete_log_shipping_primary_database** pour supprimer du serveur principal les informations relatives à la configuration d’envoi de journaux. Le travail de sauvegarde est également supprimé.  
  
4.  Sur le serveur principal de copie des journaux de transaction, désactivez le travail de sauvegarde. Pour plus d’informations, consultez [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
5.  Sur le serveur secondaire de copie des journaux de transaction, désactivez les travaux de copie et de restauration.  
  
6.  Éventuellement, si vous n'utilisez plus la base de données secondaire de copie des journaux de transaction, vous pouvez la supprimer du serveur secondaire.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Mise à niveau de la copie des journaux de transaction vers SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Ajouter une base de données secondaire dans une configuration de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Supprimer une base de données secondaire dans une configuration de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Surveiller la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md)  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
