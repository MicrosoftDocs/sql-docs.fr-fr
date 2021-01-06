---
title: Changer les rôles de copie des journaux de transaction entre base de données primaire et base de données secondaire
description: Découvrez comment configurer votre base de données secondaire pour qu’elle joue le rôle de base de données primaire dans le cadre de votre solution de copie des journaux de transaction SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], role changes
- secondary data files [SQL Server], roles changed between
- primary databases [SQL Server]
- initial role changes [SQL Server]
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: 2d7cc40a-47e8-4419-9b2b-7c69f700e806
author: cawrites
ms.author: chadam
ms.openlocfilehash: 927ea18c7644d18e4dcf2a094e9ba0f962dcbd54
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644446"
---
# <a name="change-roles-between-primary-and-secondary-log-shipping-servers-sql-server"></a>Changer des rôles entre les serveurs primaire et secondaire de copie des journaux de transaction (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Après avoir basculé une configuration de copie des journaux de transactions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un serveur secondaire, vous pouvez configurer votre base de données secondaire de façon à ce qu'elle agisse en tant que base de données primaire. Vous pourrez alors intervertir les bases de données primaire et secondaire en fonction des besoins.  
  
## <a name="performing-the-initial-role-change"></a>Exécution du changement de rôle initial  
 La première fois que vous voulez basculer vers la base de données secondaire et en faire votre base de données primaire, vous devez effectuer un ensemble d'opérations. Après cela, vous pourrez intervertir facilement les rôles des bases de données primaire et secondaire.  
  
1.  Basculez manuellement de la base de données primaire vers la base de données secondaire. Vérifiez que vous avez sauvegardé le journal des transactions en cours sur votre serveur principal en utilisant l'option NORECOVERY. Pour plus d’informations, consultez [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md).  
  
2.  Désactivez l'opération de copie des journaux de transaction sur le serveur principal, ainsi que les opérations de copie et de restauration sur le serveur secondaire d'origine.  
  
3.  Dans la base de données secondaire (que vous voulez transformer en base de données primaire), configurez la copie des journaux de transaction au moyen de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md). Incorporez les étapes suivantes :  
  
    1.  Utilisez le même partage pour la création des sauvegardes que vous avez créées pour le serveur principal d'origine.  
  
    2.  Lorsque vous ajoutez la base de données secondaire, dans la boîte de dialogue **Paramètres de base de données secondaire** , tapez le nom de la base de données primaire dans la zone **Base de données secondaire** .  
  
    3.  Dans la boîte de dialogue **Paramètres de base de données secondaire** , sélectionnez **Non, la base de données secondaire est initialisée**.  
  
4.  Si l'analyse de l'envoi de journaux est activée sur la configuration de copie des journaux de transaction précédente, reconfigurez l'analyse de l'envoi de journaux pour surveiller la nouvelle configuration de copie des journaux de transaction.  L’affectation de la valeur 1 à threshold_alert_enabled spécifie qu’une alerte est déclenchée en cas de dépassement de restore_threshold. Exécutez les commandes suivantes, en remplaçant *nom_base_de_données* par le nom de votre base de données :  
  
    1.  **Sur le nouveau serveur principal**  
  
         Exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
        ```sql  
        -- Statement to execute on the new primary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_secondary_database @secondary_database = N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
    2.  **Sur le nouveau serveur secondaire**  
  
         Exécutez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
        ```sql  
        -- Statement to execute on the new secondary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_primary_database @database=N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
## <a name="swapping-roles"></a>Interversion des rôles  
 Une fois les opérations ci-dessus effectuées pour le changement initial des rôles, vous pouvez intervertir les rôles des bases de données primaire et secondaire en effectuant les opérations de cette section. Pour changer les rôles, effectuez ces opérations générales :  
  
1.  Connectez la base de données secondaire, en sauvegardant le journal des transactions du serveur principal avec l'option NORECOVERY.  
  
2.  Désactivez l'opération de copie des journaux de transaction sur le serveur principal, ainsi que les opérations de copie et de restauration sur le serveur secondaire d'origine.  
  
3.  Activez l'opération de copie des journaux de transaction sur le serveur secondaire (nouveau serveur principal, ainsi que les opérations de copie et de restauration sur le serveur principal (nouveau serveur secondaire).  
  
> [!IMPORTANT]  
>  Lorsque vous modifiez une base de données secondaire en base de données primaire, pour garantir une expérience cohérente aux utilisateurs et aux applications, vous devrez peut-être recréer tout ou partie des métadonnées de la base de données, telles que les connexions et les travaux, sur la nouvelle instance de serveur principal. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Basculer vers un serveur secondaire d’envoi de journaux &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et procédures stockées liées à la copie des journaux de transaction](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
