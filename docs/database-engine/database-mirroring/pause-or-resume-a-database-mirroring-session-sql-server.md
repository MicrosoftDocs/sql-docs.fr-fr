---
title: Suspendre et reprendre une session de mise en miroir de bases de données
description: Découvrez comment suspendre et reprendre une session de mise en miroir de bases de données SQL Server à l’aide de SQL Server Management Studio ou de T-SQL (Transact-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- resuming database mirroring
- database mirroring [SQL Server], sessions
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: 05ede3b4-6abe-4442-abb7-9f5aee1d6bc0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9e3348259a8fbfbdb59e0ce5939f6c3e95ee2419
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783335"
---
# <a name="pause-or-resume-a-database-mirroring-session-sql-server"></a>Suspendre ou reprendre une session de mise en miroir de bases de données (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment suspendre ou reprendre la mise en miroir de bases de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Sur ReplaceThisText, à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir suspendu ou repris la mise en miroir de bases de données](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
 Vous pouvez à tout moment suspendre une session de mise en miroir de bases de données afin d'améliorer les performances pendant les goulots d'étranglement. De même, vous pouvez reprendre une session interrompue à tout moment.  
  
> [!CAUTION]  
>  Après un service forcé, lorsque le serveur principal d'origine se reconnecte, la mise en miroir est suspendue. La reprise de la mise en miroir dans cette situation peut entraîner des pertes de données sur le serveur principal d'origine. Pour plus d’informations sur la gestion des problèmes éventuels de perte de données, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Pour suspendre ou reprendre une session de mise en miroir de bases de données, utilisez la page **Mise en miroir** de la boîte de dialogue Propriétés de la base de données.  
  
#### <a name="to-pause-or-resume-database-mirroring"></a>Pour suspendre ou reprendre la mise en miroir de bases de données  
  
1.  Lors d'une session de mise en miroir de bases de données, connectez-vous à l'instance du serveur principal, puis, dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer l'arborescence du serveur.  
  
2.  Développez le nœud **Bases de données** et sélectionnez la base de données.  
  
3.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Tâches**, puis cliquez sur **Miroir**. La page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** s'affiche.  
  
4.  Pour suspendre la session, cliquez sur **Suspendre**.  
  
     Vous êtes invité à confirmer l'opération ; si vous cliquez sur **Oui**, la session est suspendue et le bouton devient **Reprendre**.  
  
     Pour plus d’informations sur les effets de la suspension d’une session, consultez [Suspension et reprise de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md).  
  
5.  Pour reprendre la session, cliquez sur **Reprendre**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-pause-database-mirroring"></a>Pour suspendre la mise en miroir de bases de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de l'un des partenaires.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Soumettez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
     ALTER DATABASE *nom_base_de_données* SET PARTNER SUSPEND  
  
     où *nom_base_de_données* est la base de données en miroir dont vous voulez suspendre la session.  
  
     L'exemple suivant suspend l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER SUSPEND;  
    ```  
  
##### <a name="to-resume-database-mirroring"></a>Pour reprendre la mise en miroir de bases de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de l'un des partenaires.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Émettez l'instruction Transact-SQL suivante :  
  
     ALTER DATABASE *nom_base_de_données* SET PARTNER RESUME  
  
     où *nom_base_de_données* est la base de données en miroir dont vous voulez reprendre la session.  
  
     L'exemple suivant suspend l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET PARTNER RESUME;  
    ```  
  
##  <a name="follow-up-after-pausing-or-resuming-database-mirroring"></a><a name="FollowUp"></a> Suivi : Après avoir suspendu ou repris la mise en miroir de bases de données  
  
-   **Après avoir suspendu la mise en miroir de bases de données**  
  
     Sur la base de données primaire, prenez des précautions pour éviter la saturation du journal des transactions. Pour plus d'informations, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
-   **Après avoir repris la mise en miroir de bases de données**  
  
     La reprise de la mise en miroir de la base de données place la base de données miroir dans l'état Synchronisation. Si le niveau de sécurité est FULL, le miroir récupère le principal et la base de données miroir entre dans l'état Synchronisé. À ce stade, le basculement devient possible. Si le serveur témoin est présent et activé, le basculement automatique est possible. En l'absence de serveur témoin, le basculement manuel est possible.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Supprimer une mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
