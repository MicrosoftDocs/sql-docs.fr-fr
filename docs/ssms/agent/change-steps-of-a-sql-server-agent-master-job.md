---
description: Modifier les étapes d'un travail maître SQL Server Agent
title: Modifier les étapes d'un travail maître SQL Server Agent
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 8f1a0ee6-49ff-4080-94ca-d661daeff2a6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e4dc7a4bfea99917792166172e1577a8b7850489
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766213"
---
# <a name="change-steps-of-a-sql-server-agent-master-job"></a>Modifier les étapes d'un travail maître SQL Server Agent
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment modifier les étapes d'un travail maître SQL Server Agent dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l 'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
Un travail maître [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne peut pas être ciblé sur des serveurs locaux et distants à la fois.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Vous pouvez modifier uniquement les travaux dont vous êtes propriétaire, à moins d'être membre du rôle de serveur fixe **sysadmin** . Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-make-changes-to-the-steps-of-a-sql-server-agent-master-job"></a>Pour modifier les étapes d'un travail maître SQL Server Agent  
  
1.  Dans l' **Explorateur d'objets** , cliquez sur le signe plus (+) pour développer le serveur qui contient le travail dont vous souhaitez modifier les étapes.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier **Travaux** .  
  
4.  Cliquez avec le bouton droit sur le travail dont vous voulez modifier les étapes, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du travail -** _nom\_travail_, sous **Sélectionner une page**, sélectionnez **Étapes**.  
  
6.  Cliquez sur **Modifier** pour ouvrir la boîte de dialogue **Propriétés de l’étape du travail -** _nom\_étape\_travail_. Pour plus d’informations sur les options disponibles dans cette boîte de dialogue, consultez [Propriétés de l’étape du travail - Nouvelle étape du travail &#40;page Général&#41;](../../ssms/agent/job-step-properties-new-job-step-general-page.md) et [Propriétés de l’étape du travail - Nouvelle étape du travail &#40;page Avancé&#41;](../../ssms/agent/job-step-properties-new-job-step-advanced-page.md).  
  
7.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
8.  Dans la boîte de dialogue **Propriétés du travail -** _nom\_travail_, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-make-changes-to-the-steps-of-a-sql-server-agent-master-job"></a>Pour modifier les étapes d'un travail maître SQL Server Agent  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- changes the number of retry attempts for the first step
    -- of the Weekly Sales Data Backup job.   
    -- After running this example, the number of retry attempts is 10   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_update_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_id = 1,  
        @retry_attempts = 10 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_update_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md).  
