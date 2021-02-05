---
description: Création d’un travail
title: Création d’un travail
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: b35af2b6-6594-40d1-9861-4d5dd906048c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2c577bb79487db6047c173ac331ed27fdf7ba3b2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237438"
---
# <a name="create-a-job"></a>Création d’un travail
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer un travail de l'Agent SQL Server dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)] ou de SQL Server Management Objects (SMO).  
  
Pour ajouter des étapes de travail, des planifications, des alertes et des notifications à envoyer aux opérateurs, utilisez les liens d'accès aux rubriques figurant dans la section Voir aussi.  
  
-   **Avant de commencer :**  
  
    [Limitations et restrictions](#Restrictions)  
  
    [Sécurité](#Security)  
  
-   **Pour créer un travail, utilisez :**  
  
    [SQL Server Management Studio](#SSMSProcedure),  
  
    [Transact-SQL](#TsqlProcedure)  
  
    [SQL Server Management Objects](#SMOProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Pour créer un travail, l'utilisateur doit être membre de l'un des rôles de base de données fixes de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du rôle de serveur fixe **sysadmin** . Un travail ne peut être modifié que par son propriétaire ou par les membres du rôle **sysadmin** . Pour plus d’informations sur les rôles de base de données fixe de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, consultez [Rôles de base de données fixe de SQL Server Agent](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
-   L'attribution d'un travail à une autre connexion ne garantit pas que le nouveau propriétaire dispose des autorisations nécessaires pour exécuter le travail.  
  
-   Les travaux locaux sont mis en cache par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local. Ainsi, toutes les modifications obligent implicitement l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à remettre le travail dans le cache. Étant donné que l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne met pas le travail en mémoire cache tant que la procédure stockée **sp_add_jobserver** n’est pas appelée, il est plus efficace d’appeler **sp_add_jobserver** en dernier.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
-   Vous devez être administrateur système pour modifier le propriétaire d'un travail.  
  
-   Pour des raisons de sécurité, seul le propriétaire du travail ou un membre du rôle **sysadmin** peut modifier la définition du travail. Seuls les membres du rôle serveur fixe **sysadmin** peuvent attribuer la propriété du travail à d'autres utilisateurs et peuvent exécuter n'importe quel travail, quel qu'en soit le propriétaire.  
  
    > [!NOTE]  
    > Si vous transférez la propriété d’un travail à un utilisateur qui n’est pas membre du rôle serveur fixe **sysadmin** et que ce travail exécute des étapes qui nécessitent des comptes proxy (par exemple l’exécution de packages [!INCLUDE[ssIS](../../includes/ssis_md.md)] ), vérifiez que l’utilisateur en question a accès à ce compte proxy, sinon le travail échouera.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-job"></a>Pour créer un travail de l'Agent SQL Server  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer un travail SQL Server Agent.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez avec le bouton droit sur le dossier **Travaux** et sélectionnez **Nouveau travail...** .  
  
4.  Dans la boîte de dialogue **Nouveau travail** , sur la page **Général** , modifiez les propriétés générales du travail. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Général&#41;](../../ssms/agent/job-properties-new-job-general-page.md).  
  
5.  Sur la page **Étapes** , organisez les étapes de travail. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Étapes&#41;](../../ssms/agent/job-properties-new-job-steps-page.md).  
  
6.  Sur la page **Planifications** , organisez les planifications du travail. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Planifications&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md).  
  
7.  Sur la page **Alertes** , organisez les alertes pour le travail. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Alertes&#41;](../../ssms/agent/job-properties-new-job-alerts-page.md).  
  
8.  Sur la page **Notifications** , définissez des actions que l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devra exécuter une fois le travail terminé. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Notifications&#41;](../../ssms/agent/job-properties-new-job-notifications-page.md).  
  
9. Sur la page **Cibles** , gérez les serveurs cibles pour le travail. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Cibles&#41;](../../ssms/agent/job-properties-new-job-targets-page.md).  
  
10. Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-job"></a>Pour créer un travail de l'Agent SQL Server  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backup';  
    GO  
    ```  
  
Pour plus d'informations, consultez les pages suivantes :  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProcedure"></a>Utilisation de SQL Server Management Objects  
**Pour créer un travail de l'Agent SQL Server**  
  
Appelez la méthode **Create** de la classe **Job** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell. Pour obtenir un exemple de code, consultez [Planification des tâches administratives automatiques dans l’Agent SQL Server](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md).  
