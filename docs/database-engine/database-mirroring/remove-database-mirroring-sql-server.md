---
title: Supprimer la mise en miroir de bases de données (SQL Server) | Microsoft Docs
description: Découvrez comment supprimer une mise en miroir de bases de données à partir d’une base de données à l’aide de SQL Server Management Studio ou de Transact-SQL dans SQL Server.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- removing database mirroring [SQL Server]
ms.assetid: bbc4d7f7-3bc7-40d6-a822-af195fe7f8c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d1e09227bd8f3334e2dfea5cce81dcb96857247
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783708"
---
# <a name="remove-database-mirroring-sql-server"></a>Supprimer la mise en miroir des bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment supprimer la mise en miroir de bases de données depuis une base de données de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  À tout moment, le propriétaire d'une base de données peut arrêter manuellement une session de mise en miroir de bases de données en supprimant la mise en miroir de la base de données.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une mise en miroir de bases de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir supprimé une mise en miroir de bases de données](#FollowUp)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-remove-database-mirroring"></a>Pour supprimer une mise en miroir de bases de données  
  
1.  Lors d'une session de mise en miroir de bases de données, connectez-vous à l'instance du serveur principal, puis, dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer l'arborescence du serveur.  
  
2.  Développez le nœud **Bases de données** et sélectionnez la base de données.  
  
3.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Tâches**, puis cliquez sur **Miroir**. La page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** s'affiche.  
  
4.  Dans le volet **Sélectionner une page** , cliquez sur **Mise en miroir**.  
  
5.  Pour supprimer une mise en miroir, cliquez sur **Supprimer la mise en miroir**. Vous êtes invité à confirmer l'opération. Si vous cliquez sur **Oui**, la session s'arrête et la mise en miroir est supprimée de la base de données.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Pour supprimer une mise en miroir de bases de données, utilisez la page **Propriétés de la base de données**. Utilisez la page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** .  
  
#### <a name="to-remove-database-mirroring"></a>Pour supprimer une mise en miroir de bases de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de l'un des partenaires de mise en miroir.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Soumettez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    ALTER DATABASE database_name SET PARTNER OFF  
    ```  
  
     où *nom_base_de_données* représente la base de données en miroir dont vous voulez supprimer la session.  
  
     L'exemple suivant supprime la mise en miroir de bases de données de l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER OFF;  
    ```  
  
##  <a name="follow-up-removing-database-mirroring"></a><a name="FollowUp"></a> Suivi : Suppression d'une mise en miroir des bases de données  
  
> [!NOTE]  
>  Pour plus d’informations sur l’impact de la suppression d’une mise en miroir de bases de données, consultez [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   **Si vous envisagez de redémarrer la mise en miroir sur la base de données**  
  
     Toutes les sauvegardes du journal effectuées sur la base de données principale après la suppression de la mise en miroir doivent être toutes appliquées à la base de données miroir avant de pouvoir redémarrer la mise en miroir.  
  
-   **Si vous n’envisagez pas de redémarrer la mise en miroir**  
  
     Vous avez la possibilité de récupérer la base de données miroir initiale. Sur l'instance de serveur qui était le serveur miroir, vous pouvez utiliser l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    RESTORE DATABASE database_name WITH RECOVERY;  
    ```  
  
    > [!IMPORTANT]  
    >  Si vous récupérez cette base de données, deux bases de données divergentes portant le même nom se trouvent alors en ligne. Vous devez par conséquent vérifier que les clients ne peuvent accéder qu’à une seule d’entre elles, en général la base de données principale la plus récente.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Suspendre ou reprendre une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
-   [Supprimer le témoin d’une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-the-witness-from-a-database-mirroring-session-sql-server.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Exemple : Configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
