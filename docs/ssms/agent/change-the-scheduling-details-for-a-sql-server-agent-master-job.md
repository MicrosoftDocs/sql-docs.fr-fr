---
description: Change the Scheduling Details for a SQL Server Agent Master Job
title: Modifier les détails de planification d’un travail principal
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: f5414451-4d8e-464b-bd9e-f2b70c6899b3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: a49821ba65ede149baf2ce4aeaa54724dd796421
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765836"
---
# <a name="change-the-scheduling-details-for-a-sql-server-agent-master-job"></a>Change the Scheduling Details for a SQL Server Agent Master Job

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment modifier les informations de planification pour une définition de travail dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
Un travail maître [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne peut pas être ciblé sur des serveurs locaux et distants à la fois.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Vous pouvez modifier uniquement les travaux dont vous êtes propriétaire, à moins d'être membre du rôle de serveur fixe **sysadmin** . Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-change-the-scheduling-details-for-a-job-definition"></a>Pour modifier les détails de planification d'une définition de travail  
  
1. Dans l' **Explorateur d'objets** , cliquez sur le signe plus (+) pour développer le serveur qui contient le travail dont vous souhaitez modifier la planification.  
  
2. Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3. Cliquez sur le signe plus (+) pour développer le dossier **Travaux** .  
  
4. Cliquez avec le bouton droit sur le travail dont vous souhaitez modifier la planification, puis sélectionnez **Propriétés**.  
  
5. Dans la boîte de dialogue **Propriétés du travail -**_nom\_travail_, sous **Sélectionner une page**, sélectionnez **Planifications**. Pour plus d’informations sur les options disponibles dans cette page, consultez [Propriétés du travail - Nouveau travail &#40;page Planifications&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md).  
  
6. Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-change-the-scheduling-details-for-a-job-definition"></a>Pour modifier les détails de planification d'une définition de travail
  
1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- changes the enabled status of the NightlyJobs schedule to 0
    -- and sets the owner to terrid.
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_update_schedule  
        @name = 'NightlyJobs',  
        @enabled = 0,  
        @owner_login_name = 'terrid' ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md).