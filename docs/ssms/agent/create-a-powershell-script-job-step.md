---
description: Create a PowerShell Script Job Step
title: Create a PowerShell Script Job Step
ms.custom: seo-lt-2019
ms.date: 01/20/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], job steps
- jobs [SQL Server Agent], PowerShell
- job steps [PowerShell]
- SQL Server Agent jobs, PowerShell steps
ms.assetid: 50afcf84-fae0-4eb5-9b0f-f2cf144c1433
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2231dbd11ae198d2dac6f44e5c23f2e2ec717305
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338805"
---
# <a name="create-a-powershell-script-job-step"></a>Create a PowerShell Script Job Step
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer et définir une étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécute un script PowerShell dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  

[!INCLUDE[Freshness](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-powershell-script-job-step"></a>Pour créer une étape de travail exécutant un script PowerShell  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **SQL Server Agent**, créez un travail ou cliquez avec le bouton droit de la souris sur un travail existant, puis cliquez sur **Propriétés**. Pour plus d'informations sur la création d'un travail, consultez [Création de travaux](../../ssms/agent/create-jobs.md).  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , cliquez sur la page **Étapes** , puis sur **Nouveau**.  
  
4.  Dans la boîte de dialogue **Nouvelle étape du travail** , tapez un **nom d'étape** de travail.  
  
5.  Dans la liste **Type** , cliquez sur **PowerShell**.  
  
6.  Dans la liste **Exécuter en tant que** , sélectionnez le compte proxy avec les informations d'identification que le travail utilisera.  
  
7.  Dans la zone **Commande** , tapez la syntaxe du script PowerShell qui sera exécuté pour l'étape de travail. Vous pouvez aussi cliquer sur **Ouvrir** et sélectionner un fichier contenant la syntaxe du script. Pour obtenir un exemple de script PowerShell, consultez **Utilisation de Transact-SQL** ci-dessous.  
  
8.  Cliquez sur la page **Avancé** pour paramétrer les options suivantes pour l'étape de travail : l'action à exécuter si l'étape de travail échoue ou réussit, le nombre de tentatives d'exécution de l'étape de travail que doit effectuer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et la fréquence de ces tentatives.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-a-powershell-script-job-step"></a>Pour créer une étape de travail exécutant un script PowerShell  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- creates a PowerShell job step that finds the processes
    -- that use more than 1000 MB of memory and kills them  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Kills all processes that use more than 1000 MB of memory',  
        @subsystem = N'PowerShell',  
        @command = N'Get-Process | Where-Object { $_.WS -gt 1000MB } | Stop-Process',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour créer une étape de travail exécutant un script PowerShell**  
  
Utilisez la classe **JobStep** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell.  
